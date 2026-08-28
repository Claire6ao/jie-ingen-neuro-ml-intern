# W03 Methodology Note — From EEG CSP to Robot Sensor Classification

## 1. CSP in EEG Motor-Imagery Analysis

Common Spatial Patterns (CSP) is a supervised feature-extraction method commonly used for binary classification of multichannel time-series signals such as motor-imagery EEG. In EEG, each channel corresponds to an electrode measuring neural activity at a different scalp location. For a two-class task, such as left-hand versus right-hand motor imagery, CSP learns linear combinations of the electrode signals whose variances differ as strongly as possible between the two conditions.

For each trial or time window, the multichannel signal can be represented as a matrix $X \in \mathbb{R}^{C \times T}$, where $C$ is the number of channels and $T$ is the number of time samples. A normalized covariance matrix is computed for each window:

$$
C = \frac{XX^T}{\mathrm{trace}(XX^T)}
$$

The covariance matrices are averaged separately within the two classes to obtain $R_1$ and $R_2$. CSP then solves the generalized eigenvalue problem:

$$
R_1 w = \lambda R_2 w
$$

The eigenvectors define spatial filters. Directions associated with large and small generalized eigenvalues emphasize variance patterns that differ between the two classes. After applying the filters,

$$
Z = WX
$$

the variance of each projected component is calculated and converted to normalized log-variance features. These features provide a compact representation that can then be passed to a classifier such as Linear Discriminant Analysis (LDA).

The important idea is that CSP is not simply selecting individual channels. Instead, it learns combinations of channels based on class-dependent covariance and variance structure.

## 2. Transferring CSP to Robot Sensor Data

The Week 3 feature pipeline applies the same mathematical idea to the high-frequency synthetic robot sensor data generated in Week 2. The binary task uses **PATROL versus FAULT**. The dataset contains 20 independent 30-second windows for each class, sampled at 20 Hz. Each window therefore contains 600 time samples. Five simultaneous sensor channels are used: **motor current, accel_x, accel_y, accel_z, and IMU magnitude**. The resulting CSP input has shape $40 \times 5 \times 600$: 40 windows, five channels, and 600 samples per window.

The major conceptual change from EEG is the physical meaning of a channel. In EEG, channels are electrodes measuring the same type of neural signal at different spatial locations. In the robot dataset, channels are heterogeneous sensor measurements with different physical meanings and numerical scales. For this reason, the robot channels are standardized before covariance estimation so that a channel does not dominate the analysis simply because of its units or magnitude.

The CSP implementation was constructed from first principles with NumPy. Each 30-second sensor window was mean-centered, converted to a trace-normalized covariance matrix, and averaged within PATROL and FAULT to obtain the two class covariance matrices. The generalized eigenvalue problem produced eigenvalues of approximately **4.02, 1.29, 1.15, 0.19, and 0.12** in the full feature-analysis notebook. Filters from both ends of the eigenvalue spectrum were retained, producing four CSP components.

The learned filters combined information across multiple robot sensors. For example, the first CSP filter placed its largest weight on motor current, while filters near the opposite end of the eigenvalue spectrum placed stronger weights on accel_z and IMU magnitude. The class-average CSP features also differed substantially. Mean CSP_1 was approximately **−0.61 for PATROL** and **−1.51 for FAULT**, while mean CSP_3 was approximately **−3.60 for PATROL** and **−1.44 for FAULT**. These differences show that CSP concentrated class-dependent variance information into a small number of projected dimensions.

This transfer preserves the mathematical role of CSP while changing its interpretation. In EEG, CSP identifies discriminative covariance patterns across electrodes. In this robot application, it identifies discriminative covariance patterns across sensor channels. The method therefore generalizes from neural spatial filtering to a broader multichannel sensor-fusion setting, although the resulting robot filters should not be interpreted as literal physiological or spatial maps.

## 3. Classification Results and Comparison

The classification notebook evaluated whether the CSP representation was useful for predicting PATROL versus FAULT. The 40 independent windows were divided using a stratified **80/20 train-test split**, giving 32 training windows and eight held-out test windows, with equal class representation in both sets. To avoid data leakage, channel standardization and CSP filters were fitted using only the training windows and were then applied unchanged to the test windows.

Three approaches were compared:

1. a constant majority-class baseline;
2. Logistic Regression using raw sensor log-variance features; and
3. LDA using CSP log-variance features.

The constant baseline achieved **0.50 accuracy**, **0.50 precision**, **1.00 recall**, and **0.667 F1**. Because the training set was exactly balanced, there was no unique majority class; the classifier consistently selected FAULT in the tied case. It therefore classified all four FAULT test windows correctly but misclassified all four PATROL windows.

Both stronger models achieved perfect performance on the eight held-out windows. **Raw Features + Logistic Regression** achieved accuracy, precision, recall, and F1 of **1.00**, and **CSP + LDA** also achieved **1.00** for all four metrics. Their confusion matrices contained four correctly classified PATROL windows and four correctly classified FAULT windows, with no false positives or false negatives.

These results show that the CSP-derived features were sufficient to make PATROL and FAULT linearly separable for this split. However, CSP did **not** improve classification performance over the raw-feature Logistic Regression baseline. The synthetic PATROL and FAULT signals were already strongly distinguishable through their original channel-level variance characteristics, allowing the simpler raw-feature model to reach the same test performance.

Therefore, the main value of CSP in this experiment is not an observed increase in test accuracy, but the successful transfer of an EEG-inspired covariance-based representation to robot telemetry. CSP compressed multichannel relationships into discriminative components and provided an interpretable bridge between EEG spatial filtering and robot sensor fusion. At the same time, the experiment highlights an important methodological limitation: a more sophisticated feature transformation is not necessarily beneficial when simpler features already separate the classes.

Finally, the evaluation is a small proof of concept. Only 40 independent windows were available, and the test set contained eight windows. Perfect performance on this synthetic dataset should therefore not be interpreted as evidence of robust real-world generalization. A larger dataset with more heterogeneous operating conditions, noise, unit-to-unit variability, and less strongly separated failure patterns would be needed to determine whether CSP offers a consistent advantage over simpler raw-feature classifiers.

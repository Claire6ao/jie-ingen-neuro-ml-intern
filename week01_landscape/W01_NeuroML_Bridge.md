# Week 1 — Neuro-ML Methodology Bridge

## 1. Purpose

This document maps methods from my prior neuroscience, neural-signal, and embedded-control projects to analogous computational problems in physical AI.

The goal is not to claim that biological neural signals and robot sensor signals are physically equivalent. EEG electrodes, LFP recordings, IMUs, motor-current sensors, cameras, LiDAR, and other robotic sensors measure fundamentally different physical quantities.

Instead, the bridge focuses on cases where the two domains share the same or closely related **mathematical operations**.

The general methodological structure is:

$$\text{multichannel signals} \rightarrow \text{signal processing} \rightarrow \text{feature representation} \rightarrow \text{classification} \rightarrow \text{state estimate}$$

The eight mappings below identify:

1. A specific method or operation from my prior neuroscience or engineering work.
2. Its physical-AI analogue.
3. The mathematical structure shared by the two problems.
4. Why the mapping is methodologically valid.

---

## 2. Mapping 1 — EEG Filtering and PSD → Robot Sensor Signal Analysis

### Prior Neuroscience Method

In my EEG projects, neural recordings were represented as multichannel time-series signals and processed before classification.

One important operation is frequency-domain analysis using the Power Spectral Density (PSD).

For a signal $x(t)$, the PSD describes how signal power is distributed across frequency:

$$x(t) \rightarrow P_{xx}(f)$$

Using Welch's method, the signal is divided into overlapping windows and the resulting periodograms are averaged:

$$P_{xx}(f)=\frac{1}{K}\sum_{k=1}^{K}P_{xx}^{(k)}(f)$$

Frequency-band energy can then be extracted:

$$E_{[f_1,f_2]}=\int_{f_1}^{f_2}P_{xx}(f)\,df$$

In EEG, this produces features associated with frequency ranges such as Delta, Theta, Alpha, Beta, and Gamma.

### Physical-AI Analogue

Aido Rover and Sentinel also generate continuous time-series measurements.

Examples include:

- IMU acceleration
- Motor current
- RSSI
- Proximity measurements
- Acoustic signals
- Vibration signals

The same signal-processing pipeline can be applied:

$$s(t) \rightarrow P_{ss}(f) \rightarrow E_{[f_1,f_2]}$$

For example, changes in motor-current or IMU frequency content may provide information about different operational states or abnormal behavior.

### Shared Mathematical Structure

The mathematical operation is identical:

$$\text{time-domain signal} \rightarrow \text{frequency-domain representation} \rightarrow \text{band-energy features}$$

The physical interpretation of the frequencies changes, but the Fourier and PSD operations do not.

### Why the Mapping Is Valid

PSD estimation is a signal-processing operation independent of whether the input represents neural voltage, mechanical vibration, motor current, or another physical signal.

Therefore, EEG filtering and spectral analysis provide a direct methodological foundation for exploratory analysis of Aido Rover and Sentinel sensor streams.

---

## 3. Mapping 2 — EEG CSP → Aido Rover Multichannel Spatial Filtering

### Prior Neuroscience Method

In my EEG motor-imagery project, Common Spatial Patterns (CSP) was used to extract discriminative information from multichannel EEG.

Suppose the class-specific covariance matrices are:

$$R_1$$

and

$$R_2$$

CSP solves the generalized eigenvalue problem:

$$R_1w=\lambda R_2w$$

The resulting spatial filter $w$ defines a projection:

$$z(t)=w^Tx(t)$$

The goal is to identify linear combinations of EEG channels whose variance differs strongly between two motor-imagery conditions.

### Physical-AI Analogue

Aido Rover also produces multiple correlated sensor channels.

A Rover observation may contain signals such as:

$$X_{\text{Rover}}=
\left[
x_{\text{IMU}},
x_{\text{motor}},
x_{\text{proximity}},
x_{\text{RSSI}},
\ldots
\right]
$$

Instead of scalp electrodes, each channel represents a different physical sensor stream.

For two operational states, class-specific covariance matrices can again be constructed:

$$R_{\text{state 1}},\;R_{\text{state 2}}$$

and the same generalized eigenvalue problem can be solved:

$$R_{\text{state 1}}w=\lambda R_{\text{state 2}}w$$

### Shared Mathematical Structure

The correspondence is:

$$\text{EEG channels} \leftrightarrow \text{Rover sensor channels}$$

and:

$$\text{motor-imagery classes} \leftrightarrow \text{Rover operational states}$$

Both problems use class-dependent covariance structure to construct discriminative linear projections.

### Why the Mapping Is Valid

CSP operates on covariance matrices rather than on the biological identity of EEG electrodes.

Therefore, if Rover sensor channels contain class-dependent covariance patterns, the same generalized eigenvalue decomposition can be applied.

The physical meaning of the channels changes, but the mathematical operation:

$$R_1w=\lambda R_2w$$

remains unchanged.

---

## 4. Mapping 3 — EEG LDA → Aido Rover Operational-Mode Classification

### Prior Neuroscience Method

After CSP feature extraction in EEG motor-imagery classification, Linear Discriminant Analysis (LDA) can be used to classify the resulting feature vectors.

For two classes, LDA seeks a projection that maximizes separation between class means relative to within-class variance.

A simplified discriminant direction can be represented as:

$$w \propto S_W^{-1}(\mu_1-\mu_2)$$

where:

- $S_W$ is the within-class scatter matrix,
- $\mu_1$ and $\mu_2$ are the class means.

The EEG pipeline is:

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow z \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{motor imagery}}$$

### Physical-AI Analogue

For Aido Rover, CSP-derived features can instead represent patterns across robot telemetry channels.

The corresponding pipeline becomes:

$$X_{\text{Rover}} \rightarrow \text{CSP} \rightarrow z \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{operational mode}}$$

Representative operational modes may include:

- PATROL
- ALERT
- CHARGING
- FAULT

### Shared Mathematical Structure

In both domains, LDA receives a feature vector:

$$z \in \mathbb{R}^{d}$$

and attempts to separate labeled classes in feature space.

The optimization problem depends on between-class and within-class statistics, not on whether the original measurements came from EEG electrodes or robot sensors.

### Why the Mapping Is Valid

This is a direct domain transfer.

In EEG:

$$\text{CSP features} \rightarrow \text{motor-state class}$$

In Rover:

$$\text{CSP features} \rightarrow \text{operational-state class}$$

The input semantics and output labels change, but the LDA classification operation remains the same.

---

## 5. Mapping 4 — LFP vs. Calcium Imaging → Robot Sensor Modality Comparison

### Prior Neuroscience Method

In my reaching-behavior project, I compared different neural recording modalities to determine which provided more predictive information about behavior.

The central question was:

> Which neural modality better predicts reaching behavior?

For example, separate models can be trained using:

$$X_{\text{LFP}} \rightarrow f_{\theta} \rightarrow \hat{y}$$

and:

$$X_{\text{calcium}} \rightarrow f_{\theta} \rightarrow \hat{y}$$

The models can then be compared using the same evaluation metric:

$$M_{\text{LFP}} \quad \text{vs.} \quad M_{\text{calcium}}$$

where $M$ may represent accuracy, F1 score, ROC-AUC, or another predictive-performance metric.

### Physical-AI Analogue

The same methodology can be applied to Aido Rover sensor modalities.

Instead of asking which neural modality is most predictive, the physical-AI question becomes:

> Which robot sensor modality provides the most information about operational state?

For example:

$$X_{\text{IMU}} \rightarrow f_{\theta} \rightarrow \hat{y}_{\text{state}}$$

$$X_{\text{motor current}} \rightarrow f_{\theta} \rightarrow \hat{y}_{\text{state}}$$

$$X_{\text{proximity/RSSI}} \rightarrow f_{\theta} \rightarrow \hat{y}_{\text{state}}$$

The resulting models can then be compared under the same evaluation framework.

### Shared Mathematical Structure

The common operation is a controlled modality benchmark:

$$X^{(m)} \rightarrow f_{\theta}^{(m)} \rightarrow \hat{y}$$

followed by:

$$M_1,M_2,\ldots,M_k$$

for different modalities $m$.

### Why the Mapping Is Valid

The scientific question is identical:

> How much predictive information about the target state is contained in each sensing modality?

The biological interpretation of LFP and calcium imaging differs from the physical interpretation of IMU or motor-current measurements, but the experimental comparison is structurally the same.

This mapping provides the methodological foundation for comparing robot sensor modalities in later weeks.

---

## 6. Mapping 5 — EEG Frequency Bands → Robot Sensor Frequency Bands

### Prior Neuroscience Method

In my EEG confusion-prediction project, frequency-related features were used to represent neural activity.

EEG signals can be decomposed into frequency ranges such as:

- Delta
- Theta
- Alpha
- Beta
- Gamma

The energy in a frequency range can be represented as:

$$E_{\text{band}}=\int_{f_1}^{f_2}P_{xx}(f)\,df$$

This converts a long time-series signal into a smaller feature vector:

$$x(t) \rightarrow [E_1,E_2,\ldots,E_k]$$

These features can then be used for cognitive-state classification.

### Physical-AI Analogue

Robot sensors may also contain meaningful frequency-dependent behavior.

For example, IMU or motor-current signals can be divided into low-, middle-, and high-frequency ranges:

$$s(t) \rightarrow [E_{\text{low}},E_{\text{mid}},E_{\text{high}}]$$

Different operational states may produce different spectral-energy distributions.

For example, normal movement and abnormal mechanical vibration may differ in their frequency content.

### Shared Mathematical Structure

Both pipelines calculate:

$$E_{[f_1,f_2]}=\int_{f_1}^{f_2}P(f)\,df$$

The operation is therefore mathematically identical.

### Why the Mapping Is Valid

The mapping does **not** imply that robot sensors have biological Delta, Theta, or Alpha rhythms.

Instead, the transferable concept is **frequency-band energy extraction**.

EEG bands have neurophysiological interpretations, while robot frequency bands have mechanical or operational interpretations.

The mathematical feature-extraction operation remains the same.

---

## 7. Mapping 6 — Shallow EEG Neural Network → Robot Sensor Classifier

### Prior Neuroscience Method

In my EEG confusion-prediction project, extracted EEG features were used as inputs to machine-learning classifiers, including a shallow neural network.

The general supervised-learning pipeline can be represented as:

$$\mathbf{x}_{\text{EEG}} \rightarrow f_{\theta}(\mathbf{x}) \rightarrow \hat{y}_{\text{cognitive state}}$$

For a neural network layer:

$$h=\sigma(Wx+b)$$

and the final layer produces a class prediction or class probability.

### Physical-AI Analogue

The same model structure can be applied to features extracted from Aido Rover or Sentinel sensor data:

$$\mathbf{x}_{\text{sensor}} \rightarrow f_{\theta}(\mathbf{x}) \rightarrow \hat{y}_{\text{operational state}}$$

For Rover, the target may represent operational mode.

For Sentinel, the target may represent a normal or anomalous environmental state.

### Shared Mathematical Structure

Both are supervised classification problems:

$$f_{\theta}:\mathbb{R}^{d}\rightarrow\mathcal{Y}$$

where the input is a feature vector and the output is a state label.

The model learns parameters $\theta$ by minimizing a classification loss.

For example:

$$\theta^*=\arg\min_{\theta}\sum_i L\left(f_{\theta}(x_i),y_i\right)$$

### Why the Mapping Is Valid

The neural network does not depend on the biological identity of EEG features.

Once both domains are represented as feature vectors and labeled states, the same network architecture, loss function, training procedure, and evaluation metrics can be used.

Therefore:

$$\text{EEG feature classifier} \rightarrow \text{robot sensor classifier}$$

is a direct methodological transfer.

---

## 8. Mapping 7 — EEG Motor Imagery → Aido Humanoid Motion-State Classification

### Prior Neuroscience Method

My EEG motor-imagery project classified neural patterns associated with different imagined motor states, such as left- versus right-hand motor imagery.

The pipeline can be represented as:

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{left/right}}$$

The important feature of this problem is that a motor state is inferred from a spatial pattern distributed across multiple EEG channels.

### Physical-AI Analogue

Aido Humanoid can also be represented using spatially distributed motion-related sensor channels, such as:

- Joint-angle measurements
- IMU signals
- Other proprioceptive measurements

Instead of predicting left- versus right-hand motor imagery, the target becomes a robot motion primitive such as:

- WALK
- REACH
- BALANCE

The analogous pipeline is:

$$X_{\text{joint/IMU}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{motion primitive}}$$

### Shared Mathematical Structure

The mapping is:

$$\text{EEG scalp channels} \leftrightarrow \text{Aido Humanoid joint/IMU channels}$$

$$\text{motor-imagery states} \leftrightarrow \text{robot motion states}$$

$$\text{CSP+LDA classification} \leftrightarrow \text{CSP+LDA classification}$$

Both problems attempt to identify a motor-related state from a multichannel spatial pattern.

### Why the Mapping Is Valid

The connection is more specific than simply stating that both systems involve movement.

In both cases:

1. Multiple channels describe a distributed system.
2. Different motor states produce different multichannel covariance patterns.
3. CSP can construct discriminative projections from those covariance differences.
4. LDA can classify the resulting representation.

Therefore, the EEG motor-imagery pipeline provides a direct methodological bridge to Aido Humanoid motion-state classification.

---

## 9. Mapping 8 — Bionic Prosthetic Arm → Aido Humanoid Embedded Control Architecture

### Prior Engineering Project

My bionic prosthetic arm project provides a different type of bridge to physical AI.

Unlike the previous mappings, which primarily transfer statistical or machine-learning methods, this project provides experience with an embodied control architecture.

The system combined:

- Mechanical structure
- Arduino-based embedded control
- Physical actuation
- A Java Android application for user control

Its basic architecture can be represented as:

$$\text{user input} \rightarrow \text{embedded controller} \rightarrow \text{actuator command} \rightarrow \text{physical motion}$$

### Physical-AI Analogue

Aido Humanoid operates at a substantially greater level of complexity, but it follows the same broad embodied-system principle:

$$\text{sensor state} \rightarrow \text{processing / control} \rightarrow \text{actuation} \rightarrow \text{robot motion}$$

A humanoid must integrate sensing, state estimation, motion planning, embedded control, and multi-degree-of-freedom actuation.

### Shared Architectural Structure

The correspondence is:

$$\text{input} \rightarrow \text{computation} \rightarrow \text{control signal} \rightarrow \text{physical actuation}$$

The bionic arm therefore provides intuition for the transition from software output to physical motion.

### Why the Mapping Is Valid

This mapping is architectural rather than a claim that the prosthetic-arm controller and Aido Humanoid use the same algorithms.

The shared structure is the integration of:

- Software
- Embedded computation
- Mechanical systems
- Actuators
- Physical feedback

This experience is especially relevant to Aido Humanoid because it demonstrates how computational decisions ultimately have to be translated into commands that produce physical movement.

---

## 10. Summary of the Eight Neuro-ML Mappings

| # | Prior Project / Method | Specific Operation | Physical-AI Analogue | Primary Platform |
|---|---|---|---|---|
| 1 | EEG filtering and PSD | $x(t) \rightarrow P_{xx}(f)$ | Robot time-series spectral analysis | Aido Rover / Sentinel |
| 2 | EEG CSP | $R_1w=\lambda R_2w$ | Multichannel robot spatial filtering | Aido Rover |
| 3 | EEG LDA | $w \propto S_W^{-1}(\mu_1-\mu_2)$ | Operational-mode classification | Aido Rover |
| 4 | LFP vs. calcium comparison | $X^{(m)} \rightarrow f_{\theta} \rightarrow M_m$ | Sensor-modality benchmark | Aido Rover |
| 5 | EEG frequency bands | $E=\int P(f)\,df$ | Robot sensor band-energy features | Aido Rover / Sentinel |
| 6 | EEG shallow neural network | $x \rightarrow f_{\theta}(x) \rightarrow \hat{y}$ | Robot sensor state classifier | Aido Rover / Sentinel |
| 7 | Motor imagery classification | CSP + LDA | Motion-primitive classification | Aido Humanoid |
| 8 | Bionic prosthetic arm | Input → controller → actuator | Embedded physical-AI control architecture | Aido Humanoid |

---

## 11. Overall Methodological Bridge

The eight mappings show that the relationship between my prior neuroscience work and physical AI is not based on treating neural and robotic signals as physically equivalent.

Instead, the connection comes from shared mathematical and computational structures.

### EEG Motor Imagery → Aido Rover

The strongest direct algorithmic transfer is:

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{motor imagery}}$$

becoming:

$$X_{\text{Rover}} \rightarrow \text{CSP} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{operational mode}}$$

The sensor channels change, but the covariance analysis, generalized eigenvalue decomposition, feature extraction, and classification structure remain the same.

### EEG Confusion Prediction → Rover and Sentinel

The EEG confusion project contributes a second methodological pathway:

$$\text{time series} \rightarrow \text{frequency features} \rightarrow \text{classifier} \rightarrow \text{state}$$

This transfers naturally to continuous physical sensor streams used for operational-state or anomaly classification.

### LFP vs. Calcium → Robot Sensor Benchmarking

The LFP-versus-calcium comparison contributes the experimental question:

$$\text{Which modality contains the most predictive information?}$$

This becomes a robot-sensor benchmarking problem comparing IMU, motor current, proximity, RSSI, and other sensing modalities.

### Motor Imagery + Bionic Prosthetic Arm → Aido Humanoid

The Aido Humanoid bridge contains two complementary levels.

The EEG motor-imagery project provides the **mathematical classification analogue**:

$$\text{multichannel signals} \rightarrow \text{motor-state classification}$$

while the bionic prosthetic arm provides the **embodied control analogue**:

$$\text{computational decision} \rightarrow \text{control} \rightarrow \text{physical actuation}$$

Together, these projects connect neural decoding with physical-AI sensing and control.

---

## 12. Key Takeaway

Across all eight mappings, the central methodological principle is:

$$\boxed{\text{different physical signals} + \text{shared mathematical structure} = \text{transferable methodology}}$$

The same mathematical tools can operate on signals with very different physical meanings when the underlying computational problem is structurally similar.

The internship therefore extends methods I have previously used for neural signals into physical-AI problems through a progression of:

$$\text{signal analysis} \rightarrow \text{feature extraction} \rightarrow \text{state classification} \rightarrow \text{sensor comparison} \rightarrow \text{embodied control}$$

This methodological bridge provides the foundation for the later Aido Rover, Sentinel, and Aido Humanoid analyses.
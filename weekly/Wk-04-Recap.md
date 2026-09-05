# Week 4 Recap — Multi-Modal Sensor Benchmark

## Overview

This week, I developed a multi-modal sensor benchmark to compare how different
robot sensor modalities perform in operational-state classification. The main
task remained **PATROL vs FAULT**, maintaining consistency with the CSP + LDA
pipeline developed in Week 3.

The benchmark reused the Week 2 robot telemetry datasets and compared five
sensor modality conditions:

- IMU-only
- Motor-current-only
- Proximity + RSSI
- Battery SoC + task success
- All sensors combined

## Benchmark Pipeline

For a fair comparison, all modalities used the same temporal windows, labels,
train/test split, and cross-validation procedure. The main benchmark used the
5-minute fleet telemetry because it contains all sensor modalities required
for the comparison.

I evaluated each modality using five classifiers:

1. Logistic Regression
2. Random Forest
3. Shallow MLP (64 → 32 hidden units with ReLU)
4. CSP + LDA
5. Gaussian Naive Bayes

A single stratified 80/20 train/test split was shared across all experiments.
Within the training set, I used **5-fold stratified cross-validation** and
reported mean Accuracy and macro-F1 with their standard deviations. Macro-F1
was particularly useful because PATROL and FAULT were not equally represented.

For CSP + LDA, I adapted the Week 3 pipeline by fitting channel scaling and CSP
filters only on the training portion of each fold to avoid data leakage.

## Results and Interpretation

The benchmark showed that **All-sensors** and **Motor-current-only** achieved
the strongest cross-validation performance and were effectively tied at the
top of the modality ranking. This suggests that motor current contains a
particularly strong signal for distinguishing PATROL from FAULT in the
synthetic telemetry.

I also applied **permutation importance** on the held-out test set to identify
which individual measurements contributed most strongly to classification.
The feature-importance results further supported the importance of
motor-current information.

The near-perfect performance should be interpreted in the context of the
synthetic dataset. PATROL and FAULT were generated with distinct sensor
signatures, so these results demonstrate that the benchmark pipeline can
recover informative sensor-state relationships rather than guaranteeing the
same performance on real-world robot telemetry.

## Key Takeaways

Week 4 extended the Week 3 CSP + LDA work into a broader machine-learning
benchmark and demonstrated how sensor modality selection affects operational-
state prediction. I also learned that sampling rate must be considered when
comparing modalities: the Week 2 high-frequency band-energy features are valid
for the 20 Hz dataset, but cannot be directly applied to the 5-minute fleet
telemetry. This benchmark provides the baseline for the Week 5 neural-network
comparison and tuning.
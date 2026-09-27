# Week 7 Recap — Robot Sensor Anomaly Detection

## Overview

This week, I extended the Week 2 synthetic robot sensor dataset into an anomaly-detection benchmark inspired by the EEG confusion-prediction project. The main idea was to treat abnormal robot operational states similarly to abnormal cognitive states: deviations from normal sensor patterns can be represented through frequency-domain features and detected using machine-learning methods.

I injected five controlled fault types into the 20 Hz high-frequency sensor stream: **motor stall, proximity sensor drift, accelerated battery degradation, IMU axis failure, and RSSI disruption**. I then extracted frequency-band energy features using the Week 2 bands of **0–0.5 Hz, 0.5–2 Hz, and 2–5 Hz** and compared four detection methods across 10 random seeds: Energy Threshold, Isolation Forest, One-Class SVM, and Logistic Regression.

## Hardest Fault to Detect

The most difficult fault to detect was **IMU axis failure**, with a mean AUROC of only **0.603** across the four methods. In comparison, motor stall, RSSI disruption, battery degradation, and proximity drift achieved mean AUROCs of approximately 0.998, 0.982, 0.972, and 0.915, respectively.

One reason IMU failure was more difficult is that the injected failure zeroed only one accelerometer axis while the remaining IMU channels continued to contain normal information. As a result, its overall feature representation changed less dramatically than faults such as motor stall, which simultaneously produced a motor-current spike and velocity drop. Even Logistic Regression, the strongest method for IMU failure, achieved an AUROC of only 0.742.

## Frequency-Band Energy vs. Classical Anomaly Detection

The neuroscience-inspired **Energy Threshold** method achieved an overall AUROC of **0.883**. It outperformed Isolation Forest, which achieved 0.820, but underperformed One-Class SVM, which achieved 0.924. The Energy Threshold method performed particularly well for motor stall, battery degradation, proximity drift, and RSSI disruption, achieving an AUROC of 1.000 for each. However, its AUROC dropped to only 0.417 for IMU failure.

Overall, this suggests that frequency-band energy is an effective and interpretable anomaly signal when a fault produces a strong spectral deviation, but it can struggle with more subtle or localized sensor failures. Logistic Regression produced the strongest overall performance, with an AUROC of **0.948** and F1 score of **0.847**. However, its AUROC improvement over One-Class SVM was not statistically significant in the paired Wilcoxon signed-rank test (**p = 0.105 > 0.05**).

# Week 06 Recap — BCI-to-Humanoid Bridge

## Overview

This week focused on establishing and testing a formal analogy between EEG motor imagery classification and motion-state classification for the Aido Humanoid. The central idea is that both problems can be represented as multichannel time-series classification tasks. In EEG motor imagery, scalp electrodes capture spatial patterns associated with imagined movements, while in the Aido Humanoid, distributed joint sensors capture spatial kinematic patterns associated with robot motion primitives.

## Work Completed

I generated a synthetic Aido Humanoid dataset containing three motion primitives: **WALK, REACH, and BALANCE**. The dataset contained 300 trials, with 100 trials per class, six simulated joint-angle sensor channels, and realistic trial-to-trial noise. Each primitive was designed with a distinct kinematic pattern: WALK emphasized alternating lower-body activity, REACH emphasized upper-body shoulder and elbow activity, and BALANCE represented smaller coordinated postural adjustments.

I then transferred the Week 3 CSP feature-extraction approach to the Aido sensor data. Because the Aido task contains three classes while standard CSP is a binary method, I implemented a **one-vs-rest CSP strategy** for WALK, REACH, and BALANCE. The resulting CSP log-variance features were concatenated and classified using Linear Discriminant Analysis (LDA).

## Results and Comparison

The Aido CSP+LDA pipeline achieved a **test accuracy of 1.000** and a **macro F1 score of 1.000**. The CSP feature plots also showed strong separation among the three motion primitives.

For comparison, the EEG motor imagery CSP+LDA benchmark achieved approximately **77.9% mean 10-fold cross-validation accuracy** across nine subjects for left-vs-right hand motor imagery. The lower separability of the EEG data is reasonable because real EEG contains biological variability, artifacts, measurement noise, and overlapping neural activity. In contrast, the synthetic Aido motion primitives were intentionally generated with distinct spatial covariance patterns, making them particularly suitable for CSP.

## Takeaway

Overall, the BCI-to-humanoid analogy held as well as expected at the signal-processing level. In both cases, CSP identifies class-dependent spatial variance patterns across multiple sensor channels and converts them into features that can be classified with LDA. The results demonstrate that the same general framework can be transferred from scalp EEG channels to humanoid joint sensors.

However, the perfect Aido performance reflects the controlled synthetic dataset rather than expected real-world performance. A more meaningful next step would be to evaluate the pipeline using real Aido joint and IMU recordings, where sensor noise, motion transitions, environmental disturbances, and variability in robot behavior would provide a more challenging test of whether the observed CSP feature separability persists.
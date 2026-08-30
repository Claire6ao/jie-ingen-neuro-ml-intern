# Week 3 Recap — CSP and LDA for Robot Sensor Data

## Overview

This week focused on transferring **Common Spatial Patterns (CSP)**, a spatial
filtering method commonly used for EEG motor-imagery classification, to the
multi-channel robot sensor data developed in Week 2. The main goal was to test
whether class-dependent covariance patterns across robot sensors could provide
a useful representation for distinguishing **PATROL** from **FAULT**.

The analysis used the Week 2 high-frequency synthetic sensor stream. Each
operational mode contained 20 independent 30-second windows sampled at 20 Hz.
Five channels were included: motor current, accel_x, accel_y, accel_z, and IMU
magnitude.

## CSP Feature Separation

CSP was implemented from scratch using NumPy. For each sensor window, a
trace-normalized covariance matrix was computed, and the matrices were averaged
within PATROL and FAULT to obtain the two class-conditional covariance matrices.
The generalized eigenvalue problem was then solved to identify directions that
maximized the relative variance difference between the two modes.

The CSP representation showed clear differences between PATROL and FAULT in the
projected feature space. The generalized eigenvalues ranged from approximately
4.02 to 0.12, indicating strong class-dependent variance differences along some
sensor combinations. The learned filters combined information from multiple
channels, particularly motor current, acceleration, and IMU magnitude.

Compared with the raw sensor log-variance scatter plot, CSP concentrated the
class-dependent information into a smaller number of discriminative dimensions.
However, the raw features were already strongly separable because the synthetic
PATROL and FAULT signals were intentionally generated with distinct variance and
frequency characteristics. Therefore, CSP produced a cleaner covariance-based
representation, but the improvement in practical separability was limited by
the already-easy classification problem.

## Classification and Takeaway

Using an 80/20 train-test split, the majority-class baseline achieved **50%**
accuracy. Logistic Regression on the raw log-variance features achieved
**100% accuracy**, while LDA on the CSP features also achieved **100% accuracy**,
with perfect precision, recall, and F1 on the eight held-out windows.

These results show that CSP-style spatial filtering can generalize from EEG
electrodes to heterogeneous robot sensor channels: the mathematical procedure
remains valid even though the physical meaning of a "channel" changes.
However, CSP did not improve classification accuracy over the raw-feature
baseline in this synthetic dataset. Its main contribution was instead a compact,
covariance-based representation of relationships across sensors. Testing on
larger, noisier, and less separable robot datasets would be necessary to
determine whether this representation provides a stronger practical advantage.
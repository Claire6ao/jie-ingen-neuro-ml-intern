# Week 5 Recap — Shallow Neural Networks vs. Classical Models

## Overview

This week, I extended the robot sensor classification pipeline from classical machine-learning methods to a shallow neural network implemented in PyTorch. The task remained PATROL vs. FAULT classification using the same high-frequency sensor data and 80/20 train-test structure used in the previous weeks.

The shallow NN used window-level statistical features extracted from motor-current and IMU channels. The architecture included two hidden layers with batch normalization, ReLU activation, and dropout. I evaluated 18 hyperparameter combinations across hidden-layer size (32, 64, 128), dropout rate (0.1, 0.3, 0.5), and learning rate (1e-3, 1e-4). All configurations reached a best validation accuracy of 1.000, so the smallest configuration—32 hidden units, 0.1 dropout, and a learning rate of 1e-3—was selected to avoid unnecessary complexity.

## NN vs. CSP+LDA

On the held-out test set, the selected NN achieved 0.875 accuracy and an F1 score of 0.857, while the Week 3 CSP+LDA benchmark achieved perfect classification. Across 10 paired seed re-runs, CSP+LDA achieved a mean accuracy of 1.000 ± 0.000 compared with 0.963 ± 0.060 for the NN.

The paired t-test gave t = -1.964 and p = 0.0811. Since p > 0.05, the difference was not statistically significant. Therefore, the NN did not significantly outperform CSP+LDA. Its additional nonlinear modeling capacity did not translate into better predictive performance on the current dataset.

## Interpretation and Physical AI Implications

The comparison highlights an important trade-off between nonlinear modeling and interpretability. Neural networks can learn more complex relationships between sensor features and may become advantageous when the data contain nonlinear interactions that simpler models cannot capture. However, they introduce additional parameters and make the relationship between sensor signals and predictions less transparent.

CSP+LDA provides a more interpretable pipeline because CSP features are directly related to differences in sensor variance patterns, followed by a simple linear classifier. For the current highly separable PATROL vs. FAULT task, this simpler representation was sufficient.

For physical AI, these results show that increasing model complexity does not automatically improve sensor classification. Model selection should consider not only accuracy, but also stability, interpretability, computational requirements, and the complexity of the underlying sensor data. Shallow NNs remain promising for larger and more complex datasets, but classical approaches can remain highly competitive when the sensor states are already well separated.
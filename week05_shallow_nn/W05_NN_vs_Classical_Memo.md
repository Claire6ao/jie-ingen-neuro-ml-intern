# Week 5 — Neural Network vs. Classical Model Benchmark Memo

## Purpose

This memo compares the Week 5 shallow neural network with the Week 3 CSP+LDA pipeline and the best Week 4 classical classifier for PATROL vs. FAULT classification. The goal is to determine whether the nonlinear neural-network architecture provides a meaningful performance improvement and whether its additional complexity is justified for edge deployment on Sentinel Prime AI and Aido Rover.

## Benchmark Summary

The selected shallow neural network contains two hidden layers with ReLU activation, batch normalization, and dropout regularization. Hyperparameter sensitivity analysis evaluated hidden-layer sizes of 32, 64, and 128 units, dropout rates of 0.1, 0.3, and 0.5, and learning rates of 1e-3 and 1e-4.

All 18 configurations reached a best validation accuracy of 1.000. Because increasing network size did not improve validation performance, the smallest successful configuration was selected: 32 hidden units, 0.1 dropout, and a learning rate of 1e-3. This model contains 1,490 trainable parameters.

On the held-out test set, the shallow NN achieved:

| Metric | Shallow NN |
|---|---:|
| Accuracy | 0.875 |
| Precision | 1.000 |
| Recall | 0.750 |
| F1 | 0.857 |

The model correctly classified all four PATROL windows but misclassified one of four FAULT windows as PATROL. In comparison, the Week 3 CSP+LDA pipeline and the best Week 4 classical classifier both achieved 1.000 test accuracy on the benchmark test set.

These results show that adding nonlinear modeling capacity did not improve predictive performance on the current dataset. The perfect validation accuracy across all NN configurations also suggests that the validation task reached a performance ceiling and that increasing model capacity provides little benefit.

## Statistical Comparison

To evaluate performance beyond a single train-test split, CSP+LDA and the selected shallow NN were compared across 10 paired seed re-runs. Both models used the same stratified 80/20 split for each seed.

CSP+LDA achieved a mean accuracy of **1.000 ± 0.000**, while the shallow NN achieved **0.963 ± 0.060**. The NN matched CSP+LDA in seven of the ten runs and achieved 0.875 accuracy in the remaining three.

A paired t-test produced:

- **t = -1.964**
- **p = 0.0811**

Because p > 0.05, the performance difference is not statistically significant at the 0.05 significance level. Therefore, the experiment does not support the conclusion that the shallow NN significantly outperforms CSP+LDA. The observed results instead favor CSP+LDA in mean accuracy and stability, although the small dataset and eight-sample test partitions limit the strength of the statistical conclusion.

## Complexity and Edge Inference

The selected NN is small by modern neural-network standards, with only 1,490 trainable parameters. Its measured forward-pass latency in the development environment was approximately **0.0095 ms per sample**, indicating that the network itself is computationally lightweight and capable of real-time inference.

However, latency alone does not determine the preferred edge architecture. The NN requires two learned nonlinear layers together with batch normalization, ReLU activations, dropout during training, model-weight storage, and a neural-network inference runtime. CSP+LDA instead provides a structured feature transformation followed by a simple linear discriminant classifier. It is also easier to interpret because the CSP representation directly relates classification to differences in sensor variance patterns.

The key issue is therefore not that the shallow NN is too slow for edge inference. Rather, its additional architectural complexity currently provides **no demonstrated accuracy advantage** over the classical approach.

## Deployment Recommendation

For **Aido Rover**, CSP+LDA is the preferred architecture. A resource-constrained mobile platform benefits from minimizing computation, memory requirements, software dependencies, and model complexity. In the current benchmark, CSP+LDA provides perfect and stable classification while remaining more interpretable than the NN. There is therefore little reason to introduce a neural-network inference pipeline for the current PATROL vs. FAULT task.

For **Sentinel Prime AI**, the hardware context may make deployment of the shallow NN practical, and the measured NN inference latency suggests that real-time execution is unlikely to be a limitation. Nevertheless, the benchmark still does not justify choosing the NN for the current task. CSP+LDA achieved higher and more stable observed accuracy without requiring the additional nonlinear architecture.

The NN should therefore be treated as a candidate for future expansion rather than the preferred current model. If Sentinel Prime AI later operates with larger datasets, additional operating modes, or sensor relationships that are strongly nonlinear, the greater representational capacity of a neural network may become valuable.

## Recommendation

**Use CSP+LDA as the current deployment baseline for both Sentinel Prime AI and Aido Rover.** It provides the best combination of predictive performance, stability, interpretability, and implementation simplicity in the present benchmark.

For Aido Rover, the simplicity advantage is particularly important because of edge-resource constraints. For Sentinel Prime AI, the shallow NN remains a feasible future option, but it should only replace the classical pipeline if larger and more complex datasets demonstrate a reproducible performance advantage that justifies the added model complexity.
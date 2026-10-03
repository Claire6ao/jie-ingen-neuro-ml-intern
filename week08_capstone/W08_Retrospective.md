# Week 8 Retrospective — Neuro-ML for Physical AI

**Jie (Claire) Gao | InGen Dynamics | Final Capstone**

## 1. Which Neuro-ML bridge held up best?

The strongest mapping was **EEG Common Spatial Patterns (CSP) + Linear Discriminant Analysis (LDA) → robotic state decoding**. The transfer is mathematically specific: CSP extracts projections with class-dependent variance, and LDA classifies the resulting features. Neither operation requires the input channels to be neural signals. On the synthetic Rover PATROL-versus-FAULT task, CSP+LDA achieved **1.000 held-out accuracy and F1**. Week 6 extended the same idea with one-vs-rest CSP to synthetic humanoid WALK, REACH, and BALANCE classification, again reaching **1.000 test accuracy and macro-F1**. These results validate the implementation and formal analogy, although they do not demonstrate real-world robot generalization.

## 2. Which mapping was more superficial than expected?

The **bionic prosthetic-arm → humanoid control** mapping was more architectural than algorithmic. Both systems connect computation to physical actuation, but a prosthetic arm's input-to-controller-to-actuator structure does not, by itself, establish that its control methods transfer to a multi-joint humanoid. Week 6 tested *motion-state decoding*, not a closed-loop control policy or physical actuation. The useful lesson was to distinguish a valid shared system architecture from a demonstrated transfer of a specific learning algorithm.

## 3. Strongest quantitative result

The most informative result was the **ten-seed CSP+LDA versus shallow-neural-network comparison**, rather than any single perfect test score. CSP+LDA averaged **1.000 ± 0.000 accuracy**, whereas the shallow NN averaged **0.963 ± 0.060**. The paired t-test gave **t = −1.964, p = 0.0811**; the observed difference was **not statistically significant** at α = 0.05. Together with the Week 4 finding that motor-current-only input matched full-sensor classification performance, this changed my approach to model selection: added modalities and nonlinear capacity need to earn their complexity through measurable improvement. The small synthetic dataset and eight-window test sets limit the strength of this conclusion.

## 4. Research direction for a graduate thesis

I would investigate **uncertainty-aware multimodal state decoding under robot-to-robot and environment-to-environment domain shift**. The first priority would be synchronized, high-frequency **real** telemetry from multiple robots, environments, and fault conditions. I would benchmark interpretable spectral/CSP features against temporal and neural models, then test adaptive calibration, sensor-dropout robustness, uncertainty estimates, and cross-platform generalization. Evaluation should emphasize rare-fault precision/recall, calibration, detection latency, and computational cost—not accuracy alone. This would move the project beyond demonstrating that Neuro-ML methods *can* transfer mathematically toward establishing **when they remain reliable in deployed Physical AI systems**.

## 5. Overall reflection

The eight-week project connected an initial landscape and eight proposed methodology mappings to a sequence of reproducible signal-processing, classification, modality-comparison, humanoid-decoding, and anomaly-detection experiments. My main takeaway is methodological discipline: identify the exact structure being transferred, compare it with simple baselines, report uncertainty honestly, and avoid treating strong synthetic-data performance as deployment evidence.

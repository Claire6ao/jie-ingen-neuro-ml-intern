# Week 08 — Final Capstone & Project Recap

## 1. Weekly Overview

Week 8 focused on consolidating the eight-week Neuro-ML research project into a comprehensive final capstone. The primary objective was to integrate the theoretical foundations, experimental implementations, quantitative results, and practical implications of transferring neuroscience-inspired machine-learning methods to Physical AI systems.

Throughout the internship, the project progressed from identifying conceptual connections between neural signal processing and robotic intelligence to implementing and evaluating signal-processing pipelines, classification models, multimodal benchmarks, humanoid motion decoding, and anomaly-detection methods.

The final capstone brings these individual investigations together into a unified research framework and examines their potential applications across InGen's Physical AI platforms.

## 2. Final Capstone

The final capstone consists of three main deliverables:

- **W08_Capstone_Report:** A comprehensive technical report integrating the project's mathematical foundations, experimental methods, quantitative findings, limitations, and platform-specific recommendations.
- **W08_Capstone_Deck:** A 12-slide executive presentation summarizing the research methodology, principal experimental results, and implications for Physical AI.
- **W08_Retrospective:** A reflection on the effectiveness of the Neuro-ML methodology transfers, the limitations encountered, and potential future research directions.

Together, these deliverables establish a coherent narrative connecting neuroscience research with practical machine-learning applications in robotics.

## 3. Overall Project Summary

### 3.1 Neuro-ML Methodology Bridge

The project began by examining how mathematical frameworks developed for neuroscience could be transferred to Physical AI.

Eight methodology mappings were investigated, connecting neural signal processing, spatial filtering, classification, sensor modality evaluation, neural networks, and motor-imagery decoding with corresponding robotic applications.

The central principle was that neural and robotic signals can share computational structures even when their physical origins differ.

### 3.2 Sensor Signal Analysis

The first experimental stage focused on characterizing robotic telemetry through exploratory data analysis and frequency-domain signal processing.

Using statistical analysis, correlation measurements, Power Spectral Density (PSD), and frequency-band energy extraction, the experiments identified distinct patterns associated with different operational states.

In the synthetic high-frequency dataset, FAULT windows exhibited elevated high-frequency energy, accounting for approximately 71.1% of motor-current spectral energy and 96.1% of IMU spectral energy in the 2–5 Hz band.

These results established a foundation for subsequent feature extraction and classification experiments.

### 3.3 CSP and LDA Classification

The next stage transferred Common Spatial Patterns (CSP) and Linear Discriminant Analysis (LDA), commonly used in EEG motor-imagery decoding, to robotic operational-state classification.

CSP was used to extract discriminative spatial features from multichannel sensor recordings, while LDA performed classification using the extracted features.

The CSP/LDA pipeline achieved 100% accuracy on the small held-out synthetic PATROL-versus-FAULT test set. However, raw-feature Logistic Regression achieved the same result.

This demonstrated the feasibility of transferring the mathematical framework while highlighting the importance of evaluating complex methods against simpler baselines.

### 3.4 Multimodal Sensor Benchmarking

The multimodal benchmarking stage investigated the predictive contribution of different robotic sensor modalities.

Five sensor configurations were evaluated using five classification methods, producing 25 model-modality combinations.

Both the all-sensor and motor-current-only configurations achieved a cross-validation macro-F1 of 1.000.

The results demonstrated that incorporating additional sensor modalities does not necessarily improve classification performance when individual modalities already contain highly discriminative information.

### 3.5 Shallow Neural Network Evaluation

The project subsequently examined whether a shallow neural network could improve operational-state classification compared with classical machine-learning methods.

A neural network with two hidden layers was evaluated through hyperparameter experiments and repeated comparisons with CSP/LDA.

Across ten paired evaluations:

- CSP/LDA achieved a mean accuracy of 1.000 ± 0.000.
- The shallow neural network achieved a mean accuracy of 0.963 ± 0.060.
- The paired statistical comparison produced p = 0.0811.

The experiment did not demonstrate a statistically significant performance difference at the 0.05 level.

This reinforced the importance of matching model complexity to the characteristics and scale of the available dataset.

### 3.6 BCI-to-Humanoid Methodology Transfer

The BCI-to-Humanoid investigation extended the Neuro-ML framework from binary operational-state classification to multiclass humanoid motion recognition.

Inspired by EEG motor-imagery decoding, a one-vs-rest CSP/LDA pipeline was applied to synthetic multichannel joint-angle recordings.

The model classified three humanoid motion primitives: WALK, REACH, and BALANCE.

The pipeline achieved 100% test accuracy and macro-F1 on the synthetic benchmark, demonstrating the feasibility of applying spatial-filtering methods to structured robotic motion signals.

### 3.7 Robotic Anomaly Detection

The final experimental stage investigated fault detection using frequency-domain sensor features and multiple anomaly-detection approaches.

Five controlled fault types were evaluated across repeated experiments.

Logistic Regression achieved a mean AUROC of 0.948 and a mean F1 of 0.847. However, its AUROC difference from One-Class SVM was not statistically significant according to the paired Wilcoxon test (p = 0.105).

The results also revealed differences in detection difficulty across fault categories, particularly for IMU failures.

This investigation emphasized the importance of fault-specific evaluation rather than relying exclusively on aggregate performance metrics.

## 4. Integrated Research Findings

The eight-week project produced several overarching findings.

**First, mathematical compatibility provides a foundation for cross-domain methodology transfer.** Signal-processing and classification methods originally developed for neuroscience can be adapted to robotic sensing when the underlying mathematical assumptions are appropriate.

**Second, more complex models do not automatically produce better results.** Classical machine-learning methods performed comparably to, or achieved higher observed accuracy than, the shallow neural network on the evaluated synthetic datasets.

**Third, sensor selection is an important component of model design.** The multimodal experiments demonstrated that a carefully selected individual sensor modality can sometimes provide comparable predictive performance to a full multimodal configuration.

**Finally, controlled experimental performance must be distinguished from real-world applicability.** The synthetic datasets enabled systematic evaluation of the proposed methods but did not establish their robustness under realistic robotic operating conditions.

## 5. Implications for Physical AI

The integrated findings provide a methodological foundation for developing interpretable and efficient machine-learning systems across different Physical AI platforms.

Potential applications include spectral monitoring of robotic sensors, operational-state classification, multimodal perception, humanoid motion recognition, and automated fault detection.

The project also suggests that future systems should incorporate uncertainty estimation, robust sensor fusion, and systematic evaluation of computational efficiency and generalization.

These directions require further validation using real-world robotic datasets before deployment.

## 6. Overall Reflection and Future Direction

The most significant outcome of this internship was developing a systematic approach to transferring computational methods between neuroscience and robotics.

Rather than treating individual algorithms as universally applicable, the project emphasized understanding their mathematical foundations, identifying appropriate cross-domain mappings, implementing reproducible experiments, comparing suitable baselines, and critically interpreting quantitative results.

The final capstone consolidated these experiences into an integrated Neuro-ML research framework.

A natural extension would be to investigate uncertainty-aware multimodal learning using synchronized real-world robotic sensor data, with particular emphasis on cross-platform generalization, anomaly detection, and reliable embodied intelligence.

Overall, the eight-week project strengthened the connection between my background in neuroscience and machine learning and my interest in developing computational approaches for intelligent physical systems.
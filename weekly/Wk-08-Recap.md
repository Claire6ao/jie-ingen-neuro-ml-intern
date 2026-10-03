# Week 08 — Final Capstone Recap

**Project:** InGen Neuro-ML / Physical AI Internship  
**Week:** 08 — Final Capstone and Project Integration  
**Name:** Jie (Claire) Gao

---

## 1. Weekly Objectives

Week 8 focused on consolidating the previous seven weeks of research into a final capstone, evaluating the effectiveness of the Neuro-ML methodology transfers, and preparing the project for its final presentation and handover.

The main objectives were to:

- Integrate the theoretical and experimental findings from Weeks 1–7.
- Complete the final capstone report and executive presentation.
- Evaluate the strengths and limitations of the Neuro-ML methodology mappings.
- Summarize platform-specific ML recommendations.
- Prepare the GitHub repository and final project documentation.

## 2. Work Completed

### 2.1 Final Capstone Report

Completed `W08_Capstone_Report.docx`, integrating the eight-week project into twelve sections.

The report covers:

- InGen's Physical AI landscape and PIC 2.0 context.
- Eight Neuro-ML methodology mappings.
- Sensor signal EDA and frequency-domain analysis.
- CSP/LDA operational-state classification.
- Multimodal sensor benchmarking.
- Shallow neural-network evaluation and statistical testing.
- BCI-to-Humanoid methodology transfer.
- Anomaly-detection benchmarking.
- Per-platform ML recommendations.
- Research limitations and future directions.

The report includes mathematical formulations, quantitative results, statistical comparisons, tables, and experimental figures.

### 2.2 Executive Presentation

Prepared the 12-slide `W08_Capstone_Deck.pptx` for the final capstone presentation.

The presentation emphasizes the research question, experimental methodology, quantitative findings, model comparisons, and implications for InGen's Physical AI platforms.

The slides were organized around finding-based headlines to communicate the project's principal results clearly.

### 2.3 Integration of Experimental Findings

Reviewed and consolidated the main findings from the previous technical weeks.

**Sensor Signal Analysis**

Frequency-domain analysis identified distinct operational-state signatures. In the synthetic high-frequency dataset, FAULT windows concentrated 71.1% of motor-current spectral energy and 96.1% of IMU spectral energy in the 2–5 Hz band.

**CSP/LDA Classification**

The EEG-inspired CSP/LDA pipeline achieved 100% accuracy on the eight-window held-out PATROL-versus-FAULT test set. Raw-feature Logistic Regression achieved the same result, demonstrating the feasibility of the methodology transfer without establishing an additional performance advantage for CSP.

**Multimodal Benchmarking**

The all-sensor and motor-current-only models both achieved a cross-validation macro-F1 of 1.000, demonstrating that additional sensor modalities did not automatically improve classification performance on the synthetic dataset.

**Shallow Neural Network**

Across ten paired evaluations:

- CSP/LDA mean accuracy: 1.000 ± 0.000.
- Shallow NN mean accuracy: 0.963 ± 0.060.
- Paired t-test: p = 0.0811.

The experiment did not demonstrate a statistically significant performance difference at α = 0.05.

**BCI-to-Humanoid Bridge**

The one-vs-rest CSP/LDA pipeline achieved 100% test accuracy and macro-F1 when classifying three synthetic humanoid motion primitives: WALK, REACH, and BALANCE.

**Anomaly Detection**

Logistic Regression achieved a mean AUROC of 0.948 and mean F1 of 0.847 across the controlled fault benchmarks. Its AUROC difference from One-Class SVM was not statistically significant according to the paired Wilcoxon test (p = 0.105).

These findings were interpreted with appropriate consideration of synthetic data, limited sample sizes, and generalization constraints.

### 2.4 Final Retrospective

Completed `W08_Retrospective.md`, reflecting on the eight-week research experience.

The retrospective addresses:

- The strongest Neuro-ML methodology transfer.
- Mappings whose practical significance was more limited than initially expected.
- The most important quantitative findings.
- Methodological limitations identified during the internship.
- A potential graduate-level research direction involving uncertainty-aware multimodal state decoding.

## 3. Key Takeaways

The capstone demonstrated that neuroscience-derived methods can be transferred to Physical AI when the underlying mathematical structures are shared.

Spectral analysis and CSP/LDA provided direct methodological connections between neural recordings and robotic telemetry. However, the experiments also demonstrated the importance of evaluating transferred methods against simpler baselines.

Increasing model complexity or adding sensor modalities did not automatically improve performance.

The principal limitation was the reliance on synthetic and controlled datasets. Further validation with synchronized real-world robotic telemetry is required before drawing conclusions about deployment performance.

## 4. Deliverables

| Deliverable | Status |
|---|---|
| `W08_Capstone_Report.docx` | Completed |
| `W08_Capstone_Deck.pptx` | Completed |
| `W08_Retrospective.md` | Completed |
| `weekly/Wk-08-Final-Recap.md` | Completed |
| Final presentation and Q&A | Pending confirmation |
| Final evaluation rubric and signatures | Pending |
| GitHub README and clean-clone verification | Pending verification |
| GitHub `v1.0` release tag | Pending verification |

## 5. Finalization Checklist

Before the final project handover:

- [x] Complete the capstone report.
- [x] Prepare the executive presentation.
- [x] Complete the retrospective.
- [x] Prepare the Week 8 recap.
- [ ] Verify that all notebooks execute successfully from a clean repository clone.
- [ ] Finalize the GitHub README and repository documentation.
- [ ] Create and verify the `v1.0` Git tag.
- [ ] Complete the 30-minute final presentation and 15-minute Q&A.
- [ ] Complete the final evaluation rubric and obtain the required signatures.

## 6. Overall Reflection

The eight-week internship connected my previous neuroscience and machine-learning experience with practical Physical AI research questions.

The project progressed from conceptual methodology mapping to signal analysis, classical and neural classification, multimodal benchmarking, humanoid motion-state recognition, and anomaly detection.

The most important outcome was developing a systematic approach to evaluating methodological transfer: identify shared mathematical structures, establish suitable baselines, compare quantitative performance, assess statistical evidence, and acknowledge the limitations of the available data.

This framework provides a foundation for future research in robust multimodal learning, neural decoding, and embodied intelligence.
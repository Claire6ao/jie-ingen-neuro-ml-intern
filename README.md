# InGen Neuro-ML: From Neural Decoding to Physical AI

## Overview

This repository contains the research notebooks, documentation, and final deliverables from an eight-week Neuro-ML / Physical AI internship project.

The project investigates how mathematical and computational methods from neuroscience, particularly EEG signal processing and motor-imagery decoding, can be transferred to robotic sensing, operational-state classification, humanoid motion recognition, and anomaly detection.

The central research question is:

**How can neuroscience-inspired signal-processing and machine-learning methods support interpretable and robust Physical AI systems?**

All experiments use synthetic data, public datasets, or open-source tools. No confidential InGen data is included.

## Research Objectives

The project has five main objectives:

1. Establish mathematical connections between neuroscience methods and Physical AI applications.
2. Analyze robotic sensor signals using time-domain and frequency-domain methods.
3. Transfer CSP and LDA from EEG decoding to robot operational-state classification.
4. Compare sensor modalities and classical versus neural machine-learning models.
5. Investigate humanoid motion decoding and robotic fault detection.

## Repository Structure

```text
InGen-NeuroML/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── W02_Sensor_EDA.ipynb
│   ├── W03_CSP_Feature_Pipeline.ipynb
│   ├── W03_LDA_Classifier.ipynb
│   ├── W04_Multi_Modal_Benchmark.ipynb
│   ├── W05_ShallowNN_Classifier.ipynb
│   ├── W06_Motor_Imagery_Bridge.ipynb
│   └── W07_Anomaly_Detection.ipynb
├── reports/
│   ├── W08_Capstone_Report.docx
│   ├── W08_Capstone_Deck.pptx
│   └── W08_Retrospective.md
└── weekly/
    └── Wk-08-Final-Recap.md
```

The directory structure above assumes that the notebooks and final deliverables have been organized into the corresponding folders.

## Neuro-ML Methodology

The project explores eight conceptual mappings between neural computation and robotic intelligence, including:

- EEG spectral analysis and robotic sensor frequency analysis.
- Common Spatial Patterns and multichannel robot signal filtering.
- Linear Discriminant Analysis and operational-state classification.
- Neural recording modality comparisons and robotic sensor benchmarking.
- Neural frequency-band features and robotic spectral representations.
- Shallow neural networks and robotic sensor classification.
- EEG motor-imagery decoding and humanoid motion recognition.
- Neural decoding architectures and embodied robotic control.

These mappings rely on shared mathematical structures rather than assuming that neural and robotic signals are physically equivalent.

## Experimental Notebooks

| Week | Notebook | Main objective |
|---|---|---|
| 2 | Sensor EDA | Explore telemetry, PSD, spectral energy and correlations |
| 3 | CSP Feature Pipeline | Implement spatial filtering and extract discriminative features |
| 3 | LDA Classifier | Compare raw-feature classification with CSP/LDA |
| 4 | Multi-Modal Benchmark | Evaluate sensor modalities and multiple classifiers |
| 5 | Shallow NN Classifier | Compare neural networks with classical ML |
| 6 | Motor Imagery Bridge | Transfer CSP/LDA to humanoid motion classification |
| 7 | Anomaly Detection | Benchmark robotic fault-detection approaches |

## Key Experimental Findings

**Signal analysis:** Synthetic FAULT windows exhibited elevated high-frequency motor-current and IMU energy.

**CSP/LDA:** The pipeline achieved 100% accuracy on a small held-out synthetic test set, matching raw-feature Logistic Regression.

**Multimodal classification:** Motor-current-only and all-sensor models both achieved a cross-validation macro-F1 of 1.000.

**Neural-network comparison:** Across ten paired evaluations, CSP/LDA achieved mean accuracy of 1.000 and the shallow neural network achieved 0.963. The difference was not statistically significant at the 0.05 level.

**Humanoid motion recognition:** One-vs-rest CSP/LDA achieved 100% test accuracy on three synthetic motion classes.

**Anomaly detection:** Logistic Regression achieved a mean AUROC of 0.948 across the controlled fault benchmarks.

These results demonstrate methodological feasibility on the evaluated datasets. They do not establish real-world deployment performance.

## Reproducibility

### Environment Setup

Clone the repository:

```bash
git clone <YOUR_REPOSITORY_URL>
cd InGen-NeuroML
```

Create and activate a Python virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows, activate the environment using:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter lab
```

### Data Preparation

The experiments use synthetic robotic data and, where applicable, public EEG data.

Before running the notebooks, document any required external dataset downloads, local data paths, and generated intermediate files here.

Do not commit confidential datasets, private credentials, or large generated artifacts.

### Recommended Execution Order

Run the notebooks in the following order:

1. Week 2 — Sensor EDA
2. Week 3 — CSP Feature Pipeline
3. Week 3 — LDA Classifier
4. Week 4 — Multi-Modal Benchmark
5. Week 5 — Shallow NN Classifier
6. Week 6 — Motor Imagery Bridge
7. Week 7 — Anomaly Detection

Check the input and output paths in each notebook before execution.

If a notebook requires an intermediate file generated by another notebook, run the producer notebook first.

### Evaluation Protocol

Classification experiments use an approximately 80/20 training/test split.

The held-out test set must remain separate from model fitting, preprocessing parameter estimation, and hyperparameter selection.

Where applicable, model selection uses training-set cross-validation or a separate validation subset.

Reported statistical comparisons should preserve the experimental design used in their corresponding notebooks.

### Reproducibility Notes

- Use the pinned dependency versions in `requirements.txt`.
- Record the Python version and operating system used for the final verified run.
- Preserve the random seeds specified in each experiment.
- Run every notebook from a clean repository clone.
- Document any required external downloads and generated data files.
- Compare the reproduced metrics with the results reported in the final capstone.

A successful clean-clone execution should be verified before marking the repository as fully reproducible.

## Final Deliverables

- `W08_Capstone_Report.docx` — Full technical capstone report.
- `W08_Capstone_Deck.pptx` — Executive presentation.
- `W08_Retrospective.md` — Final project reflection.
- `weekly/Wk-08-Final-Recap.md` — Week 8 completion summary.

## Limitations

The principal limitations include synthetic datasets, relatively small samples in several experiments, controlled fault injection, and unverified transfer to real robotic hardware.

Future work should prioritize real-world validation, cross-platform generalization, uncertainty estimation, and deployment-oriented performance metrics.

## Version

**Target release:** v1.0

The release should be tagged only after the final repository structure, dependencies, documentation, and clean-clone execution have been verified.

## Author

Jie (Claire) Gao  
Neuro-ML / Physical AI Internship
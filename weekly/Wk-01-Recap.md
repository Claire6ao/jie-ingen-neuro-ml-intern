# Week 1 Recap — InGen Physical AI and the Neuro-ML Bridge

## Most Analogous Platform to the EEG Motor Imagery Project: Aido Rover

Among the InGen platforms examined this week, **Aido Rover** is the most methodologically analogous to my EEG Motor Imagery Classification project.

The connection is not based on the physical similarity of the sensors. EEG electrodes measure electrical activity from the brain, whereas Aido Rover uses physical telemetry such as IMU measurements, motor current, proximity signals, and RSSI. Instead, the strongest connection is that both systems can be represented as **multichannel time-series classification problems**.

In my EEG motor-imagery project, Common Spatial Patterns (CSP) identifies discriminative projections from class-dependent covariance matrices by solving the generalized eigenvalue problem:

$$R_1w=\lambda R_2w$$

The resulting CSP features are then classified using Linear Discriminant Analysis (LDA):

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{motor imagery}}$$

The same mathematical pipeline can be transferred to Aido Rover:

$$X_{\text{Rover}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{operational mode}}$$

Here, EEG channels are replaced by robot telemetry channels, while motor-imagery classes are replaced by operational states such as PATROL, ALERT, CHARGING, or FAULT.

The important insight is that the physical meaning of the signals changes, but the underlying covariance analysis, feature extraction, and classification framework remains the same.

## PIC 2.0 Model Closest to a Neural Decoder: SEOM

For the neuroscience analogy specified in the internship plan, **SEOM** is the PIC 2.0 model most closely related to a neural decoder because it can be framed as transforming high-dimensional observations into an estimate of an underlying state.

This is analogous to my M1 LFP reaching-behavior project:

$$X_{\text{LFP}} \rightarrow \phi(X) \rightarrow \hat{y}_{\text{behavior}}$$

where measured neural activity is transformed into features that predict behavioral state.

The corresponding physical-AI structure is:

$$X_{\text{sensor}} \rightarrow \phi(X) \rightarrow \hat{s}_{\text{system state}}$$

In both cases, the measured signal is not the final variable of interest. Instead, information contained in the observations is used to infer a latent behavioral, environmental, or operational state.

## Key Takeaway

The main lesson from Week 1 is that the strongest bridge between neuroscience and physical AI is **mathematical rather than sensor-specific**:

$$\boxed{\text{different physical signals} + \text{shared mathematical structure} = \text{transferable methodology}}$$

This perspective provides the foundation for transferring neural-signal methods such as spectral analysis, CSP, LDA, modality comparison, and neural-network classification to physical-AI sensor problems in the following weeks.
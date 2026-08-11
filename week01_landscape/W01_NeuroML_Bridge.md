# Week 1 — Neuro-ML Methodology Bridge

## Purpose

This document maps methods from my prior neuroscience and neural-signal projects to analogous computational problems in physical AI.

The goal is not to claim that biological neural signals and robot sensor signals are physically equivalent. EEG electrodes, LFP recordings, cameras, LiDAR, radar, and other robotic sensors measure fundamentally different physical quantities.

Instead, the bridge focuses on cases where the two domains share the same or closely related **mathematical operations**:

$$
\text{signal acquisition}
\rightarrow
\text{preprocessing}
\rightarrow
\text{representation}
\rightarrow
\text{inference}
\rightarrow
\text{decision}.
$$

The eight mappings below identify a specific operation from neuroscience, its robot-sensor analogue, and the mathematical reason that the comparison is valid.

---

## Mapping 1 — Multichannel Signal Representation

### Prior Neuroscience Method

EEG records electrical activity simultaneously from multiple electrodes. A multichannel EEG recording can be represented as a matrix

$$
X \in \mathbb{R}^{C \times T},
$$

where $C$ is the number of recording channels and $T$ is the number of time samples.

Each row represents one EEG channel:

$$
X =
\begin{bmatrix}
x_1(1) & x_1(2) & \cdots & x_1(T) \\
x_2(1) & x_2(2) & \cdots & x_2(T) \\
\vdots & \vdots & \ddots & \vdots \\
x_C(1) & x_C(2) & \cdots & x_C(T)
\end{bmatrix}.
$$

My EEG projects used this type of multichannel representation before extracting frequency-domain or other predictive features.

### Robot-Sensor Analogue

A physical-AI system also receives multiple streams of observations. For example, a multimodal security or mobile robotic platform may receive visual, thermal, acoustic, radar, LiDAR, and environmental information.

These measurements can be represented abstractly as

$$
X_{\text{robot}}
=
\left[
X_{\text{vision}},
X_{\text{thermal}},
X_{\text{audio}},
X_{\text{radar}},
X_{\text{LiDAR}},
\ldots
\right].
$$

Each modality provides a different observation of the same underlying environment.

### Shared Mathematical Structure

Both systems begin with a collection of high-dimensional measurements:

$$
X = \{x_1,x_2,\ldots,x_C\}.
$$

The computational problem is to transform these measurements into a representation that preserves task-relevant information while suppressing noise and redundancy.

### Why the Mapping Is Valid

The analogy does not depend on EEG electrodes and robot sensors measuring the same physical variable. It depends on both systems treating multiple simultaneous measurements as inputs to a downstream inference system.

In both cases:

$$
\text{multichannel measurements}
\rightarrow
\text{joint representation}
\rightarrow
\text{prediction}.
$$

---

## Mapping 2 — Spectral Analysis and Frequency-Domain Feature Extraction

### Prior Neuroscience Method

In my EEG analysis, neural signals were transformed from the time domain into frequency-domain representations.

One method used for this type of analysis is Welch's power spectral density estimate.

For a signal $x(t)$, the power spectral density can be written conceptually as

$$
P_{xx}(f)
=
\frac{1}{K}
\sum_{k=1}^{K}
P_{xx}^{(k)}(f),
$$

where the signal is divided into overlapping windows and the periodogram from each window is averaged.

This allows neural activity to be summarized in frequency bands such as:

- Delta
- Theta
- Alpha
- Beta
- Gamma

For example, band power can be represented as

$$
P_{\text{band}}
=
\int_{f_1}^{f_2}
P_{xx}(f)\,df.
$$

These spectral features can then become inputs to a classifier.

### Robot-Sensor Analogue

Robot sensors can also produce temporal signals containing meaningful frequency structure.

Examples include:

- vibration signals from motors,
- acoustic signals,
- rotating machinery measurements,
- periodic environmental disturbances,
- mechanical resonance signals.

A robot monitoring a motor, for example, may observe a vibration signal $x(t)$. Abnormal mechanical behavior may create characteristic changes in its frequency spectrum.

The same transformation can therefore be applied:

$$
x(t)
\rightarrow
P_{xx}(f)
\rightarrow
\text{frequency features}.
$$

### Shared Mathematical Structure

In both neuroscience and physical sensing, the operation transforms a time-domain signal into a frequency-domain representation:

$$
x(t)
\xrightarrow{\text{Fourier / spectral transform}}
X(f).
$$

The physical origin of the signal changes, but the signal-processing operation does not.

### Why the Mapping Is Valid

This is a direct mathematical mapping because spectral analysis is independent of whether the original signal represents neural voltage, sound pressure, or mechanical vibration.

The same algorithms used to quantify oscillatory neural activity can therefore be repurposed for periodic or oscillatory robot-sensor signals.

---

## Mapping 3 — CSP and Spatial Filtering

### Prior Neuroscience Method

Common Spatial Patterns (CSP) is commonly used in EEG motor-imagery decoding.

Given multichannel EEG data from two conditions, CSP learns spatial filters that maximize variance for one class while minimizing variance for the other.

Let the class-specific covariance matrices be

$$
C_1
\quad \text{and} \quad
C_2.
$$

CSP can be formulated through the generalized eigenvalue problem

$$
C_1 w = \lambda C_2 w.
$$

The learned vector $w$ defines a linear projection:

$$
z(t)=w^T x(t).
$$

The resulting components emphasize combinations of channels that are discriminative between the two conditions.

### Robot-Sensor Analogue

A robot can also receive multiple correlated sensor channels.

Suppose

$$
x =
[x_1,x_2,\ldots,x_C]^T
$$

contains measurements from multiple sensors.

A learned linear projection

$$
z=w^Tx
$$

can emphasize combinations of sensor measurements that best distinguish two operational states, such as

$$
\text{normal}
\quad \text{vs.} \quad
\text{anomalous}.
$$

### Shared Mathematical Structure

Both problems learn a projection from a high-dimensional sensor space into a lower-dimensional discriminative space:

$$
x \in \mathbb{R}^{C}
\rightarrow
z \in \mathbb{R}^{d},
\qquad d<C.
$$

The objective is not simply dimensionality reduction. The transformation is selected specifically to increase class separability.

### Why the Mapping Is Valid

The mapping is mathematically valid because the generalized eigenvalue operation depends on class-conditional covariance structure rather than the biological identity of the channels.

Therefore,

$$
\text{EEG spatial filtering}
$$

and

$$
\text{multisensor discriminative projection}
$$

can share the same mathematical structure even though their sensors are physically different.

---

## Mapping 4 — PCA and Low-Dimensional State Representation

### Prior Neuroscience Method

High-dimensional neural recordings often contain correlated activity. Principal Component Analysis (PCA) can be used to identify directions that explain the largest amount of variance.

Given a centered data matrix $X$, its covariance matrix is

$$
C=\frac{1}{n-1}X^TX.
$$

PCA solves

$$
Cv_i=\lambda_i v_i,
$$

where $v_i$ is an eigenvector and $\lambda_i$ represents the variance explained along that direction.

The original data can then be projected into a lower-dimensional space:

$$
Z=XW,
$$

where

$$
W=[v_1,v_2,\ldots,v_k].
$$

### Robot-Sensor Analogue

A robot may also receive a high-dimensional observation vector containing correlated information from many sensors.

Instead of performing downstream inference directly on the full input,

$$
x \in \mathbb{R}^{D},
$$

the system can construct a lower-dimensional representation

$$
z \in \mathbb{R}^{d},
\qquad d<D.
$$

This representation may capture dominant environmental or operational variation while removing redundant information.

### Shared Mathematical Structure

Both problems perform

$$
X
\rightarrow
Z=XW.
$$

The objective is to represent high-dimensional observations using fewer latent variables.

### Why the Mapping Is Valid

PCA is defined by the covariance structure of the data, not by the physical meaning of the measurements.

Therefore, the same eigendecomposition used to identify dominant patterns in neural activity can identify dominant patterns in robot sensor observations.

The interpretation of the principal components changes, but the mathematical operation is identical.

---

## Mapping 5 — Neural Decoding and Supervised State Classification

### Prior Neuroscience Method

In my EEG confusion project and M1 LFP reaching-behavior analysis, the central ML task was decoding a state from measured signals.

After feature extraction, the classifier estimates

$$
P(y\mid x),
$$

where $x$ represents neural features and $y$ represents a behavioral or cognitive state.

For binary logistic regression,

$$
P(y=1\mid x)
=
\sigma(w^Tx+b),
$$

where

$$
\sigma(z)=\frac{1}{1+e^{-z}}.
$$

The prediction is then obtained from a decision rule such as

$$
\hat{y}
=
\mathbb{1}
\left[
P(y=1\mid x)>0.5
\right].
$$

### Robot-Sensor Analogue

A physical-AI system performs the same type of operation when sensor observations are used to classify environmental states.

For example:

$$
\text{sensor features}
\rightarrow
P(\text{threat}\mid x),
$$

or

$$
\text{sensor features}
\rightarrow
P(\text{anomaly}\mid x).
$$

The labels are different, but the classifier receives a feature vector and estimates a conditional probability over states.

### Shared Mathematical Structure

Both systems implement

$$
x
\rightarrow
f_\theta(x)
\rightarrow
P(y\mid x)
\rightarrow
\hat{y}.
$$

The learned parameters $\theta$ are optimized using labeled examples.

### Why the Mapping Is Valid

This is one of the strongest neuro-ML mappings because the mathematical learning problem can be identical.

For example, logistic regression, LDA, SVM, random forests, or neural networks can operate on either neural features or robot-sensor features without changing the fundamental supervised-learning formulation.

Only the interpretation of $x$ and $y$ changes.

---

## Mapping 6 — Multimodal Sensor Fusion

### Prior Neuroscience Method

Neuroscience often combines information from multiple sources.

In my neural-data projects, different neural measurements or feature families could provide complementary information about the same behavioral state.

Suppose two feature sets are

$$
x^{(1)}
\quad \text{and} \quad
x^{(2)}.
$$

An early-fusion approach concatenates them:

$$
z=
\left[
x^{(1)};
x^{(2)}
\right].
$$

A downstream model then predicts

$$
\hat{y}=f(z).
$$

Alternatively, separate models can produce predictions that are combined later:

$$
\hat{y}
=
g
\left(
f_1(x^{(1)}),
f_2(x^{(2)})
\right).
$$

### Robot-Sensor Analogue

A physical-AI system may combine evidence from:

- RGB cameras,
- thermal cameras,
- LiDAR,
- radar,
- acoustic sensors,
- environmental sensors.

For example,

$$
z=
\left[
z_{\text{RGB}},
z_{\text{thermal}},
z_{\text{LiDAR}},
z_{\text{audio}}
\right]
$$

can represent a fused multimodal feature vector.

### Shared Mathematical Structure

Both systems combine complementary information sources to estimate a common latent state:

$$
\{x^{(1)},x^{(2)},\ldots,x^{(M)}\}
\rightarrow
z
\rightarrow
\hat{y}.
$$

### Why the Mapping Is Valid

The mathematical problem is the same: estimate a target using multiple observations whose information content is partially complementary and partially redundant.

The challenge includes:

- alignment,
- normalization,
- redundancy,
- missing modalities,
- different noise levels,
- feature fusion.

These problems occur in both multimodal neuroscience and physical-AI sensing.

---

## Mapping 7 — Temporal Decoding and State Estimation

### Prior Neuroscience Method

Neural signals are time-dependent.

In my M1 LFP reaching-behavior project, neural activity was analyzed relative to movement onset. The temporal position of a neural observation was therefore important for determining whether the subject was in a pre-movement or post-movement state.

A sequence can be represented as

$$
X_{1:T}
=
(x_1,x_2,\ldots,x_T).
$$

Instead of predicting from one isolated observation, a temporal model estimates

$$
P(s_t\mid x_{1:t}),
$$

where $s_t$ is the underlying state at time $t$.

### Robot-Sensor Analogue

Robot state estimation is also inherently temporal.

A mobile robot should not estimate its position, environmental state, or threat state from a single measurement if previous observations contain useful information.

Instead, it can estimate

$$
P(s_t\mid z_{1:t}),
$$

where $z_{1:t}$ represents the history of sensor observations.

This general framework appears in:

- Bayesian filtering,
- Kalman filtering,
- Hidden Markov Models,
- recurrent neural networks,
- sequential state estimators.

### Shared Mathematical Structure

Both systems attempt to infer a time-dependent latent state from sequential noisy observations:

$$
x_{1:t}
\rightarrow
\hat{s}_t.
$$

The previous state may also influence the current state:

$$
P(s_t\mid s_{t-1}).
$$

### Why the Mapping Is Valid

The mathematical similarity arises because both neural activity and robot sensor measurements are sequential observations of a dynamical system.

Treating observations independently discards temporal information.

In both cases, temporal context can improve estimation of an underlying state.

---

## Mapping 8 — Anomaly Detection and Distribution Shift

### Prior Neuroscience Method

Neural and physiological data analysis often requires determining whether an observation is consistent with a baseline distribution.

Suppose normal observations follow

$$
x \sim P_{\text{normal}}(x).
$$

A new observation can be assigned an anomaly score

$$
A(x),
$$

where larger values indicate greater deviation from the baseline distribution.

A simple distance-based formulation is

$$
A(x)
=
(x-\mu)^T
\Sigma^{-1}
(x-\mu),
$$

where $\mu$ is the baseline mean and $\Sigma$ is the covariance matrix.

This is the Mahalanobis distance.

### Robot-Sensor Analogue

Physical-AI systems frequently need to identify events that differ from normal operation.

Examples include:

- unusual environmental activity,
- abnormal vibration,
- unexpected acoustic patterns,
- equipment malfunction,
- unexpected human behavior,
- sensor failure.

The robot can learn a model of normal observations

$$
P_{\text{normal}}(x)
$$

and identify observations with low likelihood:

$$
P_{\text{normal}}(x)<\tau.
$$

Alternatively, it can trigger when

$$
A(x)>\tau.
$$

### Shared Mathematical Structure

Both problems compare an observation against a learned baseline distribution:

$$
x
\rightarrow
A(x)
\rightarrow
\text{normal / anomalous}.
$$

### Why the Mapping Is Valid

The definition of an anomaly is statistical rather than sensor-specific.

Whether $x$ represents neural activity, vibration, sound, thermal measurements, or environmental sensor values, the same mathematical question can be asked:

> How unlikely is this observation under the distribution of normal observations?

This makes anomaly detection directly transferable between neural-data analysis and physical-AI monitoring.

---

# Summary of the Eight Neuro-ML Mappings

| # | Neuroscience Method | Mathematical Operation | Physical-AI Analogue |
|---|---|---|---|
| 1 | Multichannel EEG representation | $X \in \mathbb{R}^{C \times T}$ | Multichannel / multimodal robot sensing |
| 2 | EEG spectral analysis | $x(t) \rightarrow P_{xx}(f)$ | Acoustic or vibration spectral analysis |
| 3 | CSP spatial filtering | $C_1w=\lambda C_2w$ | Discriminative multisensor projection |
| 4 | PCA | $Cv=\lambda v$ | Sensor dimensionality reduction |
| 5 | Neural decoding | $P(y\mid x)$ | Environmental-state classification |
| 6 | Multimodal integration | $z=[x^{(1)};x^{(2)}]$ | Multisensor fusion |
| 7 | Temporal neural decoding | $P(s_t\mid x_{1:t})$ | Robot state estimation |
| 8 | Anomaly detection | $A(x)$ vs. threshold $\tau$ | Environmental / system anomaly detection |

---

## Final Bridge

Across these eight mappings, the common principle is that neuroscience and physical AI frequently solve the same abstract inference problem:

$$
\text{noisy observations}
\rightarrow
\text{mathematical transformation}
\rightarrow
\text{latent representation}
\rightarrow
\text{state estimate}.
$$

The sensor modality determines what the input means, but it does not necessarily determine the mathematical tools that can be applied to it.

For example:

- Welch's method remains a spectral estimator whether the input is EEG or mechanical vibration.
- PCA remains an eigendecomposition whether the variables represent neural activity or robot sensors.
- CSP remains a generalized eigenvalue problem when applied to class-dependent covariance structures.
- Logistic regression still estimates $P(y\mid x)$ regardless of whether $y$ represents a cognitive state or an environmental state.
- Temporal state estimation still attempts to infer $s_t$ from observations $x_{1:t}$ regardless of whether the hidden state is biological or robotic.

The methodological bridge can therefore be summarized as

$$
\boxed{
\text{different physical signals}
+
\text{shared mathematical structure}
=
\text{transferable methodology}
}
$$

This is the central principle I will use in subsequent weeks when adapting neural-signal processing and machine-learning methods to physical-AI sensor problems.
# Week 1 — InGen Physical AI Landscape: A Neuro-ML Perspective

## 1. Introduction

InGen Dynamics develops a family of physical-AI and robotic systems built around a shared intelligence architecture referred to as the **Origami AI Platform / Physical Intelligence Core (PIC 2.0)**. Rather than treating Fari, Senpai, Sentinel Prime AI, Aido Rover, and the InGen Humanoid as completely independent products, InGen presents them as increasingly complex embodiments of a common intelligence layer.

From a neuroscience and machine-learning perspective, this architecture is particularly interesting because many of the underlying computational problems resemble those encountered in neural signal analysis. An EEG or LFP experiment begins with noisy, high-dimensional measurements collected from multiple sensors or electrodes. These signals are transformed into useful representations, and a statistical or machine-learning model then infers an underlying state such as motor intention, cognitive state, or behavioral condition.

A physical-AI system faces a structurally similar problem: cameras, microphones, LiDAR, radar, and other sensors generate high-dimensional observations from which the system must estimate the state of its environment and select an appropriate response.

The analogy is therefore not that an EEG electrode and a robot camera measure the same physical quantity. Rather, both systems implement versions of the computational pipeline

$$
\text{multichannel observations}
\rightarrow
\text{feature representation}
\rightarrow
\text{state estimation}
\rightarrow
\text{decision}.
$$

This report examines five InGen platforms — **Fari, Senpai, Sentinel Prime AI, Aido Rover, and the InGen Humanoid** — from this neuro-ML perspective. For each platform, I identify its primary sensing problem, machine-learning task, operational constraints, and the closest methodological connection to neural-signal analysis.

---

## 2. Fari — Eldercare Companion Robot

### 2.1 Product Role

Fari is InGen's eldercare-oriented companion robot. The platform is designed to support older adults through communication, interaction, assistance, cognitive activities, and monitoring-related functions.

Unlike a conventional industrial robot whose main objective may be navigation or manipulation, Fari operates in a highly human-centered environment. Its central computational problem is therefore not simply recognizing physical objects, but also inferring information about the person interacting with the system.

### 2.2 Primary Sensor Modalities

Fari can be viewed as relying primarily on visual, behavioral, interaction, and potentially health-related signals.

Possible observation streams include:

- Visual information
- Human pose
- Facial or gaze-related information
- Voice or audio interaction
- User interaction history
- Connected health or medical-device signals

From a neuro-ML perspective, these inputs can be represented as a multimodal observation vector:

$$
\mathbf{x}_t =
[
\mathbf{x}^{vision}_t,
\mathbf{x}^{audio}_t,
\mathbf{x}^{interaction}_t,
\mathbf{x}^{health}_t
].
$$

The physical measurements differ from EEG electrodes, but the analytical challenge is similar: multiple noisy observations must be combined to estimate a state that cannot necessarily be measured directly.

### 2.3 Primary ML Tasks

The most relevant machine-learning problems for Fari include:

- Human-state recognition
- Behavioral-state classification
- Preference modeling
- Interaction adaptation
- Event detection

For example, pose and gaze information may provide evidence about what a person is doing, while repeated interactions may provide information about preferences or daily routines.

The problem can be represented as estimating a latent human state:

$$
P(z_t \mid \mathbf{x}_{1:t}),
$$

where $z_t$ represents an underlying state that is inferred from observations over time.

This is closely related to neural decoding, where a researcher attempts to infer an unobserved behavioral or cognitive state from measured neural activity.

### 2.4 Major Constraints

For eldercare applications, **safety and reliability** are more important than maximizing classification accuracy alone.

A false negative may be particularly costly if the system fails to recognize an important behavioral or health-related event. On the other hand, excessive false positives could make the system unreliable or irritating to users.

Important constraints therefore include:

- False-negative rate
- False-positive rate
- Privacy
- Reliability
- Response latency
- Confidence calibration

The objective is therefore more complex than simply

$$
\max \text{Accuracy}.
$$

Instead, the system must balance performance with safety and uncertainty.

### 2.5 Neuro-ML Analogy

The strongest neuroscience analogy for Fari is **latent-state decoding**.

In an EEG experiment, the researcher does not directly measure a state such as confusion, attention, or motor intention. Instead, electrodes measure electrical signals, and a model attempts to infer the hidden cognitive or behavioral state.

Fari faces a structurally similar problem:

$$
\text{observable behavior}
\rightarrow
\text{feature representation}
\rightarrow
\text{human-state estimate}.
$$

The similarity is therefore in the inference structure rather than in the physical meaning of the measurements.

### 2.6 Closest Prior Project

The closest methodological analogue is my **EEG confusion-classification project**.

In that project, frequency-related EEG features were used to predict a participant-reported cognitive state. The computational structure was

$$
\text{EEG observations}
\rightarrow
\text{features}
\rightarrow
\text{latent cognitive state}.
$$

Fari similarly requires observable behavioral signals to be transformed into representations from which a human-centered state can be inferred.

---

## 3. Senpai — Educational Robot

### 3.1 Product Role

Senpai is InGen's educational robot platform. Its purpose is to support interactive learning, educational activities, language-based interaction, and personalized engagement.

From a computational perspective, an educational robot must do more than recognize external objects. It must estimate something about the learner's current knowledge, engagement, performance, or learning state.

### 3.2 Primary Sensor and Input Modalities

Senpai's most important inputs can be understood as **interaction signals**.

These may include:

- Student responses
- Task performance
- Interaction history
- Speech or language inputs
- Behavioral engagement
- Visual interaction signals

These observations form a temporal sequence:

$$
X =
(\mathbf{x}_1,\mathbf{x}_2,\ldots,\mathbf{x}_T).
$$

The learner's true internal state is not directly observable. Instead, it must be inferred from the pattern of responses over time.

### 3.3 Primary ML Task

Senpai can be interpreted as combining **learner-state estimation** with **adaptive decision making**.

A simplified computational pipeline is

$$
\mathbf{x}_{1:t}
\rightarrow
\hat{z}_t
\rightarrow
a_{t+1},
$$

where:

- $\mathbf{x}_{1:t}$ represents observed student interactions,
- $\hat{z}_t$ represents an estimate of current learner state,
- $a_{t+1}$ represents the next educational action.

The important difference from ordinary classification is that the prediction may influence the next observation. If the system changes lesson difficulty, the student's later responses may also change.

This creates a **closed-loop adaptive system**.

### 3.4 Major Constraints

The main constraint for Senpai is not necessarily millisecond-level latency. Instead, the system must make stable and appropriate personalization decisions.

An incorrect estimate of learner ability could repeatedly select material that is too easy or too difficult.

Therefore, uncertainty is important. The system should distinguish between predictions such as

$$
P(\text{mastery}\mid X)=0.95
$$

and

$$
P(\text{mastery}\mid X)=0.55.
$$

Important constraints include:

- Personalization quality
- Reliability
- Uncertainty calibration
- Child safety
- Privacy
- Appropriate content selection

### 3.5 Neuro-ML Analogy

Senpai resembles experiments that estimate a continuously changing cognitive state from repeated behavioral or neural observations.

The system is not only asking:

> What object is present?

It is asking something closer to:

> What hidden learner state most likely generated the sequence of observations seen so far?

This is fundamentally a **state-estimation problem**.

### 3.6 Closest Prior Project

The closest prior project is again my **EEG confusion-classification project**.

Confusion was also a latent cognitive variable inferred from measurable signals. However, Senpai introduces an additional component: the inferred state can influence the system's next action.

Therefore, Senpai is closer to a closed-loop decoder than a purely offline classifier.

---

## 4. Sentinel Prime AI — Multimodal Security Intelligence

### 4.1 Product Role

Sentinel Prime AI is a security-oriented physical-AI platform designed to detect potentially important or dangerous events from multiple sensor streams.

Its computational workflow can be summarized as

$$
\text{Sense}
\rightarrow
\text{Classify}
\rightarrow
\text{Gate}
\rightarrow
\text{Act}.
$$

This makes Sentinel particularly useful for neuro-ML comparison because its architecture strongly resembles a multichannel signal-processing and classification pipeline.

### 4.2 Primary Sensor Modalities

Sentinel uses multiple environmental sensing modalities, including combinations of:

- RGB imaging
- Thermal imaging
- LiDAR
- Radar
- Acoustic sensing
- Environmental sensing
- Infrared sensing
- Vibration-related sensing

The important computational feature is that these measurements describe the same external environment from different perspectives.

For EEG, neural data can be represented as

$$
X_{\text{EEG}} \in \mathbb{R}^{C \times T},
$$

where $C$ is the number of electrodes and $T$ is the number of time points.

For a multimodal physical-AI system, the observation space can instead be represented as

$$X_{\text{robot}} = \left[X_{\text{RGB}}, X_{\text{thermal}}, X_{\text{LiDAR}}, X_{\text{radar}}, X_{\text{audio}}, \ldots \right]$$

The dimensions and physical units differ, but both systems require preprocessing, synchronization, representation learning, and integration across channels.

### 4.3 Primary ML Tasks

Sentinel performs several related ML tasks, including:

- Object detection
- Threat detection
- Pose estimation
- Acoustic event classification
- Environmental anomaly detection
- Multimodal evidence integration
- Confidence estimation

The overall architecture can be written abstractly as

$$
\hat{y} = g\left(
f_1(X_{\mathrm{RGB}}),
f_2(X_{\mathrm{thermal}}),
f_3(X_{\mathrm{audio}}),
f_4(X_{\mathrm{radar}}),
\ldots
\right)
$$

where each $f_i$ extracts modality-specific information and $g$ combines the resulting evidence.

### 4.4 Uncertainty as a Decision Variable

A safety-oriented system should not only generate a class label. It should also estimate whether the prediction is reliable enough to act on.

Instead of simply producing

$$
\hat{y}=\text{threat},
$$

the system should also consider a confidence or uncertainty quantity such as

$$
P(y=\text{threat}\mid x).
$$

If stochastic predictions are generated,

$$
\hat{y}^{(1)},\hat{y}^{(2)},\ldots,\hat{y}^{(K)},
$$

their predictive mean can be estimated as

$$
\bar{y} =
\frac{1}{K}
\sum_{k=1}^{K}
\hat{y}^{(k)}.
$$

Variation across predictions can then provide an uncertainty estimate.

### 4.5 Major Constraints

Sentinel has strong real-time and safety constraints.

Important operational requirements include:

- Low latency
- Low false-negative rate
- Low false-alert rate
- Robustness to noisy sensor data
- Reliable uncertainty estimates

Conceptually, the engineering objective can be written as

$$
\min
\left[
L_{\text{classification}}
+
\lambda_1L_{\text{latency}}
+
\lambda_2L_{\text{false-alert}}
+
\lambda_3L_{\text{uncertainty}}
\right].
$$

This expression is a conceptual representation rather than a specific published InGen loss function.

### 4.6 Neuro-ML Analogy

Sentinel has one of the strongest direct analogies to neural decoding.

A typical neural-decoding pipeline performs

$$
\text{electrode signals}
\rightarrow
\text{preprocessing}
\rightarrow
\text{feature extraction}
\rightarrow
\text{classification}.
$$

Sentinel performs a similar abstract operation:

$$
\text{physical sensors}
\rightarrow
\text{preprocessing}
\rightarrow
\text{modality-specific representations}
\rightarrow
\text{fusion}
\rightarrow
\text{classification}.
$$

Both systems must extract task-relevant information from noisy, high-dimensional measurements.

### 4.7 Closest Prior Project

The closest methodological analogues are my **EEG classification project** and **M1 LFP reaching-behavior project**.

The general computational structure is

$$
X(t)
\rightarrow
\phi(X)
\rightarrow
P(y\mid\phi(X)),
$$

where $\phi(X)$ represents a feature transformation.

In EEG or LFP analysis, $X(t)$ represents neural activity. In Sentinel, it represents multimodal physical sensor streams.

---

## 5. Aido Rover — Autonomous Outdoor Patrol and Inspection

### 5.1 Product Role

Aido Rover represents the transition from stationary perception to **embodied sensing and autonomous mobility**. Unlike a stationary monitoring system, Rover must continuously observe its environment while moving through it.

From the perspective of this internship, Rover is especially important because its multi-channel telemetry provides the primary robot-domain analogue of multi-channel EEG. The synthetic Rover sensor streams used in later weeks include IMU measurements, motor current, proximity sensing, RSSI, battery state, and task-related variables.

### 5.2 Primary Sensor Modalities

The most relevant Rover signals for the planned neuro-ML transfer are:

- IMU acceleration channels
- Motor current
- Proximity measurements
- RSSI
- Battery state
- Task-status signals

These can be represented as a multichannel time-series input:

$$X_{\text{Rover}} \in \mathbb{R}^{C \times T}$$

where $C$ represents robot sensor channels and $T$ represents time samples.

This has the same abstract structure as multichannel EEG:

$$X_{\text{EEG}} \in \mathbb{R}^{C \times T}$$

The physical interpretation of the channels differs, but both systems contain correlated time-series measurements whose covariance structure may contain information about the underlying system state.

### 5.3 Primary ML Task

The primary ML problem is **operational-mode classification** from multichannel robot sensor data.

Later stages of the internship use operational states such as:

- PATROL
- ALERT
- CHARGING
- FAULT

The classification problem can be written as:

$$X_{\text{Rover}} \rightarrow \phi(X) \rightarrow \hat{y}_{\text{operational mode}}$$

where $\phi(X)$ represents signal-processing or feature-extraction operations.

A central planned method is the direct transfer of Common Spatial Patterns and Linear Discriminant Analysis from EEG motor-imagery classification to Rover sensor classification.

### 5.4 Major Constraints

The main constraints are:

- Real-time inference
- Reliable operational-state detection
- Robustness to noisy or correlated sensor channels
- Fault detection
- Safe navigation and control

Unlike an offline neural-signal classification problem, errors in robot state estimation may influence physical behavior. Therefore, reliability and latency have operational consequences.

### 5.5 Neuro-ML Analogy

The strongest analogy is **multi-channel state decoding**.

In EEG motor-imagery decoding, spatially distributed electrode signals are analyzed to determine an underlying motor state.

For Rover, multiple telemetry channels are analyzed to determine an underlying operational state.

The correspondence is:

$$\text{EEG channels} \leftrightarrow \text{robot telemetry channels}$$

$$\text{motor-imagery class} \leftrightarrow \text{operational-mode class}$$

The similarity is therefore mathematical rather than physical.

### 5.6 Closest Prior Project

The closest methodological analogue for Aido Rover is my **EEG Motor Imagery Classification project using Common Spatial Patterns (CSP) and Linear Discriminant Analysis (LDA)**.

In the EEG motor-imagery pipeline, CSP identifies spatial projections that maximize differences in class-dependent variance. The CSP filters are obtained from class-conditional covariance matrices using the generalized eigenvalue problem:

$$R_1w=\lambda R_2w$$

The filtered signals are then converted into discriminative features, commonly using log-variance, and classified using LDA:

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{motor imagery}}$$

The Rover pipeline follows the same mathematical structure:

$$X_{\text{Rover}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{operational mode}}$$

The major difference is the interpretation of the channels. In EEG, the channels are scalp electrodes measuring neural activity. For Rover, the channels are physical telemetry streams such as IMU axes, motor current, and proximity measurements.

The covariance decomposition, generalized eigenvalue calculation, feature extraction, and discriminant classification remain mathematically equivalent. This makes the EEG motor-imagery project the most direct methodological precedent for Aido Rover analysis.

---

## 6. InGen Humanoid — Generalist Embodied Intelligence

### 6.1 Product Role

The InGen Humanoid represents the most direct connection between the neuroscience and physical-AI components of this internship.

A humanoid system must recognize its current motion state from distributed body sensors while coordinating multiple degrees of freedom during behaviors such as walking, reaching, balancing, and recovering from instability.

The central neuro-ML question is therefore how multichannel sensor patterns can be transformed into an estimate of the robot's current motor state.

### 6.2 Primary Sensor Modalities

For the planned BCI-to-Humanoid bridge, the most relevant signals are:

- Joint-angle sensors
- Body-mounted IMU signals
- Other proprioceptive motion measurements

These measurements can be represented as:

$$X_{\text{Humanoid}} \in \mathbb{R}^{C \times T}$$

where each channel corresponds to a spatially distributed measurement of the robot's body state.

This provides a direct structural analogy to EEG motor-imagery data, where multiple scalp electrodes record spatially distributed neural signals over time.

### 6.3 Primary ML Task

The primary task is **motion-mode classification**.

Representative motion primitives include:

- WALK
- REACH
- BALANCE

The classification problem can be written as:

$$X_{\text{joint/IMU}} \rightarrow \phi(X) \rightarrow \hat{y}_{\text{motion primitive}}$$

The internship specifically applies a CSP+LDA-style pipeline to this problem.

### 6.4 Major Constraints

Humanoid control introduces several important constraints:

- Real-time state recognition
- Physical safety
- Stable motion execution
- Robustness to noisy joint and IMU measurements
- Coordination across multiple degrees of freedom

Errors in motion-state inference may directly influence physical control, making reliable classification especially important.

### 6.5 Neuro-ML Analogy

The strongest formal analogy is between **EEG motor-imagery classification and humanoid motion-state classification**.

In EEG motor imagery, the channels are spatially distributed scalp electrodes and the classes represent motor states such as imagined left- versus right-hand movement.

For the Humanoid, the channels are spatially distributed joint or IMU sensors and the classes represent robot motion primitives.

The mapping is:

$$\text{scalp EEG channels} \leftrightarrow \text{joint/IMU sensor channels}$$

$$\text{left/right motor imagery} \leftrightarrow \text{robot motion primitives}$$

$$\text{CSP+LDA decoding} \leftrightarrow \text{CSP+LDA motion-state classification}$$

Thus, the shared structure is not simply that both systems involve movement. Both attempt to identify a motor state from a spatial pattern distributed across multiple sensor channels.

### 6.6 Closest Prior Projects

Two prior projects provide complementary connections to the Aido Humanoid.

#### Methodological Analogue: EEG Motor Imagery Classification

The strongest mathematical analogue is my **EEG motor imagery classification project using CSP and LDA**.

The EEG pipeline can be summarized as:

$$X_{\text{EEG}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{left/right}}$$

The corresponding Humanoid pipeline is:

$$X_{\text{joint/IMU}} \rightarrow \text{CSP} \rightarrow Z_{\text{CSP}} \rightarrow \text{LDA} \rightarrow \hat{y}_{\text{WALK/REACH/BALANCE}}$$

In both cases, the classifier attempts to recover the current motor state from a spatial pattern distributed across multiple channels.

The exact meaning of the classes differs, but the mathematical classification problem is closely aligned.

#### Physical-AI and Control Analogue: Bionic Prosthetic Arm

My **bionic prosthetic arm project** provides a second, system-level connection.

The prosthetic arm combined:

- Mechanical structure
- Arduino-based embedded control
- Motion actuation
- A Java Android application for user control

Its basic architecture can be represented as:

$$\text{input command} \rightarrow \text{embedded controller} \rightarrow \text{actuation} \rightarrow \text{physical motion}$$

A humanoid system is substantially more complex, but it follows a related physical-AI architecture:

$$\text{sensor state} \rightarrow \text{signal processing / control} \rightarrow \text{multi-DOF actuation} \rightarrow \text{robot motion}$$

The prosthetic arm is therefore not the primary mathematical analogue of the Humanoid classification task. Instead, it provides practical intuition for how software, embedded control, mechanical structure, and physical actuation interact within an embodied robotic system.

---

### 8.3 SEOM — Semantic / State Encoding

Among the PIC 2.0 concepts, SEOM is most naturally interpreted as a **semantic or state-encoding model**: a system that transforms high-dimensional observations into a representation that captures information relevant to the underlying state.

The general operation can be represented as:

$$z=f_{\theta}(x)$$

where $x$ represents high-dimensional observations and $z$ represents a learned or extracted state representation.

A downstream decoder can then estimate a state:

$$\hat{s}=g(z)$$

#### Neuroscience Analogy: LFP Reaching-Behavior Decoding

The closest neuroscience analogue in my prior work is the **M1 LFP reaching-behavior decoding project**.

In that project, neural recordings were used to predict whether the subject was in a behavioral state associated with movement:

$$X_{\text{LFP}} \rightarrow \phi(X) \rightarrow \hat{y}_{\text{behavior}}$$

The corresponding physical-AI structure is:

$$X_{\text{sensor}} \rightarrow \phi(X) \rightarrow \hat{s}_{\text{environment or robot}}$$

The important similarity is **state encoding and decoding**. In both domains, the measured signal itself is not the final variable of interest. Instead, the signal contains information about an underlying state that must be recovered computationally.

SEOM is therefore most closely related to neural decoding methods that transform high-dimensional physiological signals into representations of behavioral state, rather than to a specific sensor modality.

---

## 9. Cross-Platform Neuro-ML Comparison

| Platform | Dominant Input | Primary ML Problem | Most Important Constraint | Closest Prior Project |
|---|---|---|---|---|
| **Fari** | Human behavioral, interaction, and monitoring signals | Behavioral-state / anomaly detection | Safety, false negatives, privacy | EEG confusion prediction |
| **Senpai** | Learner interaction and behavioral signals | Student engagement / cognitive-state classification | Reliable personalization | EEG confusion prediction |
| **Sentinel Prime AI** | Continuous multimodal environmental sensor streams | Anomaly and security-event detection | Real-time detection, false alerts | EEG confusion prediction: frequency analysis + shallow NN |
| **Aido Rover** | IMU, motor current, proximity, RSSI, and related telemetry | Operational-mode classification | Real-time reliability and physical safety | **EEG motor imagery classification with CSP + LDA** |
| **Aido Humanoid** | Joint, IMU, and proprioceptive motion signals | Motion-primitive classification | Physical safety and real-time control | **EEG motor imagery CSP/LDA + bionic prosthetic arm** |

The projects therefore form several distinct methodological bridges rather than one generic neuroscience analogy.

The EEG motor-imagery project provides the strongest direct mathematical transfer to Aido Rover and Aido Humanoid because both involve multi-channel measurements, class-dependent covariance structure, spatial filtering, and state classification.

The EEG confusion-prediction project provides the stronger precedent for Fari, Senpai, and Sentinel because its central problem is extracting signal features and predicting a behavioral or anomalous state.

The LFP-versus-calcium project contributes a different methodological perspective: comparing the predictive value of different sensing modalities. This becomes especially relevant when evaluating which physical sensor modalities contribute most strongly to robot operational-state classification.

Finally, the bionic prosthetic arm provides a system-level rather than statistical bridge to Aido Humanoid by connecting signal processing and software control to physical actuation.
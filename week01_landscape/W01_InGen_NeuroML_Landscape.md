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

Aido Rover represents the transition from passive sensing to **embodied sensing and autonomous mobility**.

A stationary security system primarily observes its environment. Rover must observe the world while also moving through it.

This creates a more difficult computational problem because actions affect future sensor observations.

### 5.2 Primary Sensor Modalities

The most relevant sensing modalities for a mobile robotic platform include environmental and navigation-related signals such as:

- Cameras
- LiDAR
- Position information
- Orientation information
- Environmental observations
- Other multimodal navigation sensors

At a computational level, the robot's state can be represented as

$$
\mathbf{s}_t =
[
x_t,
y_t,
\theta_t,
v_t,
\mathbf{o}_t,
\ldots
],
$$

where:

- $x_t$ and $y_t$ represent position,
- $\theta_t$ represents orientation,
- $v_t$ represents velocity,
- $\mathbf{o}_t$ represents environmental observations.

### 5.3 Primary ML Tasks

Rover introduces several coupled computational problems:

- Localization
- Mapping
- Obstacle detection
- Anomaly detection
- Navigation
- Path planning
- Sensor fusion

The robot must estimate its state from observations:

$$
P(s_t \mid z_{1:t}),
$$

where $z_{1:t}$ represents sensor measurements up to time $t$.

It must then choose an action:

$$
a_t=\pi(s_t),
$$

where $\pi$ represents a policy or controller.

The full problem can therefore be summarized as

$$
\text{perception}
\rightarrow
\text{state estimation}
\rightarrow
\text{action selection}.
$$

### 5.4 Major Constraints

Rover's main constraint is **real-time embodied safety**.

A classification error in an offline neuroscience experiment may reduce model accuracy. A perception error in a moving robot can produce a physical collision.

Important constraints therefore include:

- Real-time response
- Collision avoidance
- Robust localization
- Sensor reliability
- Uncertainty handling
- Safe failure behavior

The robot repeatedly performs the loop

$$
\text{sense}
\rightarrow
\text{estimate}
\rightarrow
\text{plan}
\rightarrow
\text{act}
\rightarrow
\text{sense}.
$$

### 5.5 Neuro-ML Analogy

Rover is particularly analogous to **sensorimotor neuroscience**.

The nervous system does not simply classify sensory signals. Sensory information contributes to an internal estimate of body and environmental state, which influences motor output.

The same closed-loop structure occurs in robotics:

$$
s_t
\xrightarrow{\text{policy}}
a_t
\xrightarrow{\text{environment}}
s_{t+1}.
$$

The strongest analogy is therefore not between individual sensors, but between the continuous feedback architecture.

### 5.6 Closest Prior Project

My **M1 LFP reaching-behavior project** provides the strongest conceptual connection.

In that project, M1 neural activity was analyzed around movement onset, linking neural state to motor behavior.

Both problems exist near the boundary between

$$
\text{state representation}
\leftrightarrow
\text{motor behavior}.
$$

The neuroscience project analyzes biological sensorimotor representations, whereas Rover implements an artificial sensorimotor loop.

---

## 6. InGen Humanoid — Generalist Embodied Intelligence

### 6.1 Product Role

The InGen Humanoid represents a more general form of embodied artificial intelligence.

A humanoid robot must coordinate perception, internal body state, planning, manipulation, locomotion, and interaction with humans.

The major challenge is therefore not simply increasing the number of sensors or actuators. It is integrating many different computational systems into a stable closed-loop architecture.

### 6.2 Primary Sensor Modalities

At a general robotics level, humanoid control requires both external and internal information.

These can be separated into:

$$
X =
[
X_{\text{exteroceptive}},
X_{\text{proprioceptive}}
].
$$

**Exteroceptive signals** describe the external environment.

**Proprioceptive signals** describe the robot's own configuration and movement.

This separation has a direct biological analogue because the nervous system also combines external sensory information with proprioceptive information during movement control.

### 6.3 Primary ML Tasks

A generalist humanoid may require simultaneous solutions to:

- Visual perception
- State estimation
- Object representation
- Instruction understanding
- Motion planning
- Manipulation
- Locomotion
- Feedback control
- Multimodal integration

The architecture is inherently hierarchical.

A high-level goal such as

> Bring an object to the user.

must eventually be converted into low-level movement commands.

Conceptually:

$$
\text{goal}
\rightarrow
\text{task plan}
\rightarrow
\text{motion plan}
\rightarrow
\text{motor command}.
$$

### 6.4 Major Constraints

Humanoid robotics combines most of the constraints present in the previous platforms:

- Latency
- Safety
- Uncertainty
- Generalization
- Multimodal integration
- Temporal dependency
- Physical stability

A humanoid cannot therefore be viewed as one large classifier. It is better understood as a collection of interacting inference and control systems.

### 6.5 Neuro-ML Analogy

The strongest analogy is **hierarchical sensorimotor processing in the nervous system**.

Biological movement depends on interactions among sensory systems, cortical representations, motor planning, proprioception, feedback control, and continuous error correction.

A humanoid similarly requires repeated transformations:

$$
\text{sensation}
\rightarrow
\text{representation}
\rightarrow
\text{planning}
\rightarrow
\text{action}
\rightarrow
\text{feedback}.
$$

The analogy is architectural rather than biological.

### 6.6 Closest Prior Project

My **M1 LFP reaching-behavior analysis** is the closest direct neuroscience analogue because M1 provides an example of internal neural representations associated with movement.

The relationship can be represented as

$$
\text{M1 neural activity}
\rightarrow
\text{movement state},
$$

compared with

$$
\text{robot internal representation}
\rightarrow
\text{motor command}.
$$

Both raise the question of how high-dimensional internal representations encode variables relevant to action.

---

## 7. PIC 2.0 and the Shared Intelligence Architecture

### 7.1 One Brain, Many Bodies

A central idea behind InGen's physical-AI architecture is that multiple products can share common intelligence components rather than requiring completely independent AI systems.

From a machine-learning perspective, this can be interpreted as combining

$$
\text{shared representations}
+
\text{task-specific modules}.
$$

Conceptually, a shared encoder can be written as

$$
z=f_{\theta}(x),
$$

followed by platform-specific prediction functions:

$$
\hat{y}_i=g_{\phi_i}(z).
$$

Here:

- $f_{\theta}$ represents reusable learned structure,
- $z$ represents a shared representation,
- $g_{\phi_i}$ represents a task-specific output module.

This resembles ideas from transfer learning and multi-task learning.

---

## 8. PIC 2.0 Foundation-Model Orientation

The Week 1 specification identifies six PIC 2.0 concepts:

- GRPO
- STUM
- SEOM
- AMDC
- HTD-IRL
- CRL-MRS

These terms should be interpreted carefully. Where the exact proprietary implementation is not publicly available, the goal is to identify the closest open-literature concept rather than claim undocumented implementation details.

### 8.1 GRPO — Policy Optimization

GRPO can be interpreted as belonging to the broader family of **reinforcement-learning policy optimization methods**.

A policy can be represented as

$$
\pi_\theta(a\mid s),
$$

which gives the probability of selecting action $a$ given state $s$.

Learning adjusts the parameter vector $\theta$ so that actions associated with better outcomes become more likely.

A generic objective is

$$
\max_\theta
\mathbb{E}_{\pi_\theta}
\left[
\sum_{t=0}^{T}
\gamma^t r_t
\right].
$$

For physical AI, this type of framework is relevant whenever the system must improve action selection based on feedback.

The neuroscience analogy is reinforcement learning, where behavior changes according to reward or outcome signals.

### 8.2 STUM — Predictive Uncertainty

STUM can be connected to the open-literature concept of **predictive uncertainty estimation**.

Instead of treating a model prediction as certain, the system estimates how reliable the prediction is.

If multiple stochastic forward passes produce

$$
\hat{y}^{(1)},
\hat{y}^{(2)},
\ldots,
\hat{y}^{(K)},
$$

the predictive mean can be estimated as

$$
\bar{y} =
\frac{1}{K}
\sum_{k=1}^{K}
\hat{y}^{(k)}.
$$

The predictive variance is

$$
\sigma^2 =
\frac{1}{K}
\sum_{k=1}^{K}
\left(
\hat{y}^{(k)}-\bar{y}
\right)^2.
$$

A high value of $\sigma^2$ indicates that the model is less certain.

The neural-decoding analogue would be reporting not only a predicted motor or cognitive state but also confidence in that prediction.

### 8.3 SEOM — State or Semantic Encoding

SEOM can be interpreted as a **state-encoding or semantic-representation model**.

The central idea is to transform high-dimensional observations into a lower-dimensional internal representation:

$$
z=f_\theta(x).
$$

The learned representation $z$ should contain information relevant to downstream prediction or decision making.

This is directly analogous to neural decoding, where high-dimensional neural signals are transformed into a representation from which behavior or cognitive state can be predicted.

For example:

$$
X_{\text{LFP}}
\rightarrow
z
\rightarrow
\hat{y}_{\text{movement}}.
$$

A robot could use the same abstract structure:

$$
X_{\text{sensor}}
\rightarrow
z
\rightarrow
\hat{s}_{\text{environment}}.
$$

### 8.4 AMDC — Multimodal Alignment and Calibration

AMDC can be interpreted through the open-literature concept of **multimodal sensor alignment and calibration**.

Different sensors observe the same event in different coordinate systems.

A coordinate transformation can be written as

$$
\mathbf{x}_B =
R\mathbf{x}_A+t,
$$

where:

- $R$ is a rotation matrix,
- $t$ is a translation vector.

Before information from multiple sensors can be fused, their spatial and temporal measurements must be aligned.

The neuroscience analogue is aligning recordings from different channels, brain regions, or modalities before comparing or integrating them.

### 8.5 HTD-IRL — Inverse Reinforcement Learning

The suffix **IRL** naturally connects to the established field of **inverse reinforcement learning**.

In ordinary reinforcement learning, the reward function is known and the agent learns a policy.

In inverse reinforcement learning, the goal is reversed.

Given observed behavior

$$
\tau =
(s_0,a_0,s_1,a_1,\ldots,s_T),
$$

the system attempts to infer a reward function

$$
R(s,a)
$$

that could explain that behavior.

The relationship is therefore

$$
\text{observed actions}
\rightarrow
\text{inferred objective}.
$$

This is conceptually related to neural decoding, where measurable activity is used to infer an underlying intention or behavioral variable.

### 8.6 CRL-MRS — Multi-Agent and Multi-Robot Learning

CRL-MRS can be interpreted in relation to **cooperative multi-agent reinforcement learning and multi-robot systems**.

Instead of optimizing the behavior of one robot, the system must coordinate several agents.

For $N$ robots, the joint state can be represented as

$$
\mathbf{s}_t =
(s_t^1,s_t^2,\ldots,s_t^N),
$$

and the joint action as

$$
\mathbf{a}_t =
(a_t^1,a_t^2,\ldots,a_t^N).
$$

The objective becomes

$$
\max_{\pi_1,\ldots,\pi_N}
\mathbb{E}
\left[
\sum_t
\gamma^t
R(\mathbf{s}_t,\mathbf{a}_t)
\right].
$$

The important challenge is coordination.

The neuroscience analogy is distributed computation: useful system-level behavior can emerge through interactions among multiple processing units rather than one isolated unit.

---

## 9. Cross-Platform Neuro-ML Comparison

| Platform | Dominant Input | Primary Computational Problem | Critical Constraint | Closest Neuro-ML Analogy |
|---|---|---|---|---|
| Fari | Human behavioral, visual, and interaction signals | Human-state inference | Safety, privacy, reliability | Latent cognitive-state decoding |
| Senpai | Learner interactions | Learner-state estimation and adaptation | Personalization and uncertainty | Cognitive-state decoding |
| Sentinel Prime | Multimodal environmental sensors | Detection, fusion, and uncertainty estimation | False alerts, latency, safety | Multichannel neural decoding |
| Aido Rover | Navigation and environmental signals | State estimation, navigation, and control | Real-time physical safety | Sensorimotor integration |
| InGen Humanoid | External and internal body-state signals | Perception, planning, and control | Safety, latency, generalization | Hierarchical sensorimotor processing |

Despite the different applications, all five platforms can be represented using variants of the same computational structure:

$$
X
\xrightarrow{\text{representation}}
Z
\xrightarrow{\text{inference}}
\hat{S}
\xrightarrow{\text{decision}}
A.
$$

The meaning of each variable changes across platforms.

For Fari:

$$
\hat{S}=\text{estimated human state}.
$$

For Senpai:

$$
\hat{S}=\text{estimated learner state}.
$$

For Sentinel:

$$
\hat{S}=\text{estimated environmental threat state}.
$$

For Rover:

$$
\hat{S}=\text{estimated spatial and environmental state}.
$$

For a humanoid:

$$
\hat{S}=\text{estimated environmental and body state}.
$$

---

## 10. Conclusion

Viewing InGen's product ecosystem through a neuroscience and machine-learning lens reveals a common computational problem beneath apparently different applications.

Neural-signal research begins with incomplete and noisy measurements of an underlying biological system. Useful information must be extracted from those measurements before a model can infer behavior or cognitive state.

Physical AI faces the same abstract challenge. Sensors provide incomplete measurements of the world, and the system must construct a useful representation from which it can estimate state and select an appropriate action.

The most important bridge is therefore not a superficial comparison between individual sensors. A camera is not an EEG electrode, and LiDAR is not an LFP recording.

The deeper similarity is mathematical:

$$
\boxed{
\text{noisy multichannel observations}
\rightarrow
\text{representation}
\rightarrow
\text{latent-state inference}
\rightarrow
\text{decision}
}
$$

As physical-AI systems progress from relatively passive human-state estimation toward mobile and humanoid robotics, the architecture also becomes increasingly closed-loop.

A complete embodied system follows the cycle

$$
\text{perception}
\rightarrow
\text{state estimation}
\rightarrow
\text{action}
\rightarrow
\text{new perception}.
$$

This same transition is important in computational neuroscience. Neural decoding alone describes how internal or behavioral states can be inferred from neural signals. Sensorimotor neuroscience extends this idea by asking how representations participate in continuous perception-action loops.

For this reason, moving from neural-signal analysis to physical AI is not a transition away from neuroscience methodology. Many of the same mathematical questions — representation, dimensionality, uncertainty, multimodal integration, temporal dynamics, decoding, and feedback — reappear in a different physical system.
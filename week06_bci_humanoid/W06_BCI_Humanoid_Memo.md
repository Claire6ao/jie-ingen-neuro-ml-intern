# W06 BCI–Humanoid Research Memo

## 1. BCI-to-Robot Formal Analogy

This week's work establishes a formal connection between EEG-based brain-computer interface (BCI) motor imagery classification and motion-state classification for the Aido Humanoid. Although the two systems use different sensor modalities, they share the same underlying signal-processing structure: a motor state is represented by a spatial pattern of activity distributed across multiple sensor channels, and the classification task is to infer the current motor state from that pattern.

In the EEG motor imagery task, a participant imagines performing a movement, such as moving the left or right hand. EEG electrodes distributed across the scalp record simultaneous neural activity from multiple spatial locations. Common Spatial Patterns (CSP) learns spatial filters that emphasize differences in signal variance between motor imagery classes. The resulting CSP features can then be classified using Linear Discriminant Analysis (LDA).

The Aido Humanoid problem can be formulated in the same way. Instead of scalp electrodes, the channels are sensors distributed across the robot body. Instead of imagined movements, the classes represent physical motion primitives such as WALK, REACH, and BALANCE. Joint-angle or IMU measurements provide multichannel time-series signals describing the robot's current kinematic state. CSP can therefore be used to identify spatial combinations of sensors whose variance patterns distinguish one motion state from another.

The correspondence can be summarized as:

| EEG Motor Imagery | Aido Humanoid |
|---|---|
| Scalp EEG electrodes | Joint / IMU sensors |
| Neural activity | Kinematic sensor activity |
| Left/right motor imagery | WALK/REACH/BALANCE |
| Spatial neural pattern | Spatial joint-motion pattern |
| CSP spatial filtering | CSP spatial filtering |
| LDA motor-state classification | LDA motion-primitive classification |

Thus, the analogy is not based on the signals being biologically or physically identical. Rather, it is based on their mathematical representation: both are multichannel temporal signals in which different motor states generate distinguishable spatial covariance patterns.

## 2. CSP+LDA on Aido Joint-Sensor Data

To test this analogy, a synthetic Aido Humanoid dataset was generated for three motion primitives: WALK, REACH, and BALANCE. The dataset contained 300 trials, with 100 trials per class. Each trial consisted of six simulated joint-angle sensor channels and 200 temporal samples. A fixed random seed (`SEED = 42`) was used for reproducibility, and realistic noise was added to introduce trial-to-trial variation.

Each motion primitive was designed to produce a distinct kinematic pattern. WALK emphasized alternating lower-body motion, REACH emphasized coordinated shoulder and elbow activity, and BALANCE consisted of lower-amplitude coordinated postural adjustments. These differences created class-dependent spatial covariance structures analogous to the class-dependent spatial patterns observed across EEG electrodes during motor imagery.

Because the original CSP formulation is binary while the Aido task contains three classes, the Week 3 CSP pipeline was extended using a one-vs-rest strategy. Separate CSP filter banks were learned for WALK vs. REST, REACH vs. REST, and BALANCE vs. REST. Two filters from each eigenvalue extreme were retained for each comparison, producing four features per filter bank and 12 CSP log-variance features in total. These features were then classified using LDA.

The CSP+LDA pipeline achieved a held-out test accuracy of **1.000** and a macro F1 score of **1.000**. The CSP feature visualizations also showed strong separation among the three motion primitives.

For comparison, the EEG motor imagery benchmark using the BCI Competition IV Dataset 2a achieved approximately **77.9% mean 10-fold cross-validation accuracy** across nine subjects for left-vs-right motor imagery using CSP+LDA. The lower EEG performance is expected because real EEG contains biological variability, measurement noise, artifacts, and overlapping neural activity. In contrast, the synthetic Aido dataset was intentionally constructed with distinct motion-dependent covariance patterns.

Therefore, the perfect Aido performance should not be interpreted as evidence that real humanoid motion classification will necessarily achieve 100% accuracy. Instead, it demonstrates that CSP successfully recovers the designed spatial structure of the synthetic joint-sensor data and supports the proposed BCI-to-robot analogy in a controlled setting.

## 3. Connection to the Bionic Prosthetic Arm

The previous bionic prosthetic arm project provides a simplified precursor to the Aido Humanoid architecture. In the bionic arm system, an Arduino-based embedded controller receives commands from an Android remote application and converts them into mechanical finger flexion and extension. The system therefore maps an input signal through a control pipeline to coordinated physical movement across multiple mechanical degrees of freedom. Aido Humanoid extends the same basic concept to a substantially more complex robotic platform: instead of controlling only finger movements, the system must coordinate multiple joints across the body to execute behaviors such as walking, reaching, and balancing. In both cases, successful control depends on translating signals into meaningful motor states and coordinating multiple mechanical components. The CSP+LDA pipeline explored here adds a complementary sensing layer to this architecture by demonstrating how multichannel sensor signals can be processed to identify the robot's current motion state.

## 4. InGen Deployment Implications

The Week 6 results suggest a potential signal-processing framework for motion-state monitoring in InGen humanoid systems. Joint-angle sensors and body-mounted IMUs naturally produce synchronized multichannel time-series data. A spatial feature extractor such as CSP could transform these signals into compact representations that emphasize differences between motion states, while a lightweight classifier such as LDA could provide real-time identification of motion primitives.

Such a pipeline could potentially support motion monitoring, state verification, or detection of transitions between behaviors. For example, a deployed system could distinguish whether the robot is currently walking, reaching, balancing, or entering a recovery behavior based on its distributed sensor activity. Because CSP and LDA are computationally lightweight compared with large neural-network models, the approach may also be suitable as an interpretable baseline or embedded component within a larger robotics perception and control architecture.

However, deployment on real Aido hardware would require validation beyond the current synthetic experiment. Real joint and IMU measurements would contain sensor noise, calibration differences, environmental disturbances, variation in motion speed, and transitions between primitives that are not represented in the simplified synthetic dataset. Additional motion states, including fall-recovery, would also need to be incorporated. Future work should therefore evaluate the pipeline using real humanoid sensor recordings and determine whether the strong feature separability observed in the synthetic experiment persists under realistic operating conditions.

Overall, Week 6 demonstrates that the conceptual bridge from BCI motor imagery to humanoid motion-state recognition is mathematically coherent. EEG electrodes and humanoid joint sensors measure fundamentally different physical processes, but both can be treated as spatially distributed sensor channels whose covariance structure contains information about motor state. The successful CSP+LDA experiment provides an initial proof of concept for transferring this signal-processing framework from BCI research to humanoid robotics.
# W02 Sensor EDA Memo
## Aido Rover Fleet Telemetry and Sentinel Prime AI Anomaly Detection

### Objective

This Week 2 analysis developed a reproducible synthetic sensor-analysis pipeline for Aido Rover fleet telemetry. The operational dataset represents 30 units over 14 days at 5-minute intervals, with IMU acceleration, motor current, proximity, RSSI, battery state of charge, and task-success measurements. A separate 20 Hz synthetic sensor stream was used for PSD and frequency-band analysis so that the required 0–5 Hz frequency range could be analyzed reliably.

The EDA highlights three findings that are particularly relevant to fleet monitoring and future Sentinel Prime AI anomaly-detection workflows.

### 1. High-frequency sensor energy is a strong indicator of simulated FAULT behavior

The clearest frequency-domain signature appears in the FAULT condition. Motor-current high-band energy (2–5 Hz) is almost perfectly associated with FAULT windows (r = 0.999), while IMU high-band energy shows a similarly strong relationship (r = 0.998). PSD and spectrogram analysis also show substantially greater high-frequency activity during FAULT than during PATROL.

For Aido Rover fleet management, this suggests that frequency-domain features could complement conventional threshold-based monitoring. A robot may exhibit abnormal oscillation or vibration patterns even when an instantaneous sensor value does not independently exceed a fixed threshold. Sentinel Prime AI could therefore use rolling PSD or high-band energy features as candidate anomaly indicators for detecting abnormal motor or motion behavior.

### 2. Operational modes exhibit distinct frequency signatures

The synthetic signals show structured relationships between frequency bands and operational state. PATROL is strongly associated with low-frequency IMU energy (r = 0.982), ALERT with medium-frequency IMU energy (r = 0.983), and FAULT with high-frequency IMU energy (r = 0.998). Motor-current features show a similar pattern, with PATROL associated with low-frequency energy (r = 0.716) and FAULT associated with high-frequency energy (r = 0.999).

This indicates that anomaly detection should consider operational context rather than apply identical expectations to every robot at every time. For example, a sensor pattern that is unusual during PATROL may be expected during ALERT. A Sentinel Prime AI monitoring system could use mode-conditioned baselines to distinguish expected state-dependent behavior from genuinely abnormal deviations.

### 3. Cross-sensor relationships provide useful context but also reveal redundancy

Cross-channel analysis identified a near-perfect correlation between z-axis acceleration and IMU magnitude (r = 0.995), indicating substantial redundant information. In contrast, motor current and RSSI showed a moderately strong negative correlation (r = -0.628). This relationship reflects the synthetic operational design, in which higher-load or FAULT states tend to combine increased motor current with weaker RSSI, rather than demonstrating a direct causal relationship between the two sensors.

For fleet-level monitoring, these results suggest that Sentinel Prime AI should evaluate both individual sensor behavior and relationships across sensors. Redundant features may be reduced during model development, while changes in normally stable cross-sensor relationships could themselves become useful anomaly signals.

### Operational Takeaway

The Week 2 EDA demonstrates that raw telemetry alone does not capture all potentially useful information in robot sensor streams. Frequency-domain features, operational-mode context, and cross-sensor relationships provide complementary views of robot behavior. For Sentinel Prime AI, a practical next step would be to combine these feature types into a mode-aware anomaly-detection pipeline that can flag unusual sensor patterns for fleet-level inspection.

Because the current dataset is synthetic and its mode-specific frequency components were intentionally designed, the strong correlations reported here validate the data-generation and signal-processing pipeline rather than establish empirical properties of deployed Aido Rover hardware. Validation on real fleet telemetry would be required before these patterns could be used as operational fault signatures.
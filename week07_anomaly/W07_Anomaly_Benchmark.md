# Week 7 Anomaly Detection Benchmark

## 1. Overview

This benchmark evaluates four anomaly-detection methods for identifying abnormal robot operational states from the Week 2 synthetic sensor dataset. Five controlled fault types were injected into the 20 Hz high-frequency sensor stream:

* Motor stall
* Proximity sensor drift
* Battery degradation acceleration
* IMU axis failure
* RSSI disruption

Following the neuroscience-inspired EEG confusion-detection framework, frequency-band energy features were extracted from each analysis window using the Week 2 frequency bands:

* Low: 0–0.5 Hz
* Medium: 0.5–2 Hz
* High: 2–5 Hz

Four detection methods were evaluated across 10 random seeds:

1. Frequency-Band Energy Threshold
2. Isolation Forest
3. One-Class SVM
4. Logistic Regression, the best-performing classifier from the Week 4 multi-modal benchmark

Performance was evaluated using precision, recall, F1 score, and AUROC.

---

## 2. Overall Benchmark Results

| Method              | Precision | Recall |    F1 | AUROC |
| ------------------- | --------: | -----: | ----: | ----: |
| Logistic Regression |     0.834 |  0.884 | 0.847 | 0.948 |
| One-Class SVM       |     0.171 |  0.864 | 0.285 | 0.924 |
| Energy Threshold    |     0.361 |  0.812 | 0.494 | 0.883 |
| Isolation Forest    |     0.199 |  0.556 | 0.288 | 0.820 |

Logistic Regression achieved the highest mean AUROC (0.948) and the highest F1 score (0.847). Its precision of 0.834 and recall of 0.884 indicate a relatively balanced trade-off between detecting anomalies and limiting false-positive detections.

One-Class SVM achieved the second-highest AUROC (0.924) and high recall (0.864), but its precision was only 0.171. This indicates that the model was sensitive to abnormal windows but produced substantially more false-positive detections at its current decision boundary.

The neuroscience-inspired Energy Threshold method achieved an AUROC of 0.883. It outperformed Isolation Forest (0.820) in overall AUROC but underperformed One-Class SVM and Logistic Regression. Its relatively high recall (0.812) indicates that spectral deviations can still provide useful anomaly signals.

Isolation Forest produced the lowest overall AUROC and recall among the four evaluated methods.

---

## 3. Performance by Fault Type

The difficulty of anomaly detection varied substantially across the five injected faults.

| Fault Type          | Mean AUROC Across Methods |
| ------------------- | ------------------------: |
| Motor Stall         |                     0.998 |
| RSSI Disruption     |                     0.982 |
| Battery Degradation |                     0.972 |
| Proximity Drift     |                     0.915 |
| IMU Failure         |                     0.603 |

### Motor Stall

Motor stall was the easiest fault to detect, with an average AUROC of approximately 0.998. Logistic Regression, One-Class SVM, and Energy Threshold all achieved an AUROC of 1.000. The simultaneous increase in motor current and decrease in velocity created a strong deviation from normal operating patterns.

### Battery Degradation

Battery degradation was also detected reliably. Both Logistic Regression and Energy Threshold achieved an AUROC of 1.000, while Isolation Forest achieved 0.887. The accelerated decline in battery state of charge created a consistent deviation from the normal signal pattern.

### Proximity Sensor Drift

Logistic Regression, One-Class SVM, and Energy Threshold each achieved an AUROC of 1.000 for proximity drift. Isolation Forest achieved a lower AUROC of 0.659.

### RSSI Disruption

RSSI disruption was highly detectable. Logistic Regression, One-Class SVM, and Energy Threshold achieved AUROC values of 1.000, while Isolation Forest achieved 0.929.

### IMU Axis Failure

IMU failure was the most difficult anomaly to detect, with a mean AUROC of only 0.603 across methods.

| Method              | Precision | Recall |    F1 | AUROC |
| ------------------- | --------: | -----: | ----: | ----: |
| Logistic Regression |     0.169 |  0.420 | 0.233 | 0.742 |
| Isolation Forest    |     0.098 |  0.240 | 0.137 | 0.633 |
| One-Class SVM       |     0.071 |  0.320 | 0.116 | 0.622 |
| Energy Threshold    |     0.042 |  0.060 | 0.049 | 0.417 |

Unlike the other injected faults, IMU failure zeroed only one accelerometer axis while the remaining IMU channels continued to contain normal sensor information. As a result, the overall feature representation changed less dramatically than for faults such as motor stall or RSSI disruption, making IMU failure more difficult to distinguish from normal operating windows.

---

## 4. Wilcoxon Signed-Rank Test

The two methods with the highest overall mean AUROC were:

* Logistic Regression: 0.948
* One-Class SVM: 0.924

A paired Wilcoxon signed-rank test was performed on their AUROC values across the 10 random seeds.

* Wilcoxon statistic: 11.000
* p-value: 0.105469
* Significance level: alpha = 0.05

Because the p-value was greater than 0.05, the difference between Logistic Regression and One-Class SVM was not statistically significant across the 10 seeds.

Although Logistic Regression achieved a higher observed mean AUROC, the benchmark does not provide sufficient evidence to conclude that its AUROC improvement over One-Class SVM is statistically significant.

---

## 5. Neuroscience-Inspired Energy Threshold

The frequency-band Energy Threshold method provides a direct connection to the EEG confusion-prediction framework by representing abnormal robot behavior as deviations in spectral energy.

Its overall AUROC was 0.883, compared with:

* Isolation Forest: 0.820
* One-Class SVM: 0.924
* Logistic Regression: 0.948

The Energy Threshold method therefore outperformed Isolation Forest in mean AUROC but underperformed One-Class SVM and Logistic Regression.

Its performance was strongly dependent on fault type. Energy Threshold achieved an AUROC of 1.000 for motor stall, battery degradation, proximity drift, and RSSI disruption, but only 0.417 for IMU failure. This suggests that frequency-band thresholding is effective when faults generate strong spectral deviations but is less effective for anomalies that produce subtler or more localized changes across sensor channels.

---

## 6. Deployment Recommendations

### Fari — Companion Distress Detection

For Fari, Logistic Regression is the most appropriate method among those evaluated in this benchmark.

Companion distress detection requires a balance between sensitivity and precision. Missing a genuine distress event is undesirable, but excessive false alarms could also reduce the usefulness of the system during everyday interactions.

Logistic Regression achieved both high recall (0.884) and substantially higher precision (0.834) than the other methods, producing the highest F1 score (0.847). In comparison, One-Class SVM achieved similar recall (0.864) but much lower precision (0.171), which would generate substantially more false alarms.

Therefore, Logistic Regression provides the strongest precision-recall balance for a companion-robot distress detection setting.

### Sentinel Prime AI — Security Stream Anomaly Detection

For Sentinel Prime AI, One-Class SVM is a useful candidate when the deployment places greater emphasis on anomaly sensitivity and detecting previously unseen deviations.

One-Class SVM achieved high recall (0.864) and the second-highest AUROC (0.924) without requiring anomalous examples during training. This can be useful in a security-monitoring context where future anomalies may differ from previously labeled fault patterns.

However, its low precision (0.171) indicates a substantial false-positive cost. If Sentinel Prime AI requires a lower false-alarm rate and labeled anomaly examples are available, Logistic Regression provides a more balanced alternative.

Thus, the deployment choice depends on the operational priority: One-Class SVM favors sensitivity to deviations, while Logistic Regression provides substantially stronger precision and overall classification balance.

---

## 7. Conclusion

The Week 7 benchmark demonstrates that robot operational anomalies can be detected using the same general framing used for cognitive-state deviation in EEG analysis.

Logistic Regression achieved the highest overall performance with an AUROC of 0.948 and F1 score of 0.847, although its AUROC advantage over One-Class SVM was not statistically significant according to the paired Wilcoxon test (p = 0.105).

Detection performance depended strongly on fault type. Motor stall, battery degradation, proximity drift, and RSSI disruption produced clear deviations and were detected reliably, whereas IMU axis failure was the most challenging anomaly, with a mean AUROC of only 0.603.

The frequency-band Energy Threshold method achieved strong performance for four of the five faults and an overall AUROC of 0.883. This demonstrates that neuroscience-inspired spectral features can provide a useful and interpretable approach to robot anomaly detection while also revealing limitations for subtler sensor failures.

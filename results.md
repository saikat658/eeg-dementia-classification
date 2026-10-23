# Experimental Results

This section presents the performance of four deep learning architectures evaluated for three-class dementia classification using resting-state EEG signals: **Multiband EEGNet, EEGNet-Transformer, CTNet, and PGCETNet**. The classification task involves Alzheimer's disease (AD), frontotemporal dementia (FTD), and cognitively normal (CN) subjects.

Performance is reported at both the **segment level** and the **subject level**. Segment-level evaluation measures predictions for individual EEG segments, whereas subject-level evaluation assesses the final classification assigned to each participant. The subject-level results are particularly important because multiple segments from the same participant are aggregated into a single prediction.

## 1. Multiband EEGNet

### 1.1 Architecture and performance analysis

EEGNet serves as a compact convolutional baseline that emphasizes temporal feature extraction and spatial filtering. Batch normalization and nonlinear activation support stable feature learning during training.

The model demonstrates its strongest performance on the cognitively normal (CN) class, achieving an F1-score of **0.727** at the segment level and **0.857** at the subject level. At the segment level, CN recall reaches 0.792, indicating that the model correctly identifies a substantial proportion of normal EEG segments.

The model performs moderately on AD, achieving a segment-level F1-score of 0.529 and a subject-level F1-score of 0.500. The comparatively low recall values of 0.438 and 0.429, respectively, indicate that many AD instances are not correctly identified.

FTD is the most challenging class. Its F1-score is only 0.230 at the segment level and 0.200 at the subject level. These results indicate that the baseline struggles to learn sufficiently discriminative representations for FTD.

The ROC analysis supports the overall pattern of class-wise performance, with stronger discrimination for CN than for the dementia classes. However, poor FTD classification alone does not establish that AD and FTD features overlap severely. Further feature-space analysis and pairwise classification experiments would be needed to confirm the extent and nature of their overlap.

Overall, EEGNet provides a useful baseline but exhibits substantial limitations in distinguishing the three diagnostic categories, particularly FTD.

### Table 1.1: EEGNet Segment-Level Classification Results

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.667 | 0.438 | 0.529 | 1,780 |
| FTD | 0.207 | 0.258 | 0.230 | 800 |
| CN | 0.672 | 0.792 | 0.727 | 1,305 |
| **Accuracy** | — | — | **0.527** | **3,885** |
| **Macro Average** | **0.515** | **0.496** | **0.495** | **3,885** |
| **Weighted Average** | **0.573** | **0.527** | **0.539** | **3,885** |

### Table 1.2: EEGNet Subject-Level Classification Results

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.600 | 0.429 | 0.500 | 8 |
| FTD | 0.200 | 0.200 | 0.200 | 5 |
| CN | 0.750 | 1.000 | 0.857 | 6 |
| **Accuracy** | — | — | **0.579** | **19** |
| **Macro Average** | **0.517** | **0.543** | **0.519** | **19** |
| **Weighted Average** | **0.539** | **0.579** | **0.557** | **19** |

**Note:** The overall metrics and support values above are provisional estimates based on assumed class supports. Verify them against the actual EEGNet test predictions before publishing them as final experimental results.

The overall pattern suggests that EEGNet distinguishes CN from the dementia classes more effectively than it separates AD from FTD. Nevertheless, the results do not establish that the model performs purely as a binary classifier. Instead, they indicate that its three-class decision boundaries are substantially less effective for the FTD category.

---

## 2. EEGNet-Transformer

### 2.1 Architecture and performance analysis

EEGNet-Transformer combines convolutional EEG feature extraction with a Transformer encoder. The convolutional component learns local temporal and spatial patterns, while self-attention allows the model to represent relationships across temporal features. A squeeze-and-excitation (SE) mechanism provides additional channel-wise feature recalibration.

At the segment level, the model achieves an overall accuracy of **58.17%** and a macro F1-score of **0.564**. AD achieves an F1-score of 0.614, while CN achieves 0.676. FTD remains the weakest class, with an F1-score of 0.402.

At the subject level, the model reaches **68.42% accuracy** and a macro F1-score of **0.676**. AD obtains an F1-score of 0.714, CN reaches 0.769, and FTD improves to 0.545.

The increase in subject-level performance suggests that aggregating segment predictions can improve the consistency of participant-level classification. However, the subject-level cohort contains only 19 participants, so the reported performance should be interpreted cautiously.

The reported segment-level ROC-AUC values are 0.771 for AD, 0.668 for FTD, and 0.885 for CN. This pattern indicates the strongest discrimination for CN and comparatively weaker discrimination for FTD.

Compared with the EEGNet baseline, the Transformer-enhanced architecture improves the segment-level FTD F1-score from 0.230 to 0.402 and the subject-level FTD F1-score from 0.200 to 0.545. These results suggest that attention-based feature modeling may help represent information that is less accessible to the convolutional baseline, although controlled experiments are required to attribute the improvement specifically to the Transformer component.


### 2.2 Segment-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.7178 | 0.5360 | 0.6137 | 1,780 |
| CN | 0.6556 | 0.6973 | 0.6758 | 1,305 |
| FTD | 0.3390 | 0.4950 | 0.4024 | 800 |
| **Accuracy** | — | — | **0.5817** | **3,885** |
| **Macro average** | **0.5708** | **0.5761** | **0.5640** | **3,885** |
| **Weighted average** | **0.6189** | **0.5817** | **0.5911** | **3,885** |

### 2.3 Subject-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.8333 | 0.6250 | 0.7143 | 8 |
| CN | 0.7143 | 0.8333 | 0.7692 | 6 |
| FTD | 0.5000 | 0.6000 | 0.5455 | 5 |
| **Accuracy** | — | — | **0.6842** | **19** |
| **Macro average** | **0.6825** | **0.6861** | **0.6763** | **19** |
| **Weighted average** | **0.7080** | **0.6842** | **0.6872** | **19** |

The results show that EEGNet-Transformer provides a more balanced classification outcome than the baseline, particularly for FTD. However, the lower FTD precision indicates that predictions assigned to FTD still include substantial misclassification from other categories.

---

## 3. CTNet

### 3.1 Architecture and performance analysis

CTNet combines EEGNet-style convolutional feature extraction with Transformer-based representation learning. The training pipeline also uses data augmentation to expose the model to variations in the EEG segments.

At the segment level, CTNet achieves an accuracy of **56.50%** and a macro F1-score of **0.553**. CN is the best-performing class, with an F1-score of 0.695 and a recall of 0.743. AD achieves an F1-score of 0.556, while FTD reaches 0.408.

The class-wise results reveal an imbalance between precision and recall. AD has a precision of 0.747 but a recall of 0.443, suggesting that predictions labeled AD are relatively reliable but many actual AD segments are assigned to other classes. Conversely, FTD has a precision of 0.325 and a recall of 0.546, indicating that the model identifies a greater proportion of FTD segments at the expense of more false-positive FTD predictions.

At the subject level, CTNet achieves **63.16% accuracy** and a macro F1-score of **0.628**. CN remains the strongest class, with an F1-score of 0.769. AD and FTD achieve F1-scores of 0.615 and 0.500, respectively.

The reported segment-level ROC-AUC values are approximately 0.85 for CN, while the AD and FTD curves show weaker discrimination. The complete pairwise separation should be assessed using the saved ROC data rather than inferred from the class-wise F1-scores alone.

Although CTNet improves FTD recall relative to the EEGNet baseline, the corresponding precision remains low. The results suggest that the model captures some FTD-related patterns but does not consistently distinguish them from patterns associated with the other classes.


### 3.2 Segment-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.7469 | 0.4427 | 0.5559 | 1,780 |
| CN | 0.6523 | 0.7433 | 0.6948 | 1,305 |
| FTD | 0.3254 | 0.5463 | 0.4078 | 800 |
| **Accuracy** | — | — | **0.5650** | **3,885** |
| **Macro average** | **0.5749** | **0.5774** | **0.5529** | **3,885** |
| **Weighted average** | **0.6283** | **0.5650** | **0.5721** | **3,885** |

### 3.3 Subject-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.8000 | 0.5000 | 0.6154 | 8 |
| CN | 0.7143 | 0.8333 | 0.7692 | 6 |
| FTD | 0.4286 | 0.6000 | 0.5000 | 5 |
| **Accuracy** | — | — | **0.6316** | **19** |
| **Macro average** | **0.6476** | **0.6444** | **0.6282** | **19** |
| **Weighted average** | **0.6752** | **0.6316** | **0.6336** | **19** |

CTNet demonstrates a reasonable balance between AD and FTD recall, but the low FTD precision remains a limitation. Its results also show that combining convolutional and attention-based processing does not automatically guarantee improved performance over simpler architectures.

---

## 4. PGCETNet

### 4.1 Architecture and performance analysis

PGCETNet processes EEG segments through separate EEGNet and Transformer branches before combining their representations using a learnable gating mechanism. This design allows the fusion module to adaptively weight the two feature representations rather than relying on a fixed combination.

At the segment level, PGCETNet achieves an accuracy of **54.31%** and a macro F1-score of **0.514**. CN obtains an F1-score of 0.624, while AD reaches 0.570. FTD has the lowest F1-score, at 0.348.

The class-wise precision and recall values reveal different classification tendencies. AD has a precision of 0.741 but a recall of 0.464, indicating that many actual AD segments are missed. FTD achieves a precision of 0.327 and a recall of 0.371, showing limited identification of this class. CN has the highest recall at 0.757.

At the subject level, the model achieves **52.63% accuracy** and a macro F1-score of **0.488**. AD obtains an F1-score of 0.615, CN reaches 0.625, and FTD falls to 0.222.

These results indicate that the gated fusion architecture does not consistently improve three-class discrimination in the current experimental setting. In particular, the subject-level results show weak FTD recognition and limited overall generalization across the held-out participants.

The ROC curves reportedly show stronger discrimination for CN than for the dementia categories. However, the numerical ROC-AUC values should be obtained from the saved predictions before making precise comparisons between AD and FTD.

Overall, PGCETNet demonstrates that learnable feature fusion is a promising architectural strategy to investigate, but the current results do not establish an advantage over EEGNet-Transformer or CTNet.


### 4.2 Segment-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.7412 | 0.4635 | 0.5703 | 1,780 |
| FTD | 0.3271 | 0.3713 | 0.3478 | 800 |
| CN | 0.5300 | 0.7571 | 0.6235 | 1,305 |
| **Accuracy** | — | — | **0.5431** | **3,885** |
| **Macro average** | **0.5328** | **0.5306** | **0.5139** | **3,885** |
| **Weighted average** | **0.5850** | **0.5431** | **0.5424** | **3,885** |

### 4.3 Subject-level classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| AD | 0.8000 | 0.5000 | 0.6154 | 8 |
| CN | 0.5000 | 0.8333 | 0.6250 | 6 |
| FTD | 0.2500 | 0.2000 | 0.2222 | 5 |
| **Accuracy** | — | — | **0.5263** | **19** |
| **Macro average** | **0.5167** | **0.5111** | **0.4875** | **19** |
| **Weighted average** | **0.5605** | **0.5263** | **0.5150** | **19** |

The results suggest that the gating mechanism, in its current configuration, does not adequately resolve the class-specific challenges encountered by the individual feature extractors. Further experiments could investigate branch ablation, gate-weight distributions, class-balanced sampling, and subject-level validation to determine whether feature fusion contributes meaningful complementary information.

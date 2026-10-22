# Experimental Results

This document presents the classification performance of the deep learning architectures evaluated in the thesis *Subject-Independent EEG-Based Dementia Classification using Deep Learning Architectures: A Leakage-Free Comparative Study*.

## 1. Proposed Multi-Band EEGNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 73.68% | 100.00% | 37.50% | 54.55% |
| FTD | 73.68% | 50.00% | 60.00% | 54.55% |
| CN | 78.95% | 60.00% | 100.00% | 75.00% |
| Overall accuracy | 63.16% | — | — | — |
| Macro average | — | 70.00% | 65.83% | 61.36% |

## 2. Hybrid Bi-LSTM Attention

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.5789 | 0.0000 | 0.0000 | 0.0000 |
| FTD | 0.5263 | 0.3333 | 0.8000 | 0.4706 |
| CN | 0.8421 | 0.7143 | 0.8333 | 0.7692 |
| Overall accuracy | 0.4737 | — | — | — |
| Macro average | — | 0.3492 | 0.5444 | 0.4133 |

## 3. CTNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.5789 | 0.0000 | 0.0000 | 0.0000 |
| FTD | 0.4737 | 0.2727 | 0.6000 | 0.3750 |
| CN | 0.7895 | 0.6250 | 0.8333 | 0.7143 |
| Overall accuracy | 0..4211             |
| Macro average | — | 0.2992 | 0.4778 | 0.3631 |

## 4. EEGNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.579 | 0.000 | 0.000 | 0.000 |
| FTD | 0.263 | 0.263 | 1.000 | 0.417 |
| CN | 0.684 | 0.000 | 0.000 | 0.000 |
| Overall accuracy | 0.2632          |
| Macro average | — | 0.0877 | 0.3330 | 0.1390 |


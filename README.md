# eeg-dementia-classification
Subject-independent EEG-based dementia classification using deep learning architectures: a leakage-free comparative study.
# Subject-Independent EEG-Based Dementia Classification Using Deep Learning Architectures

**A Leakage-Free Comparative Study**

[![Dataset](https://img.shields.io/badge/Dataset-OpenNeuro%20ds004504-blue)](https://openneuro.org/datasets/ds004504/versions/1.0.2)
![Status](https://img.shields.io/badge/Thesis-Completed-success)
![Code](https://img.shields.io/badge/Source%20Code-Not%20Publicly%20Available-lightgrey)
![Domain](https://img.shields.io/badge/Domain-EEG%20%7C%20Deep%20Learning-purple)

## Overview

This repository documents the research conducted for the B.Sc. thesis **"Subject-Independent EEG-Based Dementia Classification using Deep Learning Architectures: A Leakage-Free Comparative Study."**

The study investigates deep learning approaches for classifying three diagnostic groups from resting-state electroencephalography (EEG):

- **Alzheimer's disease (AD)**
- **Frontotemporal dementia (FTD)**
- **Cognitively normal controls (CN)**

The research emphasizes subject-independent evaluation, comparative assessment of deep learning architectures, and performance analysis at both segment and subject levels. A multi-band EEGNet architecture is investigated alongside established deep learning baselines.

The primary objective is to assess model performance under an evaluation protocol designed to prevent EEG segments from the same participant from appearing in both training and evaluation partitions.

## Abstract

Electroencephalography offers a non-invasive approach to investigating brain activity and its potential application in neurological disease classification. However, reliable EEG-based dementia classification remains challenging because of inter-subject variability, similarities between diagnostic groups, class imbalance, and the risk of data leakage during model development.

This study presents a comparative investigation of deep learning architectures for distinguishing Alzheimer's disease, frontotemporal dementia, and cognitively normal subjects using resting-state EEG. The dataset was obtained from OpenNeuro (ds004504), comprising 87 usable subjects after excluding one subject during quality control: 36 AD, 22 FTD, and 29 CN participants.

The experimental framework uses 19-channel EEG recordings sampled at 500 Hz and non-overlapping four-second segments. Subject-level data separation is performed before segmentation to prevent participant overlap between training and evaluation partitions. Grouped cross-validation and held-out subject-level testing are used to assess generalization.

The evaluated architectures include EEGNet, CNN–Transformer, CTNet, Hybrid Bi-LSTM Attention, and a proposed multi-band EEGNet architecture. The proposed architecture employs four parallel temporal-filter branches intended to capture information associated with delta, theta, alpha, and beta frequency ranges. Performance is assessed using accuracy, precision, recall, F1-score, and comparative evaluation across diagnostic classes.

The supplied results indicate that performance varies across architectures and diagnostic groups, highlighting the importance of class-wise evaluation rather than relying exclusively on overall accuracy. Further validation is required to establish the robustness and generalizability of the findings.

## Research Objectives

1. Develop and compare deep learning architectures for three-class dementia classification using resting-state EEG.
2. Implement subject-independent data partitioning to reduce the risk of subject-level data leakage.
3. Investigate multi-band temporal feature extraction using a multi-branch EEGNet architecture.
4. Evaluate classification performance using class-wise and aggregate metrics.
5. Compare segment-level predictions with subject-level predictions where applicable.
6. Examine the limitations associated with inter-subject variability and the relatively small number of participants, particularly in the FTD group.

## Dataset

The study uses the publicly available EEG dataset hosted on OpenNeuro.

| Attribute | Description |
|---|---|
| Dataset identifier | ds004504 |
| Dataset source | OpenNeuro |
| Signal modality | Resting-state EEG |
| Sampling frequency | 500 Hz |
| Number of EEG channels | 19 |
| Segmentation window | 4 seconds |
| Segmentation strategy | Non-overlapping windows |
| Number of usable subjects | 87 |
| Classification task | Three-class classification |

### Subject distribution

| Diagnostic group | Label | Subjects |
|---|---|---:|
| Alzheimer's disease | AD | 36 |
| Frontotemporal dementia | FTD | 22 |
| Cognitively normal controls | CN | 29 |
| **Total** | | **87** |

One subject, identified as `sub-086`, was excluded during quality control.

**Dataset access:** [OpenNeuro ds004504 — Dataset README](https://openneuro.org/datasets/ds004504/versions/1.0.2/file-display/README)

The original dataset should be obtained from its official source. Raw EEG recordings and participant-level data are not included in this repository.

## Methodology

### 1. EEG data preparation

Previously preprocessed EEG recordings were used as the input to the segmentation and classification pipeline. The recordings contain 19 EEG channels sampled at 500 Hz.

No additional filtering or artifact-removal procedures are claimed as part of the segmentation stage described here.

### 2. Segmentation

Continuous EEG recordings are divided into non-overlapping four-second segments.

At a sampling frequency of 500 Hz, each segment contains:

- 2,000 time samples per channel
- 19 EEG channels
- 38,000 channel-sample values per segment

The resulting segments are used as model inputs, with diagnostic labels inherited from their corresponding participants.

### 3. Subject-independent evaluation

To reduce the risk of data leakage, participants are separated into training and evaluation partitions before their EEG recordings are segmented.

The evaluation framework incorporates grouped cross-validation and a held-out subject-level test set. The same participant must not occur in both training and evaluation partitions.

Subject-level separation is an established evaluation practice rather than a standalone methodological novelty of this study. Its purpose here is to support a more credible assessment of generalization to unseen participants.

**Configuration details requiring confirmation:** the exact held-out subject count, cross-validation fold count for each experiment, and whether all reported architectures used identical partitions.

### 4. Deep learning architectures

The comparative study includes the following architectures.

| Architecture | Role in the study |
|---|---|
| EEGNet | Compact convolutional neural network baseline for EEG |
| CNN–Transformer | Convolutional feature extraction combined with attention-based sequence modeling |
| CTNet | Deep learning baseline for EEG classification |
| Hybrid Bi-LSTM Attention | Recurrent sequence modeling with attention |
| Multi-Band EEGNet | Proposed multi-branch temporal-filter architecture |

The availability of results varies by architecture. Only the numerical results supplied for Hybrid Bi-LSTM Attention, CTNet, EEGNet, and the proposed architecture are reported in the current results section. Results for CNN–Transformer require confirmation before inclusion.

### 5. Proposed multi-band EEGNet

The proposed architecture uses four parallel temporal-filter branches designed to learn complementary EEG representations associated with the following conventional frequency bands:

- Delta
- Theta
- Alpha
- Beta

The branch outputs are combined for downstream classification.

The branches are intended to emphasize information associated with these frequency ranges; they should not be interpreted as guaranteeing strict frequency isolation unless this has been verified through the implementation and spectral analysis.

The architecture is evaluated alongside baseline models to investigate whether multi-branch temporal feature extraction is useful for the three-class classification task.

### 6. Evaluation metrics

The study uses the following metrics:

- **Accuracy:** proportion of correct predictions.
- **Precision:** proportion of positive predictions for a class that are correct.
- **Recall:** proportion of actual instances of a class that are correctly identified.
- **F1-score:** harmonic mean of precision and recall.
- **Macro precision, macro recall, and macro F1-score:** unweighted averages across the three diagnostic classes.

Segment-level and subject-level results should be reported separately because they measure different aspects of model performance.

## Experimental Results

The tables below reproduce the supplied results. Missing or inconsistent values are explicitly identified rather than silently corrected.

### 1. Proposed Multi-Band EEGNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 73.68% | 100.00% | 37.50% | 54.55% |
| FTD | 73.68% | 50.00% | 60.00% | 54.55% |
| CN | 78.95% | 60.00% | 100.00% | 75.00% |
| **Overall / macro** | **63.16% overall accuracy** | **70.00% macro** | **65.83% macro** | **61.36% macro** |

The supplied macro F1-score is 61.36%. The displayed class F1-scores yield approximately 61.37% when averaged, a minor rounding difference that should be checked against the original evaluation output.

### 2. Hybrid Bi-LSTM Attention

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.5789 | 0.0000 | 0.0000 | 0.0000 |
| FTD | 0.5263 | 0.3333 | 0.8000 | 0.4706 |
| CN | 0.8421 | 0.7143 | 0.8333 | 0.7692 |
| **Overall accuracy** | **0.4737** | — | — | — |
| **Macro average** | — | **0.3492** | **0.5444** | **0.4133** |

### 3. CTNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.5789 | 0.0000 | 0.0000 | 0.0000 |
| FTD | 0.4737 | 0.2727 | 0.6000 | 0.3750 |
| CN | 0.7895 | 0.6250 | 0.8333 | 0.7143 |
| **Overall accuracy** | **0.4211** | — | — | — |
| **Macro average** | — | **0.2992** | **0.4778** | **0.3631** |

The supplied CTNet overall accuracy was written as `0..4211`. It is intentionally not treated as a verified numeric result in this table.

### 4. EEGNet

| Class | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| AD | 0.579 | 0.000 | 0.000 | 0.000 |
| FTD | 0.263 | 0.263 | 1.000 | 0.417 |
| CN | 0.684 | 0.000 | 0.000 | 0.000 |
| **Overall accuracy** | **0.2632** | — | — | — |
| **Macro average** | — | **0.0877** | **0.3330** | **0.1390** |

### Results interpretation

The supplied results show substantial variation in class-wise performance across the evaluated models. In particular, the baseline results include zero recall for some classes, indicating that aggregate accuracy alone does not adequately characterize model behavior.

The proposed architecture reports a macro F1-score of 61.36% and an overall accuracy of 63.16%. These figures are presented as supplied, but the accuracy definition and consistency of the class-wise metrics should be verified against the original predictions before making direct comparative claims.

**Important:** These tables should not be interpreted as a definitive ranking of architectures until the metrics, test partitions, aggregation procedures, and evaluation levels have been confirmed to be comparable.

## Limitations

The study has several limitations that should be considered when interpreting the results:

1. **Limited cohort size:** The dataset contains 87 usable participants, which restricts the strength of conclusions about generalization.
2. **Class imbalance:** The FTD group contains fewer participants than the AD and CN groups, potentially affecting model training and evaluation.
3. **Inter-subject variability:** EEG characteristics may vary substantially between individuals, making generalization to unseen participants challenging.
4. **Class-wise performance:** Some baseline results indicate poor detection of particular diagnostic groups despite nonzero overall accuracy.
5. **External validation:** Performance on an independent dataset has not been established by the results documented here.
6. **Reproducibility details:** Exact random seeds, fold assignments, held-out subject identifiers, software versions, and complete model configurations must be documented to enable independent replication.
7. **Result verification:** Some supplied accuracy and aggregate metrics require reconciliation with the original evaluation outputs.

The results should be interpreted as experimental findings on the specified dataset, not as evidence of clinical diagnostic readiness.

## Reproducibility

The repository provides research documentation rather than a directly executable implementation.

For future reproducibility, the following information should be documented where available:

- Dataset version and access instructions.
- Subject-level training, validation, and test assignments.
- Segmentation and input-shape specifications.
- Model architecture descriptions and hyperparameters.
- Training and validation procedures.
- Random seeds and software environment.
- Evaluation scripts or detailed metric definitions.
- Segment-level and subject-level prediction aggregation methods.
- Confusion matrices and per-class performance reports.

Source code and model implementation files are not publicly available in this repository.

## Repository Structure

The repository is intended to contain documentation and selected research outputs, not the private model implementation.

```text
eeg-dementia-classification/
├── README.md
├── docs/
│   ├── methodology.md
│   └── dataset.md
├── figures/
│   ├── architecture.png
│   └── evaluation_protocol.png
└── results/
    ├── model_comparison.csv
    └── confusion_matrices/
```

These files and directories are a proposed structure; they should be added only when the corresponding materials are ready for publication.

## Code Availability

The source code is **not publicly available** at this time.

This repository is intended to document the research objectives, dataset, methodology, experimental findings, and limitations while keeping the implementation private.

The absence of publicly available source code limits independent computational reproduction of the experiments.


**Dataset citation:** Please consult the OpenNeuro dataset page for the dataset's recommended citation and associated publication details.

- [OpenNeuro ds004504](https://openneuro.org/datasets/ds004504/versions/1.0.2/file-display/README)


## Contact

**A. K. Saikat Alam**  

saikatalam002@gmail.com

---

*This repository documents an academic research project. The reported metrics require verification against the original evaluation outputs, and the findings should not be interpreted as clinical validation.*

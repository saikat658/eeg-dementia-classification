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

The research emphasizes subject-independent evaluation, comparative assessment of deep learning architectures, and performance analysis at subject levels. A multi-band EEGNet architecture is investigated alongside established deep learning baselines.

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
5. Examine the limitations associated with inter-subject variability and the relatively small number of participants, particularly in the FTD group.

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

Continuous EEG recordings are divided into non-overlapping four-second segments. Then the segments are aggregated into subjects and stored as a numpy array. So each subject now contains segments of the EEG recording. Segments with bad annotations or with boundary events were excluded to prevent the apparence of unwanted features.

At a sampling frequency of 500 Hz, each segment contains:

- 2,000 time samples per channel
- 19 EEG channels
- 38,000 channel-sample values per segment

The resulting segments are used as model inputs, with diagnostic labels inherited from their corresponding participants.

### 3. Subject-independent evaluation

To reduce the risk of data leakage, participants are separated into training and evaluation partitions. As a result 68 participants were grouped as training data and remaining 19 subjects in test data.

The evaluation framework incorporates grouped cross-validation and a held-out subject-level test set. The same participant must not occur in both training and evaluation partitions.

Subject-level separation is an established evaluation practice rather than a standalone methodological novelty of this study. Its purpose here is to support a more credible assessment of generalization to unseen participants.



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


## Code Availability

The source code is **not publicly available** at this time.

This repository is intended to document the research objectives, dataset, methodology, experimental findings, and limitations while keeping the implementation private.

The absence of publicly available source code limits independent computational reproduction of the experiments.


**Dataset citation:** Please consult the OpenNeuro dataset page for the dataset's recommended citation and associated publication details.

- [OpenNeuro ds004504](https://openneuro.org/datasets/ds004504/versions/1.0.2/file-display/README)


## Contact

**A. K. Saikat Alam**  

saikatalam002@gmail.com


# Method and evaluation summary

This document summarizes the submitted HiCaps-EEG manuscript. It describes the reported approach; it does not specify an executable reproduction.

## Research question

Can a model improve EEG visual-category decoding by learning complementary neural representations and adapting their contributions to each observation?

## Representation pathways

### Structured EEG encoding

The manuscript describes temporal and spatial convolutions followed by capsule-based feature construction. Capsule features are converted to token embeddings and processed by a transformer-based contextual encoder.

### Hierarchical EEG representations

EEG representations are organized around contour-, foreground-object-, and context-related information, following the hierarchical semantic motivation of [ViEEG](https://arxiv.org/abs/2505.12408). Cross-attention progressively integrates these representations before hierarchical classification.

### Capsule classification

A class-capsule module produces a complementary category prediction. The manuscript describes confidence cues involving prediction confidence, routing certainty, classification margin, and capsule agreement.

## Training-time visual-semantic guidance

Each stimulus supplies three visual views: an object mask, a foreground object, and the contextual image. A frozen CLIP encoder supplies semantic targets for alignment with EEG features.

Visual supervision is used during training. Inference requires EEG, rather than the presented stimulus image.

## Reliability-aware prediction fusion

Confidence-related information from the prediction pathways determines an observation-dependent fusion weight. The final classification combines the hierarchical and capsule predictions.

This conceptual description intentionally leaves the exact gate parameterization to the implementation and experimental records.

## Dataset

The original [Kaneshiro et al. dataset](https://doi.org/10.1371/journal.pone.0135697) contains ten participants and 72 stimuli across six balanced categories. The original experiment collected 5,184 trials per participant.

The manuscript reports using the released preprocessing: 124 retained EEG channels, 1–25 Hz filtering, 62.5 Hz sampling, ICA-based ocular artifact correction, average reference, and approximately 0–496 ms post-stimulus epochs.

The original dataset is available from the [Stanford Digital Repository](https://purl.stanford.edu/bq914sc3730). Dataset preprocessing and dataset ownership belong to the original source.

## Generalization protocols

| Protocol | Unit held out | Reported outer folds |
| --- | --- | ---: |
| Stimulus-wise | Stimulus-image identity | 6 |
| Subject-wise | Participant identity | 5 |

Both protocols use the same six category labels. The manuscript states that the corresponding generalization units are separated and that outer-test data are reserved for final evaluation.

Exact fold assignments, group-averaging details, fitting boundaries for learned preprocessing, and model-selection records are not distributed in this documentation release.

## Implementation context reported in the manuscript

The implementation uses PyTorch. Training was performed with two NVIDIA GPUs with approximately 16 GB VRAM each. This is a description of the reported experiment, not a tested minimum hardware requirement.

## Scope of the evidence

The study investigates EEG-based category decoding from responses to still images. Video modelling and animal-behaviour retrieval were not evaluated in this study.

# Proposed Methodology

## Project Title
**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

## 1. Overview

The project proposes an enhanced anomaly-detection system based on AA-CLIP. The base AA-CLIP model produces image-level anomaly scores and pixel-level localization maps using anomaly-aware text anchors and adapted visual features.

The proposed extension is **AA-CLIP-MBPS (Memory-Bank Patch Scoring)**. It adds a branch that compares test-image patch features with a memory bank containing normal training patches. The memory-bank score map is fused with the original AA-CLIP anomaly map.

The project also plans a React frontend, FastAPI backend and an LLM-based inspection-report module supported by retrieval-augmented generation (RAG). The LLM explains model evidence and retrieved manufacturing knowledge; it does not detect defects.

**Status:** This is the proposed design for M1. It has not yet been implemented or experimentally evaluated.

## 2. Planned Dataset Setup

| Dataset | Planned role |
|---|---|
| VisA | Training and in-domain evaluation |
| MVTec AD | Cross-dataset evaluation |
| MPDD | Cross-dataset evaluation |
| BTAD | Included in scope; exact role to be finalized in M2 |

The VisA training and test images must be disjoint. Dataset splits and any permitted normal reference images must be documented.

## 3. Stage 1 — Data Preparation

1. Obtain the selected datasets and verify their directory structure.
2. Identify normal images, anomalous images and available ground-truth masks.
3. Apply the preprocessing required by the AA-CLIP implementation.
4. Preserve the original train/test separation.
5. Record dataset and category counts used in the experiments.
6. Create fixed lists of evaluation images so every method is tested on the same samples.

Ground-truth masks are used for evaluation, not to build the normal memory bank.

## 4. Stage 2 — Reproduce the AA-CLIP Baseline

Use the AA-CLIP implementation and configuration described in the base paper and the M1 presentation.

The presentation describes the following components:

1. A pretrained CLIP model with ViT-L/14.
2. A text-adaptation stage with residual adapters.
3. Normal and abnormal text anchors separated using a disentanglement loss.
4. A visual-adaptation stage with residual adapters.
5. Patch features from visual layers 6, 12, 18 and 24.
6. Image-level scoring using cosine similarity to the anchors.
7. Pixel-level anomaly-map generation and upsampling.

Reproduce the baseline before introducing MBPS. Record the code version, configuration, training split, random seeds, checkpoint and evaluation procedure. Any differences from the paper must be documented.

## 5. Stage 3 — Build the Normal Patch Memory Bank

After obtaining AA-CLIP patch features, construct a memory bank, `M`, from normal training/reference images only.

Proposed procedure:

1. Pass permitted normal images through the adapted AA-CLIP visual encoder.
2. Extract the selected patch features.
3. Store the normal patch features in the memory bank.
4. Optionally reduce the bank using random sampling or a coreset method.
5. Record the bank size and feature-normalization procedure.

**Data-safety rule:** Do not use test images, test masks or test labels to construct, select or tune the memory bank.

### Two evaluation settings

- **Few-normal-shot:** Build the bank using permitted normal reference images from the target category's training split.
- **Zero-shot variant:** Build the bank using VisA normal training patches only, then evaluate on the target dataset without target-category reference images.

These settings must be evaluated and reported separately. The few-normal-shot setting must not be described as strict zero-shot.

## 6. Stage 4 — Memory-Bank Patch Scoring

For each test image:

1. Extract its patch features using the adapted AA-CLIP visual encoder.
2. Compare each test patch with the normal patch features in the memory bank.
3. Calculate a nearest-neighbour distance for each test patch.
4. Convert the distances into a memory-bank anomaly score map, `S_MB`.

A larger distance is intended to indicate that a test patch is less similar to the stored normal patches. The exact distance metric, feature normalization and nearest-neighbour settings must be fixed and documented during implementation.

## 7. Stage 5 — Fuse AA-CLIP and Memory-Bank Scores

The original AA-CLIP branch produces an anomaly score map, `S_AA`. The memory-bank branch produces `S_MB`.

The proposed normalized fusion is:

`S = (1 − w) · S_AA + w · S_MB`

where:

- `S` is the fused anomaly map.
- `S_AA` is the normalized AA-CLIP anomaly map.
- `S_MB` is the normalized memory-bank anomaly map.
- `w` is the fusion weight selected using validation data, not test labels.

The fused map is used for pixel-level localization. An image-level anomaly score can be derived using a consistently defined aggregation rule. A binary mask can be produced using a threshold selected on permitted validation data.

The normalization method, aggregation rule, threshold and fusion weight must be documented and held fixed for final test evaluation.

## 8. Stage 6 — Small-Anomaly Evaluation

Use each anomalous image's ground-truth mask to calculate:

`Area fraction = defect pixels / total image pixels`

Divide anomalous images into small, medium and large defect groups using thresholds `τ1` and `τ2`. The thresholds will be fixed in M2 before model evaluation.

Use identical image lists for all compared methods. Report image AUROC, pixel AUROC and AU-PRO for the overall test set and each defect-size group. Define how normal images are handled in each group before running the evaluation.

## 9. Stage 7 — Perturbation-Based Robustness Evaluation

Create fixed perturbed test sets using seeded transformations. The presentation proposes:

| Perturbation | Planned levels |
|---|---|
| Brightness | −20%, −10%, +10%, +20% |
| Gaussian blur | Kernel and sigma levels to be fixed in M2 |
| Gaussian noise | Noise sigma levels to be fixed in M2 |
| Rotation | ±5° and ±10° |

For geometric transformations, apply the same rotation to the image and its mask. Use nearest-neighbour interpolation for masks. Define border handling consistently.

Save each perturbed test set and use the same files for every compared method. Report clean performance, perturbed performance, absolute performance drop and percentage retention.

## 10. Stage 8 — Cross-Dataset Evaluation

The planned direction is:

- Train on VisA.
- Evaluate on VisA as the in-domain dataset.
- Evaluate on MVTec AD and MPDD as cross-dataset datasets.
- Finalize BTAD's role in M2.

Report in-domain and cross-dataset results separately. Document any dataset-specific tuning so that the meaning of zero-shot generalization remains clear.

## 11. Stage 9 — Ablation Studies

| Ablation | Configuration | Purpose |
|---|---|---|
| A0 | Original AA-CLIP scoring | Reference baseline |
| A1 | Memory-bank distance only using AA-CLIP patch features | Measure the contribution of nearest-neighbour evidence |
| A2 | Fusion of AA-CLIP and memory-bank scores | Measure whether the two branches complement each other |
| A3 | Different memory-bank sizes / coreset levels | Study accuracy and memory cost |
| A4 | Different nearest-neighbour `k` values | Study sensitivity to neighbour settings |
| A5 | Different fusion weights `w` | Study the balance between the two branches |

Choose hyperparameters using validation data only. Do not select them using test labels.

## 12. Stage 10 — Statistical Analysis

Use paired bootstrap resampling over images, with at least 1,000 iterations and 95% confidence intervals for paired differences, as proposed in the presentation.

Resample images rather than treating individual pixels as independent observations. Report the number of images and the evaluation units used for every metric.

## 13. Stage 11 — LLM-Based Inspection Report

The planned reporting pipeline is:

1. The user uploads an industrial image through the frontend.
2. FastAPI sends the image to AA-CLIP-MBPS.
3. The model returns normal/abnormal status, score, localization map and mask/region evidence.
4. The user confirms the product category.
5. RAG retrieves relevant information from a curated manufacturing knowledge base.
6. A pre-trained LLM generates a structured inspection report.

The report may include:

- Summary and status.
- Anomaly score and localization evidence.
- Observed visual defect traits.
- Possible manufacturing stages to investigate.
- Potential contributing factors.
- Evidence and reasoning.
- Verification checks.
- Uncertainty and limitations.
- Knowledge-base sources.

The knowledge base is planned to contain fields such as defect, product, manufacturing stage and possible cause. The report must distinguish model-observed visual evidence from retrieved knowledge and LLM interpretation. It must use tentative language such as “possible stage to investigate” and must not claim that an image alone proves the exact manufacturing stage or root cause.

## 14. Planned Application Architecture

- **Frontend:** React image-upload and report-display interface.
- **Backend:** FastAPI.
- **Detection and localization:** Enhanced AA-CLIP / AA-CLIP-MBPS.
- **Knowledge retrieval:** RAG over a curated manufacturing knowledge base.
- **Report generation:** Pre-trained LLM.
- **Database:** MySQL may be used for user accounts, inspection history and report records, if required.

## 15. M2 Decisions Still Required

- Small/medium/large defect-area thresholds.
- Gaussian blur kernel and sigma values.
- Gaussian noise levels.
- AU-PRO implementation and FPR limit (the presentation proposes 0.3).
- Exact role of BTAD.
- Memory-bank feature normalization, size and nearest-neighbour settings.
- Fusion-weight selection and image-score aggregation.
- Final validation and test split details.

No experimental results are claimed in this methodology document.

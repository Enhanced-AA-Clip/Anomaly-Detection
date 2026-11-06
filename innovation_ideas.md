# Innovation Ideas

## Project Title
**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

## 1. Purpose

This document describes the proposed contributions presented for M1. These ideas are research directions to implement and evaluate; they are not completed features or experimentally verified improvements.

## 2. Main Proposed Contribution — AA-CLIP-MBPS

**AA-CLIP-MBPS** extends AA-CLIP with a Memory-Bank Patch Scoring branch.

### Existing AA-CLIP branch

AA-CLIP uses anomaly-aware normal and abnormal text anchors. It compares adapted visual features with these anchors and produces image-level anomaly scores and pixel-level anomaly maps.

### Proposed memory-bank branch

The proposed branch stores normal patch features extracted from permitted normal training/reference images. For each test patch, it calculates the nearest-neighbour distance to the normal patch memory bank. A larger distance is treated as evidence that the patch differs from stored normal appearances.

### Score fusion

The memory-bank score map is normalized and combined with the original AA-CLIP map:

`S = (1 − w) · S_AA + w · S_MB`

The fusion weight `w` is selected using validation data. The project will investigate whether semantic evidence from text anchors and instance-level evidence from normal reference patches complement each other.

**What is new in this project direction**

The contribution is not the general idea of a memory bank by itself: PatchCore and FE-CLIP are already discussed in the presentation as related work. The proposed contribution is applying memory-bank patch scoring to the AA-CLIP feature/scoring pipeline and evaluating it systematically for defect size, perturbation robustness and cross-dataset performance.

## 3. Innovation Idea 1 — Small-Anomaly Benchmark Split

### Problem

Overall detection and localization scores can hide poor performance on small defects. Small anomalous regions may occupy only a small fraction of an image and may be difficult to represent with patch-level features.

### Proposed idea

Use ground-truth masks to calculate defect-area fraction:

`Area fraction = defect pixels / total image pixels`

Create small, medium and large defect groups using thresholds `τ1` and `τ2`, to be fixed in M2 before evaluation.

### Evaluation

- Use the same image lists for every method.
- Report image AUROC, pixel AUROC and AU-PRO for each group.
- Define the handling of normal images for each group before running the experiments.

### Expected research value

This evaluation can show whether the proposed method helps small defects rather than only improving an overall average.

## 4. Innovation Idea 2 — Perturbation-Based Robustness Protocol

### Problem

Industrial inspection images can vary because of lighting changes, blur, sensor noise and object misalignment. Performance on clean images does not establish robustness to these changes.

### Proposed idea

Create fixed test sets with controlled perturbations:

- Brightness: −20%, −10%, +10%, +20%.
- Gaussian blur: kernel and sigma values to be finalized in M2.
- Gaussian noise: sigma values to be finalized in M2.
- Rotation: ±5° and ±10°.

Use fixed seeds and save the perturbed images so every method receives exactly the same inputs. For rotation, apply the same transformation to the mask using nearest-neighbour interpolation.

### Evaluation

Compare clean and perturbed image AUROC, pixel AUROC and AU-PRO. Report absolute metric drops and percentage retention.

### Expected research value

This protocol will measure how stable AA-CLIP and AA-CLIP-MBPS are under controlled image changes. It will also test the hypothesis that the memory-bank branch may be sensitive to geometric or appearance shifts.

## 5. Innovation Idea 3 — Memory-Bank Patch Scoring and Fusion

### Problem

AA-CLIP's text-anchor similarity provides semantic normal/abnormal evidence. The proposed project investigates whether comparing patches against actual normal reference patches can provide complementary evidence.

### Proposed idea

1. Extract normal patch features using the adapted AA-CLIP visual encoder.
2. Store the features in a memory bank.
3. Calculate nearest-neighbour distances for test patches.
4. Normalize the memory-bank anomaly map.
5. Fuse it with the original AA-CLIP map.
6. Produce a localization map and, using a validation-selected threshold, a binary mask.

### Safeguards

- Use only permitted normal training/reference patches.
- Never use test images or test masks in the memory bank.
- Choose the fusion weight and threshold using validation data.
- Record bank size, feature normalization and nearest-neighbour settings.

### Two settings

- Few-normal-shot: target-category normal reference patches are permitted.
- Zero-shot variant: memory bank is built from VisA normal training patches only.

Report the two settings separately.

## 6. Supporting System Idea — Evidence-Grounded LLM Inspection Report

The project also plans an LLM-based report-generation module.

### Workflow

1. AA-CLIP-MBPS detects and localizes an anomaly.
2. The system provides the predicted status, anomaly score, localization map and region evidence.
3. The user confirms the product category.
4. RAG retrieves relevant entries from a curated manufacturing knowledge base.
5. A pre-trained LLM produces a structured inspection report.

### Report contents

- Inspection summary and status.
- Model score and localization evidence.
- Observed visual characteristics.
- Possible manufacturing stages to investigate.
- Potential contributing factors.
- Suggested verification checks.
- Evidence, knowledge sources and uncertainty.

### Important limitation

The LLM does not detect the anomaly. An image alone cannot prove the exact manufacturing stage or root cause. The report must separate detected visual evidence from retrieved manufacturing information and LLM-generated interpretation. It should use cautious language such as “possible stage to investigate” and cite the knowledge-base sources used.

## 7. How the Ideas Support the Research Questions

| Proposed idea | Research question |
|---|---|
| Memory-bank patch scoring and fusion | RQ1 and RQ2; also evaluated in RQ3 |
| Small-anomaly benchmark split | RQ2 |
| Perturbation-based robustness protocol | RQ3 |
| Evidence-grounded LLM report | Planned system functionality; it does not replace the detector or directly answer RQ1–RQ3 |

## 8. Novelty and Claim Boundaries

The M1 presentation itself notes that FE-CLIP already includes a memory-bank map in a few-shot setting. Therefore, the project should not claim that it invented memory-bank anomaly scoring.

A defensible project contribution is to investigate the integration of normal-reference patch scoring with AA-CLIP and to evaluate it through a consistent set of defect-size, perturbation and cross-dataset experiments. The novelty and strength of this contribution must be established through implementation, fair comparisons and experimental results.

## 9. Planned Validation

The proposed ideas will be assessed through:

- Reproduction of AA-CLIP.
- Ablations of the AA-CLIP branch, memory-bank branch and fused system.
- Different bank sizes, nearest-neighbour settings and fusion weights.
- Small-, medium- and large-defect evaluation.
- Clean and perturbed evaluation.
- In-domain and cross-dataset evaluation.
- Paired bootstrap confidence intervals.

All improvements remain hypotheses until these experiments are completed.

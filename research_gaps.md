# Research Gaps

## Project Title
**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

## 1. Purpose

This document records the research gaps identified from the three papers discussed in the M1 presentation: AA-CLIP, AnomalyCLIP and FE-CLIP. It distinguishes gaps explicitly discussed by the papers from gaps identified by the project team's comparison.

## 2. Identified Research Gaps

### G1 — AU-PRO Is Not Reported by AA-CLIP

**Evidence in the M1 analysis**

The presentation states that AA-CLIP reports image AUROC and pixel AUROC, but not AU-PRO. AnomalyCLIP and FE-CLIP report PRO-related localization metrics.

**Why it matters**

Pixel AUROC measures pixel-level separation, but it may not fully describe how well anomalous regions are localized. A region-based metric can provide additional information about localization quality.

**Research response**

Reproduce AA-CLIP and calculate AU-PRO using one fixed implementation and a predeclared false-positive-rate limit. The presentation proposes an FPR limit of 0.3, to be finalized in M2.

### G2 — Lack of Defect-Size-Stratified Evaluation

**Evidence in the M1 analysis**

The project team's review did not find a defect-size-stratified evaluation in the three uploaded papers.

**Why it matters**

Small defects may occupy only a small part of an image and can be difficult to identify accurately. A single overall metric can hide poor performance on small defects.

**Research response**

Create small, medium and large defect groups using the area fraction calculated from ground-truth masks. Fix the thresholds before evaluation and use the same image lists for all methods.

**Evidence classification**

This is an absence-of-evidence gap from the team's review. It is not presented as a limitation explicitly stated by all paper authors.

### G3 — Lack of Image-Perturbation Robustness Evaluation

**Evidence in the M1 analysis**

The presentation states that none of the three uploaded papers evaluates the proposed image perturbations.

**Why it matters**

Industrial images may vary because of lighting changes, blur, sensor noise or object misalignment. A model's clean-image performance does not by itself show how stable it is under these changes.

**Research response**

Evaluate clean and reproducibly perturbed images using brightness changes, Gaussian blur, Gaussian noise and rotation. Report performance drops using identical perturbed samples for all methods.

**Evidence classification**

This is an absence-of-evidence gap identified by the project team, not an author-reported limitation.

### G4 — Opportunity to Add Normal-Reference Patch Evidence to AA-CLIP

**Evidence in the M1 analysis**

AA-CLIP uses anomaly-aware text-anchor similarity and does not use the normal-reference patch-scoring branch proposed by this project. The presentation discusses FE-CLIP's memory-bank result on MVTec AD, where image AUROC is reported as increasing from 91.9 to 96.6 in a four-shot setting.

**Why it matters**

Text-anchor similarity provides semantic evidence, while nearest-neighbour comparison with normal patches can provide instance-level evidence about how different a test patch is from normal appearances.

**Research response**

Add a memory-bank patch-scoring branch to AA-CLIP and fuse its normalized score map with the AA-CLIP anomaly map.

**Important qualification**

FE-CLIP's reported improvement belongs to FE-CLIP and its stated evaluation setting. It is motivation for testing the idea, not proof that AA-CLIP-MBPS will obtain the same improvement.

### G5 — Results Across Papers Use Different Evaluation Protocols

**Evidence in the M1 analysis**

The presentation notes that compared results come from different training and evaluation protocols. It also identifies differences in dataset tuning and few-shot settings.

**Why it matters**

A direct comparison of numbers from different papers may be unfair if the datasets, training data, tuning procedures or evaluation settings differ.

**Research response**

Reproduce selected baselines under a common project protocol where feasible. Clearly label results copied from papers separately from results reproduced by the project team.

## 3. Gap-to-Question Mapping

| Gap | Related research question | Planned response |
|---|---|---|
| G1: AU-PRO missing in AA-CLIP | RQ2 | Reproduce AA-CLIP AU-PRO |
| G2: No size-stratified evaluation found | RQ2 | Small/medium/large defect split |
| G3: No perturbation evaluation found | RQ3 | Controlled perturbation tests |
| G4: No AA-CLIP normal-reference patch branch | RQ1 and RQ2 | Memory-bank patch scoring and fusion |
| G5: Different protocols across papers | RQ1, RQ2 and RQ3 | Common reproduction and evaluation protocol |

## 4. Overall Research Opportunity

The project investigates whether AA-CLIP can benefit from combining its anomaly-aware text-anchor evidence with normal-reference patch evidence. It also aims to evaluate localization by defect size and test robustness under controlled image perturbations. These experiments are needed to establish whether the proposed method provides measurable improvements; no improvement is claimed at the M1 stage.

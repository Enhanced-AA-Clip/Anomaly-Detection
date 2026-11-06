# Research Questions

## Project Title
**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

## 1. Background

AA-CLIP uses anomaly-aware text anchors and adapted visual features to detect industrial anomalies and produce localization maps. The project proposes adding memory-bank patch scoring (MBPS) to provide additional evidence from normal patch features. The research questions are derived from the literature gaps identified in the M1 presentation.

## 2. Main Research Questions

### RQ1 — Image-Level Anomaly Detection

**Does fusing memory-bank patch evidence with AA-CLIP scores improve image-level AUROC over reproduced AA-CLIP on VisA, MVTec AD and MPDD?**

**Reason for asking**

AA-CLIP uses text-anchor similarity for anomaly scoring. The M1 literature analysis identifies the opportunity to add instance-level evidence by comparing test patches with normal reference patches. The presentation also discusses memory-bank results reported by FE-CLIP in a few-normal-shot setting. Those results motivate investigation but do not establish that the proposed AA-CLIP-MBPS method will improve performance.

**Planned evaluation**

- Compare reproduced AA-CLIP with AA-CLIP-MBPS.
- Evaluate on VisA, MVTec AD and MPDD.
- Report image AUROC per category and macro-average.
- Report the zero-shot variant and few-normal-shot setting separately.

**Proposed target**

At least a 1 percentage-point improvement in mean image AUROC is a provisional target, with the presentation noting that this is more plausible in a few-normal-shot setting than in a strict zero-shot setting.

### RQ2 — Pixel-Level Localization and Small Defects

**Does memory-bank patch scoring improve pixel-level localization (pixel AUROC and AU-PRO), particularly for small defects?**

**Reason for asking**

An image can be correctly classified as anomalous even when the predicted defective region is inaccurate. The M1 review notes that AA-CLIP reports image AUROC and pixel AUROC but does not report AU-PRO. It also identifies that the reviewed papers do not provide a common defect-size-stratified analysis.

**Planned evaluation**

- Generate defect-area fractions from ground-truth masks.
- Divide anomalous images into small, medium and large defect groups using thresholds fixed before evaluation.
- Use identical image lists for all compared methods.
- Report pixel AUROC and AU-PRO for each size group.
- Report category-level and macro-average results.

**Proposed target**

The presentation recommends a provisional target of at least a 1 percentage-point mean pixel AUROC improvement, with a possible 2 percentage-point target on MVTec AD. AU-PRO and small-defect improvements remain hypotheses until the AA-CLIP AU-PRO baseline is reproduced.

### RQ3 — Robustness and Cross-Dataset Generalization

**How do AA-CLIP and AA-CLIP-MBPS degrade under industrial image perturbations and cross-dataset shift, and does MBPS reduce this degradation?**

**Reason for asking**

The reviewed papers do not evaluate the image perturbations proposed in the presentation. The presentation also observes variation in reported performance across datasets. A memory-bank branch may provide useful normal-reference evidence, but it may also be sensitive to changes in image appearance or geometry.

**Planned evaluation**

- Evaluate clean test images and fixed, reproducible perturbed versions.
- Consider brightness changes, Gaussian blur, Gaussian noise and small rotations.
- Apply geometric transformations consistently to images and masks.
- Compare in-domain VisA performance with cross-dataset evaluation on MVTec AD and MPDD.
- Report absolute performance drops and percentage retention.
- Use paired statistical comparisons.

**Proposed target**

The coordinator's target is at least 90% retention and at least a 2 percentage-point reduction in degradation. The M1 presentation recommends reporting absolute AUROC drops as well, because retention alone can be difficult to interpret. Final thresholds should be fixed after clean baseline reproduction.

## 3. Mapping Research Questions to Project Components

| Research question | Proposed component | Main evaluation |
|---|---|---|
| RQ1 | AA-CLIP-MBPS score fusion | Image AUROC |
| RQ2 | Memory-bank localization map and small-anomaly split | Pixel AUROC and AU-PRO |
| RQ3 | Perturbation protocol and cross-dataset evaluation | Clean-to-perturbed drop and cross-dataset gap |

## 4. Scope and Conditions

- VisA is planned as the training dataset and as an in-domain evaluation dataset, with train/test separation maintained.
- MVTec AD and MPDD are planned for cross-dataset evaluation.
- BTAD remains in project scope, but its exact role will be finalized in M2.
- The memory bank must use only permitted normal training/reference patches. Test images and test labels must never be used to construct or tune it.
- Few-normal-shot and strict zero-shot-variant results must be reported separately.
- All targets are proposed objectives, not achieved results.

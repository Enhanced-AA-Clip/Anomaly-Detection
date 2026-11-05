# Literature Review

## Project Title
**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

## 1. Introduction

Industrial visual inspection aims to identify defective products and locate the exact regions containing defects. Conventional anomaly-detection methods often require training data from the target product or category. Such data may not be available when a new product is introduced or when defects are rare.

Zero-shot anomaly detection (ZSAD) aims to detect anomalies in a target dataset without using target-domain training samples. Vision-language models such as CLIP provide image-text representations that support zero-shot recognition. However, general CLIP is not specifically trained to distinguish normal and abnormal industrial appearances. As discussed in AA-CLIP and AnomalyCLIP, defective images can sometimes be associated more strongly with normal text descriptions, and global image-text representations may not precisely identify small defective regions.

This project selects **AA-CLIP** as its base method and proposes an additional memory-bank patch-scoring branch, called **AA-CLIP-MBPS**, together with evaluation protocols for small defects and image perturbations. An LLM-based module is also planned to produce an evidence-based inspection report; it will not perform anomaly detection.

## 2. Review of the Main Research Papers

### 2.1 AA-CLIP — Base Paper

**Reference as identified in the presentation:** Ma et al., 2025, *AA-CLIP: Enhancing Zero-Shot Anomaly Detection via Anomaly-Aware CLIP*, arXiv:2503.06661.

**Problem addressed**

General CLIP is anomaly-unaware. Normal and abnormal text representations may overlap, and patch-level alignment may be imprecise for industrial anomaly detection.

**Proposed method**

AA-CLIP uses a two-stage adaptation process while retaining the pretrained CLIP model:

1. It adds residual adapters to the text encoder.
2. It learns normal and abnormal text anchors and uses a disentanglement loss to separate them.
3. It then adds residual adapters to the visual encoder.
4. It aligns patch-level visual features with the normal and abnormal text anchors.
5. It produces an image-level anomaly score and a pixel-level anomaly map.

The presentation reports the residual adapter expression as:

`x_enh = λ · x_res + (1 − λ) · x`, with `λ = 0.1`.

It reports the total loss as:

`L_total = L_align + γ · L_dis`, with `γ = 0.1`.

The reported configuration uses ViT-L/14 at 518-pixel input resolution, with four visual feature levels (layers 6, 12, 18 and 24). The presentation lists 5 epochs for the first stage and 20 epochs for the second stage.

**Evaluation described in the presentation**

The presentation reports training settings on VisA (2-shot, 16-shot, 64-shot and full-shot) and testing across 11 datasets. The metrics listed for AA-CLIP are image AUROC and pixel AUROC.

**Limitations or open points identified from the paper**

- The paper reports signs of overfitting in the full-shot setting.
- Context-awareness is mentioned as future work.
- The presentation notes that AA-CLIP does not report AU-PRO.
- The presentation identifies the absence of a defect-size-stratified evaluation and image-perturbation robustness evaluation in the papers reviewed.

The last two points are findings from the team's review of the uploaded papers, rather than claims that the AA-CLIP authors explicitly listed them as limitations.

### 2.2 AnomalyCLIP

**Reference as identified in the presentation:** Zhou et al., 2024, ICLR / arXiv:2310.18961, *AnomalyCLIP*.

**Problem addressed**

Vision-language models can focus on object semantics rather than the visual characteristics that distinguish normal and abnormal regions.

**Proposed method**

AnomalyCLIP learns object-agnostic prompts to represent normality and abnormality. The presentation describes global and local learning objectives and DPAM (V-V attention) as components of the method.

**Evaluation described in the presentation**

The presentation reports evaluation across 17 datasets and lists AUROC, AP and AU-PRO/PRO-related metrics. It also notes that the paper's protocol includes dataset-specific tuning details, which must be considered when comparing its results with other methods.

**Limitations or observations recorded in the presentation**

- The presentation notes weaker results on medical datasets, partly associated with auxiliary-data limitations.
- It notes a performance gap compared with the full-shot PatchCore results discussed in the paper's appendix.
- The presentation uses the difference between AUROC and PRO on BTAD to motivate the importance of evaluating localization, not only image-level detection.

### 2.3 FE-CLIP

**Reference as identified in the presentation:** Gong et al., *FE-CLIP*, ICCV. The publication year should be verified from the final paper before submission because it is not printed in the uploaded PDF, as noted in the presentation.

**Problem addressed**

The method investigates the use of image-frequency information that may not be fully captured by conventional CLIP-based zero-shot anomaly-detection approaches.

**Proposed method**

The presentation describes DCT-based frequency feature enhancement (FFE) and local frequency statistics (LFS) adapters in the visual encoder. It also describes a few-normal-shot setting that uses a memory bank.

**Evaluation described in the presentation**

The presentation lists 10 datasets and metrics including AUROC, AP and PRO. It reports that the memory-bank setting improves MVTec AD image AUROC from 91.9 to 96.6 in a four-shot setting. This is a result reported for FE-CLIP and must not be presented as an expected or achieved result of the proposed AA-CLIP-MBPS system.

**Limitations or observations recorded in the presentation**

The presentation notes that frequency information may not help when a defect does not substantially change the frequency characteristics of an image. It gives the dumpling example from the paper.

## 3. Related Methods Mentioned for Context

The presentation also discusses CLIP, WinCLIP and PatchCore. These were not among the three uploaded full papers reviewed for this M1 analysis. Their descriptions below are therefore limited to the characterization and comparisons included in the presentation.

| Method | Main idea stated in the presentation | Relevance to this project |
|---|---|---|
| CLIP (2021) | Image-text contrastive pretraining | Provides the vision-language foundation |
| WinCLIP (2023) | Hand-crafted prompts and window features | Early CLIP-based zero-shot anomaly-detection approach |
| PatchCore (2022) | Memory of normal patch features and nearest-neighbour scoring | Provides context for normal-reference patch scoring |
| AnomalyCLIP (2024) | Object-agnostic prompts and global/local learning | Addresses anomaly-unaware visual-language representations |
| AA-CLIP (2025) | Anomaly-aware text anchors and residual adapters | Selected base method |
| FE-CLIP | DCT-based frequency adapters and a few-shot memory-bank setting | Provides related evidence for memory-bank and frequency-based approaches |

## 4. Comparison and Synthesis

The reviewed methods address different limitations of CLIP-based anomaly detection:

- CLIP supplies general image-text representations but is not specifically anomaly-aware.
- WinCLIP uses prompts and image windows to adapt CLIP to anomaly detection.
- AnomalyCLIP learns object-agnostic normal and abnormal prompts and considers global and local information.
- AA-CLIP introduces anomaly-aware text anchors and residual adapters to improve image-text alignment for anomaly detection.
- FE-CLIP investigates frequency information and includes a memory-bank setting.

AA-CLIP is selected as the base because the project intends to retain its anomaly-aware text anchors and adapted visual features. The proposed extension adds normal-reference patch evidence and evaluates whether combining the two types of evidence helps detection and localization.

## 5. Research Direction Derived from the Literature

The proposed project will:

1. Reproduce AA-CLIP as the primary baseline.
2. Add a memory-bank patch-scoring branch using normal training patches only.
3. Fuse the memory-bank score map with the original AA-CLIP anomaly map.
4. Evaluate performance by defect size and under controlled image perturbations.
5. Compare in-domain and cross-dataset performance.
6. Add an LLM-based report module grounded in retrieved manufacturing knowledge.

The memory-bank method will be reported separately in two settings:

- **Few-normal-shot setting:** the bank may contain permitted normal reference images from the target category.
- **Zero-shot variant:** the bank is built from VisA only and does not use target-category reference images.

These settings must not be combined under one unqualified “zero-shot” result.

## 6. References

The following references are transcribed from the project presentation. Complete titles, author lists, venue details, page numbers and publication years should be checked against the original PDFs and official publication records before final submission.

1. Ma et al. (2025). *AA-CLIP: Enhancing Zero-Shot Anomaly Detection via Anomaly-Aware CLIP*. arXiv:2503.06661.
2. Zhou et al. (2024). *AnomalyCLIP*. ICLR; arXiv:2310.18961.
3. Gong et al. *FE-CLIP*. ICCV. Verify the publication year and full bibliographic details.
4. Radford et al. (2021). CLIP. ICML. Mentioned for background in the presentation; not one of the three uploaded full papers.
5. Jeong et al. (2023). WinCLIP. CVPR. Mentioned for background in the presentation; not one of the three uploaded full papers.
6. Roth et al. (2022). PatchCore. CVPR. Mentioned for background in the presentation; not one of the three uploaded full papers.

## 7. Review Scope and Evidence Note

This review is based on the information presented in the M1 PowerPoint and its description of the three uploaded research papers. It does not claim that the proposed AA-CLIP-MBPS system has been implemented or evaluated. All proposed improvements and targets are future research objectives.

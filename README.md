# Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization

## Project Overview

Visual anomaly detection is an important task in automated inspection, where the objective is to identify defective or abnormal regions in product images.

Traditional anomaly detection methods often require class-specific training data. This makes it difficult to identify previously unseen objects and defect types.

This project focuses on **zero-shot visual anomaly detection and localization** using vision-language models, particularly **CLIP** and **AA-CLIP**.

The project builds upon **AA-CLIP: Enhancing Zero-Shot Anomaly Detection via Anomaly-Aware CLIP** and investigates enhancements for:

* Fine-grained anomaly localization
* Detection of small and subtle anomalies
* Robustness under different image conditions
* Prediction reliability
* Generalization across different objects and domains

The main goal is to improve anomaly localization and robustness while retaining the **zero-shot generalization capability** of AA-CLIP.

---

##  Project Title

**Enhanced AA-CLIP for Robust Zero-Shot Visual Anomaly Detection and Localization**

---

## Problem Statement

Industrial visual inspection requires accurate identification of defective regions in product images.

Existing traditional anomaly detection approaches may depend on class-specific training data. Therefore, they may struggle when:

* A new object category is introduced.
* A previously unseen defect appears.
* The anomalous region is very small or subtle.
* Image conditions change.
* The model needs to generalize to different objects or domains.

CLIP provides strong vision-language generalization capabilities, but its original representation is not specifically optimized for distinguishing normal and anomalous visual features.

AA-CLIP addresses this limitation by introducing anomaly-aware representations and patch-level visual feature alignment.

Our project investigates further enhancements to AA-CLIP, particularly for **fine-grained anomaly localization, robustness, and prediction reliability**.

---

## Proposed Approach

The project follows an experimental research approach.

### Step 1: CLIP Baseline

The original CLIP model is considered as a baseline for understanding the performance of a general vision-language model in zero-shot anomaly detection.

### Step 2: AA-CLIP Baseline

The original AA-CLIP approach is implemented and evaluated as the main baseline.

AA-CLIP introduces:

* Anomaly-aware text representations
* Residual adapters
* Patch-level visual feature alignment

These components help CLIP better represent normal and anomalous visual features.

### Step 3: Enhanced AA-CLIP

The project investigates additional strategies to improve:

* Fine-grained anomaly localization
* Detection of small anomalous regions
* Robustness
* Prediction reliability
* Generalization across different datasets and object categories

The enhanced approach is then experimentally compared with the baseline models.

---

## How Zero-Shot Anomaly Detection Works

The main idea is to detect anomalies **without requiring class-specific anomaly training for every new object or defect type**.

The model uses two types of information:

### Image Representation

The input image is converted into visual features using the vision encoder.

### Text Representation

Text descriptions representing concepts such as normal and anomalous conditions are converted into text features.

The visual and textual representations are compared using similarity.

A higher similarity with an anomaly-related representation indicates that the image or image region is more likely to contain an anomaly.

### Simplified Workflow

```text
                 Input Image
                     │
                     ▼
              Vision Encoder
                     │
                     ▼
              Image Features
                     │
                     │
                     ▼
              Similarity Matching
                     ▲
                     │
              Text Encoder
                     ▲
                     │
        Normal / Anomaly Text
                     │
                     ▼
             Anomaly Prediction
                     │
                     ▼
           Anomaly Localization
```

For localization, patch-level visual features are used to identify **where the anomalous region is present** in the image.

---

## Research Objectives

The major objectives of this project are:

1. To establish the original AA-CLIP implementation as a baseline.
2. To investigate improvements for fine-grained anomaly localization.
3. To improve detection of small or subtle anomalous regions.
4. To investigate robustness under different image conditions.
5. To improve prediction reliability.
6. To evaluate generalization across different objects and domains.
7. To compare CLIP, AA-CLIP, and the proposed enhanced approach.
8. To experimentally evaluate the proposed approach on established anomaly detection datasets.

---

## 📊 Datasets

The project uses four established industrial anomaly detection datasets.

### 1. MVTec AD

MVTec AD is an industrial anomaly detection dataset containing different object and texture categories with normal and anomalous samples.

### 2. VisA

VisA is a visual anomaly detection dataset containing multiple object categories and different types of defects.

### 3. BTAD

BTAD is an industrial anomaly detection dataset used to evaluate anomaly detection and localization performance.

### 4. MPDD

MPDD is an industrial anomaly detection dataset containing product images with defective regions.

### Dataset Evaluation

The four datasets are evaluated separately to study the behavior of the models across different object categories and domains.

```text
                Anomaly Detection Evaluation
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       MVTec AD         VisA          BTAD
          │
         MPDD
          │
          ▼
   Model Evaluation
          │
          ▼
   Detection + Localization
```

---

## 📈 Evaluation

The project evaluates both **image-level anomaly detection** and **pixel-level anomaly localization**.

The evaluation includes metrics such as:

### Image-Level Evaluation

Image-level metrics evaluate whether the model correctly identifies an image as normal or anomalous.

**AUROC (Area Under the Receiver Operating Characteristic Curve)** is used to measure classification performance.

### Pixel-Level Evaluation

Pixel-level evaluation focuses on identifying the exact regions containing anomalies.

This is important because the project specifically focuses on improving **anomaly localization**.

The models are compared using the evaluation metrics on:

* MVTec AD
* VisA
* BTAD
* MPDD

---

## 🏗️ Overall System Workflow

```text
                Dataset
                   │
                   ▼
            Input Image
                   │
                   ▼
              CLIP Model
                   │
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
   Image Features        Text Features
        │                     │
        │                     │
        └──────────┬──────────┘
                   ▼
             Similarity
                   │
                   ▼
          Anomaly Detection
                   │
                   ▼
        Patch-Level Analysis
                   │
                   ▼
        Anomaly Localization
                   │
                   ▼
       Evaluation & Comparison
```

---

## 🆚 Models Compared

The project experimentally compares the following approaches:

### CLIP

The original CLIP model is used as a general vision-language baseline.

### AA-CLIP

AA-CLIP is the selected base approach for the project. It introduces anomaly-aware text representations, residual adapters, and patch-level visual feature alignment.

### Enhanced AA-CLIP

The proposed approach builds upon AA-CLIP and investigates additional strategies for:

* Better localization
* Small anomaly detection
* Robustness
* Prediction reliability
* Generalization

---

## 📁 Project Structure

```text
Enhanced_AA_CLIP/
│
├── aaclip/
│   └── Core AA-CLIP implementation
│
├── backend/
│   └── FastAPI backend
│
├── frontend/
│   └── User interface
│
├── configs/
│   └── Configuration files
│
├── checkpoints/
│   └── Model checkpoints
│
├── results/
│   └── Experimental results
│
├── scripts/
│   └── Training and utility scripts
│
├── tests/
│   └── Testing files
│
├── train.py
│   └── Training entry point
│
├── test.py
│   └── Model testing
│
├── inference.py
│   └── Inference
│
├── zero_shot.py
│   └── Zero-shot anomaly detection
│
├── check_gpu.py
│   └── GPU availability check
│
├── sanity_test.py
│   └── Basic project validation
│
├── verify_datasets.py
│   └── Dataset verification
│
├── requirements.txt
│   └── Python dependencies
│
└── README.md
    └── Project documentation
```

---

## ⚙️ Technologies Used

* Python
* PyTorch
* CLIP
* AA-CLIP
* FastAPI
* Vision-Language Models
* Deep Learning
* Computer Vision
* Zero-Shot Learning

---

## 🖥️ Hardware and Training

The project is designed to support GPU-based training and experimentation.

GPU acceleration is used to efficiently perform:

* Model training
* Feature extraction
* Inference
* Anomaly localization
* Dataset evaluation

The experiments are performed separately on the selected datasets to enable dataset-specific evaluation and comparison.

---

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd Enhanced_AA_CLIP
```

### 2. Create a Virtual Environment

```bash
python -m venv pythonEnv
```

Activate the environment:

**Windows:**

```bash
pythonEnv\Scripts\activate
```

**Linux:**

```bash
source pythonEnv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Check GPU

```bash
python check_gpu.py
```

### 5. Verify Datasets

```bash
python verify_datasets.py
```

---

## ▶️ Running the Project

### Training

The main training script is:

```bash
python train.py
```

Configuration files can be used to specify dataset and model settings.

### Testing

```bash
python test.py
```

### Inference

```bash
python inference.py
```

### Zero-Shot Detection

```bash
python zero_shot.py
```

The zero-shot pipeline evaluates the model's ability to identify anomalies without requiring class-specific anomaly training for the target category.

---

## 🌐 Backend

The project uses **FastAPI** for the backend.

The backend is responsible for connecting the trained model with the application interface and providing prediction/inference functionality.

The backend can be started using the appropriate FastAPI application command configured in the project.

Example:

```bash
uvicorn backend.main:app --reload
```

---

## 🖼️ Anomaly Localization

An important focus of this project is not only detecting whether an image is anomalous, but also identifying **where the anomaly occurs**.

The system uses patch-level visual information to generate anomaly-related localization information.

Conceptually:

```text
Original Image
      │
      ▼
Visual Feature Extraction
      │
      ▼
Patch-Level Features
      │
      ▼
Anomaly Score for Each Patch
      │
      ▼
Anomaly Map
      │
      ▼
Localized Defective Region
```

This helps the system provide more detailed information than simple image-level classification.

---

## 🧪 Experimental Comparison

The project follows a baseline-to-enhancement evaluation strategy:

```text
                  CLIP
                   │
                   ▼
             Baseline Result
                   │
                   ▼
                AA-CLIP
                   │
                   ▼
             Base Approach
                   │
                   ▼
          Enhanced AA-CLIP
                   │
                   ▼
        Experimental Evaluation
                   │
                   ▼
     Detection + Localization Results
```

The final evaluation compares the approaches using image-level and pixel-level performance measures.

---

## 📌 Expected Outcome

The expected outcome of the project is an experimentally validated enhancement of AA-CLIP that aims to provide:

* More accurate anomaly localization
* Better detection of small and subtle anomalies
* Improved robustness
* Improved prediction reliability
* Better generalization across different objects and domains
* Retention of zero-shot generalization capability

The project is focused on research and experimental evaluation and is **not intended for deployment in safety-critical industrial environments**.

---

## 📚 Base Paper

**Ma, W., Zhang, X., Yao, Q., Tang, F., Wu, C., Li, Y., Yan, R., Jiang, Z., & Zhou, S. K. (2025).**

**AA-CLIP: Enhancing Zero-Shot Anomaly Detection via Anomaly-Aware CLIP.**

Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR 2025).

The base paper introduces anomaly-aware text representations, residual adapters, and patch-level visual feature alignment for zero-shot anomaly detection.

---

## 📖 References

1. Ma, W., Zhang, X., Yao, Q., Tang, F., Wu, C., Li, Y., Yan, R., Jiang, Z., & Zhou, S. K. (2025). **AA-CLIP: Enhancing Zero-Shot Anomaly Detection via Anomaly-Aware CLIP.** CVPR 2025.

2. Zhou, Q., Pang, G., Tian, Y., He, S., & Chen, J. (2024). **AnomalyCLIP: Object-Agnostic Prompt Learning for Zero-Shot Anomaly Detection.** ICLR 2024.

3. Radford, A., Kim, J. W., Hallacy, C., et al. (2021). **Learning Transferable Visual Models From Natural Language Supervision.** ICML 2021.

4. Roth, K., Pemula, L., Zepeda, J., Schölkopf, B., Brox, T., & Gehler, P. (2022). **Towards Total Recall in Industrial Anomaly Detection.** CVPR 2022.

5. Zou, Y., Jeong, J., Pemula, L., Zhang, D., & Dabeer, O. (2022). **SPot-the-Difference Self-Supervised Pre-training for Anomaly Detection and Segmentation.** ECCV 2022.

The references above are taken from the project registration document.

---

## 👥 Project Team

| S. No. | Team Member                    |
| ------ | ------------------------------ |
| 1      | Merugu Keerthi Bhavani         |
| 2      | Nalliboyina Yogitha            |
| 3      | Kowta Venkata Naga Sai Abhiram |
| 4      | Koku Pradyumnath Reddy         |

**Project Guide:** Mrs. M. Lavanya

---

## 📌 Research Scope

This project focuses on:

* Zero-shot visual anomaly detection
* Anomaly localization
* Vision-language models
* CLIP-based anomaly detection
* AA-CLIP
* Fine-grained anomaly localization
* Robustness evaluation
* Prediction reliability
* Cross-domain generalization
* Experimental comparison

The project does not cover deployment in safety-critical industrial environments.

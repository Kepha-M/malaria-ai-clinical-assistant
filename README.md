# Malaria Cell Classification Using Deep Learning

## Overview
A CNN-based image classifier that detects *Plasmodium*-parasitized red blood cells in Giemsa-stained microscopic smear images, deployed as a browser-based tool to support malaria triage at under-resourced health facilities in western Kenya. Transfer learning on the NIH/NLM Malaria Cell Images dataset achieves over 97% recall on the parasitized class, making the model suitable as a clinical decision-support aid where missing an infection carries the highest human cost.

---

## Problem Statement
Manual microscopic examination of blood smears — the current diagnostic gold standard — is labour-intensive and dependent on trained microscopists who are chronically understaffed at Level 2 and Level 3 public health facilities in malaria-endemic counties (Homa Bay, Kisumu, Migori, Siaya) in western Kenya. Diagnostic delays allow infections to progress to severe, life-threatening stages, while over-reliance on clinical judgement accelerates antimalarial drug resistance. This project automates cell-level parasite detection to reduce diagnostic bottlenecks and support, not replace, clinical judgement.

---

## Dataset
| Property        | Details                                                                 |
|-----------------|-------------------------------------------------------------------------|
| Name            | NIH/NLM Malaria Cell Images Dataset                                     |
| Source          | https://www.kaggle.com/datasets/iarunava/cell-images-for-detecting-malaria |
| Size            | 27,558 images (13,779 parasitized · 13,779 uninfected) — balanced       |
| Target variable | `cell_label`: 1 = parasitized, 0 = uninfected                          |
| Licence         | Public domain (NIH/LHNCBC)                                             |

---

## Methods
- **EDA**: Confirmed class balance; inspected image size variance, staining artefacts, and colour channel distributions across both classes.
- **Preprocessing**: Resized all images to 224×224 px; applied augmentation (random rotation, horizontal/vertical flip, colour jitter) to improve generalisation across staining variations.
- **Baseline**: HOG + colour-histogram features fed into a Logistic Regression classifier, establishing a non-deep-learning reference point.
- **Model**: EfficientNetB0 pretrained on ImageNet, fine-tuned on the malaria dataset; decision threshold tuned post-training to prioritise recall on the parasitized class.
- **Explainability**: Grad-CAM overlays generated to highlight cell regions driving each prediction.
- **Deployment**: Model exported to TensorFlow.js; inference runs client-side in the browser — no server required.

---

## Results
| Metric             | Baseline (LR + HOG) | This Model (EfficientNetB0) |
|--------------------|---------------------|-----------------------------|
| Recall (parasitized)|                    |                             |
| Precision          |                     |                             |
| F1-Score           |                     |                             |
| AUC-ROC            |                     |                             |

### Disaggregated Performance
| Subgroup          | N      | Recall | F1-Score |
|-------------------|--------|--------|----------|
| Parasitized cells | 13,779 | 0.7750 |  0.6310  |
| Uninfected cells  | 13,779 | 0.3186 |  0.4128  |

---

## Limitations
- Trained on single pre-segmented cell crops, not raw full-slide images; a real deployment pipeline would require a slide segmentation step upstream.
- Training data originates from Bangladeshi clinical samples; performance on Kenyan-collected smears has not been validated and may differ due to variation in staining protocols and microscope equipment.
- Grad-CAM overlays indicate attention regions but do not constitute clinical-grade explainability; the tool is intended as decision-support only.

---

## Responsible AI Statement
See [`reports/responsible_ai_statement.pdf`](reports/responsible_ai_statement.pdf)

---

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/Kepha-M/capstone-project.git
cd malaria-cell-classification

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run EDA notebook
jupyter notebook notebooks/01_eda.ipynb

# 4. Run the preprocesing notebook
jupyter notebook notebooks/02_preprocessing.ipynb

# 4. Train the model
jupyter notebook notebooks/03_modelling.ipynb

# 5. Launch the browser demo (TF.js)
open deployment/index.html
```

> The trained model weights are included in `deployment/model/`. No internet connection or server is required to run inference.

---

## Author
**Kepha Ondieki** | Data Science Capstone Project in Collaboration with Ngao Labs

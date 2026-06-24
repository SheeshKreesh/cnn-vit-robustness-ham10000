# CNN vs ViT vs Hybrid: Robustness Benchmark on HAM10000

Companion code for the paper:
**"Architectural Robustness to Image Quality Degradation in Dermatological Disease Classification: A Comparative Study of CNN, Vision Transformer, and Hybrid Models"**

## Overview
This repository contains the experimental notebooks for a systematic robustness benchmark comparing three deep learning architectural paradigms under controlled image quality degradation in dermatological disease classification.

**Architectures compared:**
- EfficientNetB0 (Pure CNN)
- ViT-B/16 (Pure Vision Transformer)
- EfficientFormer-L1 (Hybrid CNN+Transformer)

**Degradation types evaluated:**
- Gaussian noise (5 severity levels)
- Gaussian blur (5 severity levels)
- Resolution downsampling (5 severity levels)
- JPEG compression (5 severity levels)

**Dataset:** HAM10000 — 10,015 dermatoscopic images, 7 disease classes

## Key Findings
- ViT-B/16 demonstrates consistently superior robustness under all degradation types despite lower clean image accuracy
- CNN (EfficientNetB0) suffers catastrophic degradation under blur and resolution downsampling (49-51pp F1 drop)
- Rare disease classes including Melanoma degrade faster than common classes — a patient safety concern
- Results replicated across 3 independent random seeds (42, 123, 456)

## Repository Structure
```
cnn-vit-robustness-ham10000/
├── README.md
├── skin_lesion_robustness_research.ipynb
├── seed_rerun_with_drive_v2.ipynb
├── f1_drop_summary.csv
├── robustness_stats_final.csv
├── robustness_curves_final.png
└── significance_tests.csv
```

## Requirements
```
torch
timm
scikit-learn
matplotlib
seaborn
pandas
Pillow
opencv-python
```

## Usage
1. Open in Google Colab
2. Upload Kaggle API credentials when prompted
3. Run all cells top to bottom
4. Results saved to Google Drive automatically

## Dataset
HAM10000 is available at:
https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000

## Citation
If you use this code, please cite:
[Your Name] et al. "Architectural Robustness to Image Quality Degradation in Dermatological Disease Classification." [Journal/arXiv], 2025.

## AI Disclosure
Code development and literature search were assisted by Claude (Anthropic). All experimental design, analysis, and writing were performed by the authors.

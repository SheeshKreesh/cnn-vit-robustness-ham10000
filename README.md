# CNN vs ViT vs Hybrid: Robustness Benchmark on HAM10000

Companion code for the paper:
**"Architectural Robustness to Image Quality Degradation in Dermatological Disease Classification: A Comparative Study of CNN and Vision Transformer Architectures at Matched Parameter Scale"**

## Overview

This repository contains the experimental notebooks for a systematic robustness benchmark comparing five deep learning architectures under controlled image quality degradation in dermatological disease classification. A key methodological contribution is the **matched-parameter-scale comparison** between ViT-Ti/16 (5.7M parameters) and EfficientNetB0 (4M parameters), which directly addresses the confound present in prior CNN vs ViT robustness comparisons.

**Architectures compared:**
- EfficientNetB0 — Pure CNN (4.0M parameters)
- ViT-Ti/16 — Pure Transformer (5.7M parameters) ← size-matched to CNN
- ViT-S/16 — Pure Transformer (22M parameters)
- ViT-B/16 — Pure Transformer (86M parameters)
- EfficientFormer-L1 — Hybrid CNN+Transformer (11.4M parameters)

**Degradation types evaluated (5 severity levels each):**
- Gaussian noise
- Gaussian blur
- Resolution downsampling
- JPEG compression

**Dataset:** HAM10000 — 10,015 dermatoscopic images, 7 disease classes, lesion-ID-aware train/test split

## Key Findings

- **Matched-scale comparison confirms architectural advantage:** ViT-Ti/16 (5.7M) outperforms EfficientNetB0 (4M) by 16.9pp under Gaussian blur, 14.4pp under resolution downsampling, and 12.0pp under JPEG compression — demonstrating robustness is architectural, not a parameter count effect
- **ViT scaling does not improve robustness:** ViT-Ti (5.7M), ViT-S (22M), and ViT-B (86M) achieve near-identical robustness drops under blur and resolution downsampling
- **EfficientFormer unexpected fragility:** Hybrid achieves highest clean accuracy but worst robustness with extreme variance (±12-17pp), making it unpredictable in deployment
- **Rare class patient safety finding:** Vascular Lesions, Basal Cell Carcinoma, and Melanoma suffer 50-81pp F1 drops vs 16-20pp for common Melanocytic Nevi — confirmed NOT a performance ceiling artifact
- Results replicated across 3 independent random seeds (42, 123, 456) with lesion-ID-aware split preventing data leakage

## Repository Structure

```
cnn-vit-robustness-ham10000/
├── README.md
├── definitive_experiment_v3.ipynb      # Main experiment (all 5 models, 3 seeds)
├── f1_drop_summary_v3.csv              # F1 drop per model per degradation type
├── robustness_stats_final_v3.csv       # Full mean ± std results (120 conditions)
├── significance_tests_v3.csv          # Statistical analysis (Wilcoxon tests)
├── perclass_drop_v3.csv               # Per-class F1 drops across all degradations
├── robustness_curves_v3.png           # Main figure — degradation curves with error bands
└── perclass_robustness_v3.png         # Per-class robustness across all 4 degradation types
```

## Methodological Notes

### Lesion-ID Aware Split
HAM10000 contains ~10,015 images from ~7,470 unique lesions. Naive random splitting risks the same lesion appearing in both train and test sets. We use `GroupShuffleSplit` by `lesion_id`, verified with zero overlap between splits.

### Matched-Scale Comparison
ViT-Ti/16 uses `augreg_in21k_ft_in1k` weights (ImageNet-21k pretrained, fine-tuned on ImageNet-1k) as direct ImageNet-1k weights are unavailable for this variant. All other models use standard ImageNet-1k weights. This pretraining difference is acknowledged as a limitation in the paper.

### Seeds
Three seeds (42, 123, 456) were run independently. Results reported as mean ± standard deviation.

## Requirements

```
torch
timm
scikit-learn
matplotlib
seaborn
pandas
scipy
Pillow
opencv-python
kaggle
```

## Usage

1. Open `definitive_experiment_v3.ipynb` in Google Colab
2. Set `CURRENT_SEED` at the top (42, 123, or 456)
3. Mount Google Drive when prompted — all checkpoints save automatically
4. Upload Kaggle API credentials when prompted
5. Run all cells top to bottom (~5-6 hours per seed on T4 GPU)
6. After all 3 seeds complete, run Part 2 to combine results

**Resume after disconnect:** The notebook detects existing Drive checkpoints and skips retraining completed models automatically.

## Dataset

HAM10000 is available at:
https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000

## Citation

If you use this code, please cite:

```
[Author Names]. "Architectural Robustness to Image Quality Degradation in
Dermatological Disease Classification: A Comparative Study of CNN and Vision
Transformer Architectures at Matched Parameter Scale." [Journal/arXiv], 2025.
```

## AI Disclosure

Code development and literature search were assisted by Claude (Anthropic). All experimental design, model training, data analysis, result interpretation, and final scientific conclusions were performed and verified by the authors.

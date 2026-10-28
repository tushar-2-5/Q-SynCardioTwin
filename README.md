# Q-SynCardioTwin v2

**Synthetic Multimodal Cardiac Dataset: MRI k-space + ECG | 500 Patients | 4 Disease Classes**

[![Dataset](https://img.shields.io/badge/Dataset-Kaggle-blue)](https://www.kaggle.com/datasets/tushar72345/qsyncardiotwin-v2)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Paper](https://img.shields.io/badge/Paper-Scientific%20Data-green)](https://www.nature.com/sdata/)

---

## What is Q-SynCardioTwin?

Q-SynCardioTwin v2 is a fully synthetic, privacy-safe, multimodal cardiac dataset designed for AI/ML research in medical imaging and cardiac diagnostics. It provides **paired MRI k-space data and ECG signals** for 500 virtual patients — removing the ethical and legal barriers of real patient data while preserving physiological realism.

**Why synthetic?**
- No patient privacy concerns — freely shareable
- Perfectly balanced disease classes — no class imbalance
- Ground truth available for every sample
- Fully reproducible (fixed seed)

---

## Dataset at a Glance

| Property | Value |
|----------|-------|
| Patients | 500 |
| Disease classes | NORMAL, HCM, DCM, INFARCT (125 each) |
| MRI resolution | 128×128 px |
| MRI coils | 8-coil k-space |
| Undersampling | R = 4, 8, 10, 12 |
| ECG leads | 12-lead |
| ECG length | 5000 samples @ 500 Hz (10 sec) |
| Cross-modal consistency | HR correlation r = 1.000 |
| Train / Val / Test | 348 / 72 / 80 |

---

## Technical Validation Results

### MRI Reconstruction — Attention U-Net (31.4M parameters)

| Acceleration R | PSNR (dB) | SSIM | NMSE |
|:--------------:|:---------:|:----:|:----:|
| R=4  | 31.24 | 0.9532 | 0.0032 |
| R=8  | 29.74 | 0.9470 | 0.0045 |
| R=10 | 29.24 | 0.9445 | 0.0050 |
| R=12 | 28.21 | 0.9390 | 0.0066 |

Best validation SSIM: **0.9540** (50 epochs, Adam lr=1e-4)

### Disease Classification — ROC-AUC (from reconstructed images)

| Class | AUC |
|-------|-----|
| NORMAL | 0.659 |
| HCM | 0.794 |
| DCM | 0.764 |
| INFARCT | 0.695 |

### Data Scaling (C1) — SSIM vs Training Set Size

| Training % | Patients | SSIM |
|:----------:|:--------:|:----:|
| 10% | 34 | 0.2766 |
| 25% | 87 | 0.2927 |
| 50% | 174 | 0.7040 |
| 75% | 261 | 0.8746 |
| 100% | 348 | 0.9591 |

### Transfer Learning (C2) — Fine-tuning on Real Samples (50 epochs)

| Real Samples (N) | SSIM |
|:----------------:|:----:|
| 0 (baseline) | 0.9532 |
| 5 | 0.9460 |
| 10 | 0.9461 |
| 20 | 0.9500 |
| 50 | 0.9547 |

---

## Figures

| Figure | Description |
|--------|-------------|
| fig1_overview | Dataset resource overview |
| fig2_pipeline | Multimodal generation pipeline |
| fig3_representative | Representative MRI samples per disease |
| fig4_kspace | k-space: full vs R=4 undersampled |
| fig5_directory_tree | Dataset directory structure |
| fig6_distributions | ECG parameter distributions (n=500) |
| fig7_crossmodal | Cross-modal HR correlation (r=1.000) |
| fig8_roc | ROC curves — disease classification |
| fig9_reconstruction | Reconstruction quality vs acceleration factor |
| figC1_scaling | Data scaling curve (C1) |
| figC2_transfer | Transfer learning curve (C2) |

---

## Repository Structure

Q-SynCardioTwin/
├── README.md
├── requirements.txt
├── notebook/
│ └── qsyncardiotwin_validation.ipynb
└── figures/
├── fig1_overview.png
├── fig2_pipeline.png
├── fig3_representative.png
├── fig4_kspace.png
├── fig5_directory_tree.png
├── fig6_distributions.png
├── fig7_crossmodal.png
├── fig8_roc.png
├── fig9_reconstruction_curves.png
├── figC1_scaling.png
└── figC2_transfer.png


---

## Quick Start

```python
import numpy as np
import json

# Load one patient record
kspace_full = np.load('patients/P0001/kspace_full.npz')['kspace']   # shape: (8, 128, 128)
kspace_R4   = np.load('patients/P0001/kspace_R4.npz')['kspace']     # shape: (8, 128, 128)
ecg         = np.load('patients/P0001/ecg_12lead.npz')['ecg']       # shape: (12, 5000)
phenotype   = json.load(open('patients/P0001/phenotype.json'))

print(f"Disease : {phenotype['disease']}")
print(f"Heart Rate: {phenotype['heart_rate_bpm']} bpm")
print(f"MRI k-space: {kspace_full.shape}")
print(f"ECG shape : {ecg.shape}")
```

---

## Requirements

torch>=1.12.0
torchvision>=0.13.0
numpy>=1.21.0
pytorch-msssim>=0.2.1
scikit-learn>=1.0.0
matplotlib>=3.5.0
scipy>=1.7.0


---

## Dataset Access

Full dataset (500 patients): **[Kaggle](https://www.kaggle.com/datasets/tushar72345/qsyncardiotwin-v2)**

Permanent DOI: `10.5281/zenodo.XXXXXXX` *(Zenodo upload in progress)*

---

## Citation

```bibtex
@article{qsyncardiotwin2026,
  title   = {Q-SynCardioTwin: A Synthetic Multimodal Cardiac Dataset for MRI Reconstruction and ECG Analysis},
  author  = {Tushar},
  journal = {Scientific Data},
  year    = {2026}
}
```

---

## License


[Creative Commons Attribution 4.0 International (CC BY 4.0)]

(https://creativecommons.org/licenses/by/4.0/) — Free to use with attribution.

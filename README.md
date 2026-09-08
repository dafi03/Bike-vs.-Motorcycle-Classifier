# Bicycle vs. Motorcycle Image Classification

![Python](https://img.shields.io/badge/Python-3.10%20%7C%203.11-blue?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.16-FF6F00?logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-MobileNetV2-D00000?logo=keras&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
![Status](https://img.shields.io/badge/Status-Completed-success)

A binary image classifier that distinguishes **bicycles** from **motorcycles**,
comparing a CNN trained from scratch against transfer learning with
MobileNetV2. Built as a course project for *Neural Network Project Work* at
ZHAW.

**Authors:** Dario Filippone & Domenik Bächler

---

## Motivation

Reliably telling motorcyclists and cyclists apart from images is a
prerequisite for automated helmet-compliance monitoring in traffic (see
[Kennedy et al., 2022](Paper/s12889-022-13075-2-2.pdf)). This project explores
how far a small, from-scratch CNN gets on this task, and how much transfer
learning from a pretrained backbone can improve it.

## Approach

Three models were trained and evaluated on the same train/validation/test
split:

| # | Model | Description |
|---|-------|-------------|
| 1 | **Baseline CNN** | Small convolutional network trained from scratch |
| 2 | **MobileNetV2 (frozen)** | ImageNet-pretrained backbone, only a new classification head is trained |
| 3 | **MobileNetV2 (fine-tuned)** | Same as above, plus the top ~30 backbone layers unfrozen and fine-tuned at a low learning rate |

## Results

| Model                    | Test Accuracy | Test Loss |
| ------------------------ | :-----------: | :-------: |
| Baseline CNN             |    70.5 %     |  0.7369   |
| MobileNetV2 (frozen)     |    88.5 %     |  0.3484   |
| **MobileNetV2 (fine-tuned)** | **90.2 %** | **0.2363** |

Transfer learning improved test accuracy by **+18 percentage points** over the
from-scratch baseline, and fine-tuning the backbone added another **+1.6 pp**
on top of the frozen-backbone model while roughly halving the test loss. All
numbers are computed on a held-out test set (15 % of the data) that was never
used during training or model selection — see Section 9 of ['2_project_pipeline.ipynb'](2_project_pipeline.ipynb)

confusion matrices.

## Project structure

```
neune_project/
├── 1_create_dataset.ipynb      # Step 1: merge & clean the two raw Kaggle datasets
├── 2_project_pipeline.ipynb    # Step 2: EDA, train/val/test split, training, evaluation
├── requirements.txt
├── README.md
├── 00_instructions/           # How to download the raw Kaggle datasets
├── Paper/                     # Reference paper (Kennedy et al., 2022)
├── 01_data_raw/                # Raw, unzipped Kaggle archives (not committed)
│   ├── archive (1)/
│   └── archive (2)/
├── 02_data_clean/              # Cleaned, de-duplicated images (not committed)
│   ├── bicycle/
│   └── motorcycle/
└── data_split/                 # Auto-generated train/val/test split (not committed)
```

> **Note:** `01_data_raw/`, `02_data_clean/` and `data_split/` are excluded via
> `.gitignore` and are generated locally by running the two notebooks in
> order — they are not part of this repository.

## Dataset

- Two classes: `bicycle`, `motorcycle`.
- Combined from two Kaggle datasets (see `00_instructions/` for download
  links and steps), de-duplicated by file hash, and re-encoded from `.webp`
  to `.jpg` where needed.
- Split **70 / 15 / 15** into train / validation / test using
  [`splitfolders`](https://pypi.org/project/split-folders/) with a fixed seed
  (`seed=42`) for reproducibility.
- Raw images are **not** committed to the repository — see setup below.

## Setup & reproduction

Requires **Python 3.10 or 3.11**.

```bash
# 1. Clone the repository
git clone https://github.com/dafi03/CNN.git
cd CNN

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Download the raw datasets
# Follow the instructions in 00_instructions/ to download and unzip the
# two Kaggle datasets into 01_data_raw/archive (1)/ and archive (2)/.

# 5. Run the notebooks in order
jupyter notebook 1_create_dataset.ipynb     # builds 02_data_clean/
jupyter notebook 2_project_pipeline.ipynb   # split, training, evaluation
```

Both notebooks use paths that are relative to the repository root and a
fixed random seed (`42`) throughout, so re-running them from a clean clone
reproduces the same split and (up to GPU/CPU non-determinism) comparable
results.

## Tech stack

TensorFlow / Keras · MobileNetV2 · NumPy · Pandas · Matplotlib · Pillow ·
split-folders · Jupyter

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for
details.

## Contact

| Name              | Contact                       |
| ----------------- | ------------------------------ |
| Domenik Bächler   | baechdom@students.zhaw.ch     |
| Dario Filippone   | filipda1@students.zhaw.ch     |

Submitted as part of the *Neural Network Project Work* course at ZHAW.

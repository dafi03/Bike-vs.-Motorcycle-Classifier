# Bicycle vs. Motorcycle Image Classification

**Course:** Neural Network Project Work
**Authors:** Bächler Domenik, Filippone Dario
**Date:** 29 May 2026
**Repository:** [github.zhaw.ch/baechdom/neune_project](https://github.zhaw.ch/baechdom/neune_project)

---

## Project overview

Binary image classifier that distinguishes **bicycles** from **motorcycles**.
We compare three approaches on the same train / validation / test split:

1. A small **baseline CNN** trained from scratch.
2. **MobileNetV2** with a frozen ImageNet-pretrained backbone (transfer learning).
3. The same **MobileNetV2 with the top layers fine-tuned** (final model).

Motivation: as discussed in [Kennedy et al. 2022](Paper/s12889-022-13075-2-2.pdf),
reliable detection of motorcyclists vs. cyclists is a prerequisite for
helmet-compliance monitoring in traffic.

## Results

| Model                    | Test accuracy | Test loss |
| ------------------------ | ------------- | --------- |
| Baseline CNN             | 70.5 %        | 0.7369    |
| MobileNetV2 (frozen)     | 88.5 %        | 0.3484    |
| MobileNetV2 (fine-tuned) | **90.2 %**    | **0.2363**|

## Dataset

Two classes (`bicycle`, `motorcycle`), stored as one folder per class in
`02_data_clean/`. The notebook re-encodes `.webp` files to JPEG, removes
files that TensorFlow cannot decode, and splits the data 70 / 15 / 15
using `splitfolders` with `seed=42`. The raw images are not committed to
the repository.

## Project structure

```
neune_project/
├── project_pipeline.ipynb   # Main notebook (cleaning, EDA, training, evaluation)
├── README.md
├── Paper/                   # Reference paper (Kennedy et al. 2022)
├── 02_data_clean/           # Cleaned source images, one folder per class
└── data_split/              # Auto-generated train/val/test folders
```

## Setup

Requires **Python 3.10 or 3.11**.

```bash
git clone https://github.zhaw.ch/baechdom/neune_project.git
cd neune_project
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook project_pipeline.ipynb
```

## Authors

| Name              | Contact                       |
| ----------------- | ----------------------------- |
| Bächler Domenik   | baechdom@students.zhaw.ch     |
| Filippone Dario   | filipda1@students.zhaw.ch     |

Submitted as part of the *Neural Network Project Work* course at ZHAW.

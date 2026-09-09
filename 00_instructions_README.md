# How to get the raw dataset

This project uses one Kaggle dataset as its raw data source:

**[Vehicles Image Dataset](https://www.kaggle.com/datasets/mmohaiminulislam/vehicles-image-dataset)**
by mmohaiminulislam

It contains images of several vehicle types (cars, trucks, buses, bicycles,
motorcycles, ...). `create_dataset.ipynb` automatically picks out only the
`bicycle` and `motorcycle` images and ignores the rest, so you don't need to
manually delete other vehicle folders.

## Option A — Download via browser (simplest, no account setup needed beyond a free Kaggle login)

1. Open the dataset page: https://www.kaggle.com/datasets/mmohaiminulislam/vehicles-image-dataset
2. Log in (or create a free Kaggle account if you don't have one).
3. Click the **Download** button (top right) to download the dataset as a
   `.zip` file (usually saved as `archive.zip`).
4. Unzip the downloaded file.
5. Move/rename the resulting folder so that it lives at exactly this path,
   relative to the repository root:

   ```
   01_data_raw/archive/
   ```

   The folder can contain nested subfolders (e.g. `train/bicycle/`,
   `test/motorcycle/`) — the notebook searches recursively, so the exact
   internal layout doesn't matter, only the top-level path `01_data_raw/archive/`.

## Option B — Download via the Kaggle CLI (faster if you download datasets often)

```bash
pip install kaggle

# Get an API token: Kaggle account settings -> "Create New API Token"
# This downloads a kaggle.json file. Place it at:
#   Linux/Mac: ~/.kaggle/kaggle.json
#   Windows:   C:\Users\<you>\.kaggle\kaggle.json

# From the repository root:
kaggle datasets download -d mmohaiminulislam/vehicles-image-dataset -p 01_data_raw --unzip
mv "01_data_raw/vehicles-image-dataset" "01_data_raw/archive"   # rename if needed, see note below
```

> **Note:** depending on the Kaggle CLI version, `--unzip` may extract
> directly into `01_data_raw/` without an extra subfolder, or create a
> subfolder named after the dataset. After extracting, check what actually
> landed in `01_data_raw/` and rename/move it so the final path is exactly
> `01_data_raw/archive/`.

## Verify you're set up correctly

After either option, run this from the repository root — it should print
`OK` and show at least a `bicycle` and a `motorcycle` folder somewhere
underneath:

```bash
python -c "
from pathlib import Path
root = Path('01_data_raw/archive')
found = {p.name.lower() for p in root.rglob('*') if p.is_dir()}
missing = {'bicycle', 'motorcycle'} - found
print('OK' if not missing else f'Missing: {missing}')
"
```

Once this passes, continue with `create_dataset.ipynb` as described in the
main `README.md`.

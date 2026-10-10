# ADS1002-zche0400

## Run the project notebook

Clone or download this repository, open `project_notebook.ipynb` from its root,
and run all cells in order using Python with pandas, matplotlib and scikit-learn.
All required input data are in this repository; no separate desktop folder or
GitHub login is needed to run a local checkout.

- `data/raw/Rossmann_train.csv.gz`: original supplied training CSV, losslessly
  compressed. The notebook loads this file and rebuilds the cleaned analytical
  data in memory before EDA and modelling.
- `data/cleaned/Rossmann_train_clean.csv.gz`: retained cleaned output reference.
- `data/cleaned/Rossmann_store_clean.csv`: retained reference only; not used by
  the notebook. The competition test dataset is not needed for this project.

Raw and cleaned training files are two stages of the same dataset. Notebook
execution does not overwrite either file. Pandas reads `.csv.gz` directly.

Original file supplied on 6 September 2026: `Rossmann_train.csv`.
SHA-256 of the **decompressed original CSV**:
`f6e4597c142d7d909a13d53b68a8e85c00b9a4c7b5ff40adbb37d6829cc1f4cc`.

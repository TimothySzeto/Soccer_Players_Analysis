# V1 market-value data preparation

This folder contains exploratory notebooks for joining 2024–25 player statistics with historical market values. The main result so far is a prepared dataset of 180 forwards with more than 500 minutes played. It does not contain a trained valuation model or model evaluation.

## Notebook order and purpose

1. `get_id_pair.ipynb` queries an external DuckDB mapping database and writes FBref–Transfermarkt ID pairs to `../../data/v1/id_pair.csv`.
2. `collect_and_clean_training_data.ipynb` is the main notebook. It joins FBref tables, adds market-value histories, filters forwards, examines features, and prepares training and test arrays.

`get_values.ipynb` is a one-player experiment for inspecting the market-value API response. `get_ml_data.ipynb` is an earlier, incomplete experiment for merging FBref tables. Neither is required to follow the main notebook's results.

## Inputs and current limitations

The notebooks expect local inputs under `../../data/v1/` when launched from this directory:

- `fbref/Big5 2024-2025 std.csv`
- `fbref/Big5 2024-2025 Playing Time.csv`
- `fbref/Big5 2024-2025 Shooting.csv`
- `fbref/Big5 2024-2025 Miscellaneous.csv`
- `reep-register-v1.duckdb` from the [Reep football entity register](https://github.com/withqwerty/reep) for `get_id_pair.ipynb`; the notebook generates `id_pair.csv` from it.

Those source files are not committed. The main notebook also makes live API requests and includes a few manual corrections to unmatched IDs or missing historical values. The saved outputs document the current exploration, but the notebooks cannot be rerun from a fresh clone without obtaining the source data and mapping database first.

The train/test split, IQR clipping, feature removal, and standardization are preparation for later modeling. They are not evidence of a completed prediction model.

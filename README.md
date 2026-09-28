# Soccer Player Analysis

An ongoing analysis of football player performance and market value. The current work combines 2024–25 statistics from Europe's five major leagues with historical Transfermarkt market values, then prepares a dataset of forwards for later modeling. An earlier notebook explores finishing relative to expected goals (xG).

**Current status:** data integration, exploratory analysis, and preprocessing are in the repository. A market-value prediction model and evaluation results have **not** been completed yet.

## What is implemented

- Merged FBref standard, playing-time, shooting, and miscellaneous statistics by player ID and club.
- Matched FBref IDs to Transfermarkt IDs using a DuckDB mapping table, then queried historical market values through an API.
- Selected forwards with more than 500 minutes played, producing 180 unique players (one row per player) for the 2024–25 analysis.
- Split the data into training and test sets, examined feature correlations, clipped outliers using training-set IQR bounds, removed selected features, and fit a standard scaler on the training set.

These are data-preparation steps. The repository does not yet report a trained model, prediction accuracy, or a ranking of players by overvaluation.

## Repository guide

| Path | Purpose |
| --- | --- |
| `notebooks/v0/soccer_players_project_v0.1.ipynb` | Goals-versus-xG finishing analysis. |
| `notebooks/v0/soccer_players_project_v0.2.ipynb` | Exploratory forward-performance scoring across four categories. |
| [`notebooks/v1/`](notebooks/v1/README.md) | Current market-value data preparation, notebook order, and required inputs. |

## Data and reproducibility

The notebooks read local files under `data/v1/`, which are **not included** in this repository. To rerun the v1 notebook, supply the four FBref CSVs expected in `data/v1/fbref/` (`std`, `Playing Time`, `Shooting`, and `Miscellaneous` for 2024–25), a player-ID mapping database, and the generated `data/v1/id_pair.csv`. The notebook uses relative paths, so run it from `notebooks/v1/`. It also requests market-value histories from the `tmapi.transfermarkt.technology` endpoint used in the notebook; responses can change or become unavailable.

The 2024–25 player statistics were collected by downloading the relevant tables directly from FBref. The FBref–Transfermarkt ID mapping comes from the [Reep football entity register](https://github.com/withqwerty/reep); market-value history is a separate input. The committed notebook outputs show the analysis state, but this is not yet a one-command reproducible pipeline.

## Earlier performance analysis

The first `v0` notebook compares goals scored with expected goals for players with more than 1,000 minutes. The second constructs a weighted percentile score for qualified forwards using goal scoring, creativity, defense, and ball control, with a small subjective league adjustment. Those descriptive scores are separate from the newer market-value dataset in `v1`; they are not validated predictions of player value.

## Next steps

- Check that each historical market value lines up with the intended season and prediction date.
- Establish a simple baseline, train a model, and evaluate it on held-out data before drawing valuation conclusions.
- Document the data acquisition and preparation steps so the analysis can be reproduced without manual file placement.

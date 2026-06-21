# Tennis Predictions

---

A notebook-driven project for predicting ATP men's singles match winners using match-level and player-level data.

## Overview

- Raw data sourced from [JeffSackmann](https://github.com/JeffSackmann)'s [tennis_atp](https://github.com/JeffSackmann/tennis_atp) repository. As of June 21, 2026 data has either been made private or shifted to his [MatchCharteringProject](https://github.com/JeffSackmann/tennis_MatchChartingProject).
- Focus on ATP main tour, futures, and qualifying/challenger matches from 2020 to 2024.
- Notebook pipeline cleans raw CSVs, extracts match features, and trains a KNN classifier.
- Final model achieves over 99% test accuracy on the selected features.

## Repository structure

- `environment.yml` - Conda environment definition.
- `preprocessing.ipynb` - Data cleaning and feature engineering.
- `knn/knn.ipynb` - KNN modeling, hyperparameter search, and evaluation. (Figures also in `knn` directory.)
- `atp_match_data/` - Raw CSV downloads and data dictionary.
- `processed_data/` - Engineered datasets saved for analysis and modeling.

## Setup

1. Create and activate the conda environment.

   ```powershell
   conda env create -f environment.yml
   conda activate tennis
   ```

2. Navigate to the `atp_match_data` directory and run `fetch_data_command.txt`. (Command may not work due to original database status.)

3. Open and run the notebooks in the `tennis` environment.

## Raw data sources

- `atp_matches_2020.csv` through `atp_matches_2024.csv`
- `atp_matches_futures_2020.csv` through `atp_matches_futures_2024.csv`
- `atp_matches_qual_chall_2020.csv` through `atp_matches_qual_chall_2024.csv`

## Preprocessing pipeline

- Load yearly ATP, futures, and qual/challenger match files.
- Concatenate data across 2020-24.
- Remove invalid `score` entries.
- Split scores into sets played, winner/loser game totals, and tiebreak counts.
- Drop low-value or sparse metadata columns.
- Separate detailed match stats into `relevant_stats.csv`.
- One-hot encode player hand: right, left, ambidextrous.
- Cast numeric columns to compact numeric types.
- Drop rows with missing values.
- Create crossed features and interactions (preserve order):
  - compute differences: `age_diff`, `height_diff`, `rank_points_diff`.
  - compute asymmetry / interaction features where useful.
  - place crossed-feature creation after one-hot encoding and type-casting, and before final NaN-dropping and saving.
- Swap winner and loser and shuffle adding a `winner_1` column for training.

## Processed outputs

- `processed_data/combined.csv`
  - 85,813 matches
  - 24 features
  - Contains cleaned match-level feature set for modeling
- `processed_data/relevant_stats.csv`
  - 119,422 rows
  - Includes detailed in-match statistics and set-level summaries
- `processed_data/train_test.csv`
  - 85,813 rows
  - 25 columns including target `winner_1`
  - Final dataset used by `knn/knn.ipynb`

## Modeling pipeline

- Load `processed_data/train_test.csv`.
- Define four candidate feature sets:
  - `crossed`
  - `individual`
  - `mixed`
  - `all`
- Scale numeric features and passthrough one-hot encoded features.
- Search KNN hyperparameters:
  - `n_neighbors`
  - `weights` = `uniform` or `distance`
  - `p` = 1 or 2
- Use 5-fold cross-validation for selection.

## Feature set refinement

- Examine feature importance with PDPs and make new feature sets.
- `mixed` - full combination of raw and crossed features.
- `set_1` - lightweight subset excluding weak PDP features.
- `set_2` - smaller subset without `rank_points_diff`.

## Final model findings

- Best KNN parameters:
  - `n_neighbors = 65`
  - `weights = distance`
  - `p = 2` (Euclidean)
- Final model trained on `mixed` features for small accuracy boost.
- Accuracy exceeds 99% on the test split.

## Feature importance insights

- Most important predictors:
  - heights
  - rank point totals
  - rank point differences
- Less impactful features:
  - age differences
  - pressure level
  - court speed, bounce, friction
  - asymmetry
- `rank_points_diff` adds strong predictive value even with rank point totals.

## Evaluation notes

- Model performance analyzed across:
  - tournament pressure level
  - rank point difference bands
  - height difference bands
  - age difference bands
- Accuracy is stable across these slices, with rank-point gaps especially predictive.
- Slight dip closer to 90% accuracy in similar rank matches

## Usage

- Run `preprocessing.ipynb` first to generate cleaned datasets.
- Run `knn/knn.ipynb` to reproduce model training and evaluation.
- Use `processed_data/train_test.csv` for future classifiers.
- Final model stored at `knn/knn.pkl`

## Notes

- The repo is notebook-centric: preprocessing and modeling are contained in Jupyter notebooks.
- `environment.yml` installs the core analysis stack for this project.
- If `scikit-learn` or `joblib` are missing, install them in the `tennis` environment.

## Possible future updates

- Playstyle insights
- UI

## Acknowledgements

- Data source: Jeff Sackmann's tennis_atp repository.

## License

MIT License

Copyright (c) 2026 Karthikeya Turimalla

## Citation

If you use this project in research, coursework, publications, or derivative works,
please cite or reference this repository.

Author: Karthikeya Turimalla (@TKAman23)
Repository: <https://github.com/TKAman23/TennisPredictions>
Year: 2026

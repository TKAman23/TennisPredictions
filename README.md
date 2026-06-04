# Tennis Predictions

---

This project predicts the outcomes of men's singles tennis matches using match and player data from the ATP tour.
Original Data: [JeffSackmann](https://github.com/JeffSackmann)'s [tennis_atp](https://github.com/JeffSackmann/tennis_atp) database

## Repo Structure

```text
main
    atp_match_player_data - original data
        fetch_data_command.txt - command to fetch data in powershell
        matches_data_dictionary.txt - information about data features
    preprocessing.ipynb
```

## Setup

Use conda to set up your environment from environment.yml in the root directory.

```powershell
conda env create -f environment.yml
conda activate tennis
```

Then run `fetch_data_command.txt` in PowerShell inside the data directory to download the raw ATP data. Open and run `preprocessing.ipynb` in the `tennis` environment to generate cleaned and combined datasets.

## Procedure

### Preprocessing

- Load raw ATP match and player files from `atp_match_player_data`.
- Standardize column names and formats for dates, player IDs, and match scores.
- Filter the dataset to singles tournament matches only.
- Combine annual match files into a single master dataset for model training.
- Save the cleaned dataset to disk for use in exploratory analysis and modelling.

### Exploratory Data Analysis

- Review class balance for match outcomes and surface types.
- Examine distributions for player statistics, ranking differences, and match durations.
- Identify correlations between features such as surface, ranking gap, and match result.
- Use visual analysis to check for trends across seasons and player performance patterns.

### Modelling

## Results

## Extensions

## Acknowledgements

- Data source: JeffSackmann's tennis_atp repository.
- Project structure and preprocessing guided by the available ATP match data files.

## License

- Include license information here.

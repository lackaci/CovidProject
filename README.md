
# COVID-19 Case Forecasting

This notebook studies weekly COVID-19 case and death trends from 2020 to 2022 together with recorded government response measures. It builds short-term forecasts for selected European countries using historical case/death rates and policy features, comparing an MLP with one-headed and two-headed LSTM models.

## Headline result

In the full five-seed, 40-epoch comparison, lagged-log-rate ridge was the only evaluated method to beat persistence: MAE 145.664 versus 174.249 cases per 100,000 (relative MAE 0.836). The MLP and both LSTM models did not beat the persistence null on this test period; regime-specific and rolling-origin analyses remain open.

## Results figure

**TODO:** add the final seed-averaged forecast figure at `docs/img/results-figure.png` and link it here once the evaluation is final.

## Setup

- Python 3.13.2 is recorded in the notebook metadata.
- Install the pinned dependencies with:

	```powershell
	python -m pip install -r requirements.txt
	```

- The pinned environment was captured from Python 3.11.0. The notebook metadata records Python 3.13.2; if TensorFlow installation fails on Python 3.13, use the nearest supported Python version instead.

## How to run

1. Open `covid_project_clean.ipynb` in VS Code or Jupyter.
2. Select the project Python environment and run all cells from top to bottom.
3. For a quick check, set `COVID_FAST=1` before starting the notebook process:

	 ```powershell
	 $env:COVID_FAST = "1"
	 ```

The notebook uses relative paths and expects its input files under `Data/`. The full LSTM experiment trains several random seeds and can take several minutes on a CPU.

## Data sources

- [ECDC archived weekly COVID-19 cases and deaths](https://www.ecdc.europa.eu/en/publications-data/download-historical-data-20-june-2022-weekly-number-new-reported-covid-19-cases)
- [ECDC archived country response measures](https://www.ecdc.europa.eu/en/publications-data/download-data-response-measures-covid-19)

The notebook downloads the verified source files below into `Data/` when they are missing, then reuses the local copies on later runs:

- Weekly cases and deaths: `https://opendata.ecdc.europa.eu/covid19/nationalcasedeath_archive/csv/data.csv`
- Response measures: `https://www.ecdc.europa.eu/sites/default/files/documents/response_graphs_data_2022-08-25.csv`

The repository's working data files are `Data/data_new.csv`, `Data/data_old.csv`, and `Data/response_graphs_data_2022-08-25.csv`. ECDC has marked these historical datasets as archived and no longer updates them.

## Project structure

```text
covid_project_clean.ipynb  Main analysis and forecasting workflow
Data/                      Local input data files
docs/                      Usage notes, workflow guidance, and improvement plan
LICENSE                    MIT license
```

## Limitations

- The data covers a historical period ending in 2022 and is not a current surveillance system.
- The country sample and policy variables are limited to the data available in the archived files.
- Chronological prediction performance does not establish that policies caused changes in cases.
- The final baseline comparison, regime-shift analysis, and uncertainty summary are still TODOs in the improvement plan.
- Results may vary with TensorFlow, hardware, and random seed unless the recorded environment is reproduced.

## License

This project is released under the [MIT License](LICENSE).

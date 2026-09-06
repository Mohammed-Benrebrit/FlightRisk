# Dataset and reproduction notes

## Source

- Dataset: [Flight Delay Dataset - 2024](https://www.kaggle.com/datasets/hrishitpatil/flight-data-2024), published on Kaggle by `hrishitpatil`.
- Dataset identifier: `hrishitpatil/flight-data-2024`.
- Original executed notebook: [Mohammed Benrebrit's Phase 1 EDA](https://www.kaggle.com/code/mohammedbenrebrit/notebooka582a385a1).
- Notebook version reviewed: version 1, ID `347804282`, published 6 September 2026.
- The notebook metadata records dataset-version source ID `13128322`. This is a Kaggle metadata identifier, not a dataset file hash.

The notebook discovers three source files:

| File | Use in Phase 1 |
|---|---|
| `flight_data_2024_sample.csv` | Analysis input: 10,000 flight records, 35 raw variables |
| `flight_data_2024_data_dictionary.csv` | Dictionary: 35 rows and four descriptive columns |
| `flight_data_2024.csv` | Located but not loaded for the published sample analysis |

The dictionary's `null_pct` values are supplied metadata. They are not measured missingness percentages for the 10,000-row sample. Actual sample target missingness is 164/10,000 = 1.64%.

## Acquire the data

Open the dataset page and obtain the sample CSV and dictionary using Kaggle's download controls. Keep the original filenames. The full CSV is not necessary for reproducing this sample EDA and may be substantially larger.

Do not commit the data. Store local copies as:

```text
data/raw/flight_data_2024_sample.csv
data/raw/flight_data_2024_data_dictionary.csv
```

The repository's `.gitignore` excludes `data/raw/`, `data/interim/` and `data/processed/`. Verify the dataset's current licence and original provenance before redistribution or broader reuse. This update does not assert a verified data licence or redistribute raw records.

## Original Kaggle execution

`notebooks/01_eda.ipynb` is the unchanged downloaded export, including its existing outputs. Kaggle displayed a successful run; the export has 11 executed code cells, 14 PNG plots and no saved error outputs. Python 3.12.13 is recorded in its metadata.

The notebook imports NumPy, pandas, Matplotlib and kagglehub. Its Kaggle input directory is:

```text
/kaggle/input/datasets/hrishitpatil/flight-data-2024/
```

Open the linked Kaggle notebook to inspect its saved results or create your own editable copy with the same dataset attached. A newer dataset version may not reproduce the exact figures.

## Running locally (setup not yet validated)

The notebook deliberately retains its original code and Kaggle paths. It is not yet a one-command local pipeline.

1. Create a Python environment and install `numpy`, `pandas`, `matplotlib`, `kagglehub` and `jupyterlab`. Exact library versions were not recorded in the published export; a tested dependency lock is Phase 2 work.
2. Download the sample and dictionary into `data/raw/` as above.
3. Open a working copy of the notebook. When its kernel is launched with `notebooks/` as the working directory, replace the two hard-coded CSV paths with `Path('../data/raw/flight_data_2024_sample.csv')` and `Path('../data/raw/flight_data_2024_data_dictionary.csv')`. Adjust those relative paths if the working directory differs.
4. The initial `/kaggle/input` directory listings are informational only; replace or omit those listings in the local working copy.
5. Restart the kernel and run all cells from top to bottom. Confirm the input shape and target counts before comparing the plots.

Do not silently update saved outputs or claim a successful local rerun without actually executing against the source data. The publication review checked code syntax, saved outputs and document consistency; it did not independently rerun the full notebook locally.

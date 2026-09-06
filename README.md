# FlightRisk

Exploring flight arrival-delay risk using information available **before departure**.

**Phase 1: exploratory data analysis completed on a 10,000-flight sample.**
Model-dataset preparation is next. No model has been trained or evaluated, and no deployment or predictive-performance results are claimed.

[View the EDA notebook](notebooks/01_eda.ipynb) | [Read the Phase 1 report](reports/FlightRisk_Phase1_EDA_Report.pdf) | [Open on Kaggle](https://www.kaggle.com/code/mohammedbenrebrit/notebooka582a385a1)

## What I investigated

The first phase uses Python, pandas, NumPy and Matplotlib to understand the target before choosing a model:

- Reviewed the data dictionary and the 35-variable sample structure.
- Investigated every missing arrival-delay target using cancellation and diversion flags.
- Explored delay counts, percentiles, long-tail behaviour and differences between carriers.
- Compared scheduled departure hour, weekday, month, airports, distance and scheduled duration with observed delays.
- Extracted repeated grouped summaries and plotting into `summarize_delay_by` and `ploting_summarize` helper functions.
- Documented a prediction-time leakage policy and the next modelling steps.

## Findings from the sample

| Measure | Observed value |
|---|---:|
| Flight records / raw variables | 10,000 / 35 |
| Defined arrival-delay targets | 9,836 |
| Missing targets | 164: 122 cancellations + 42 diversions |
| Early arrivals | 6,092 |
| Arrivals 0 to less than 15 minutes late | 1,625 |
| Arrivals at least 15 minutes late | 2,119 / 21.54% of defined targets |
| 99th percentile arrival delay | 217 minutes |
| Maximum observed arrival delay | 2,014 minutes |

Later scheduled departures generally have higher observed delay rates than morning departures in this sample. Carrier, weekday, month and airport groups also differ. Very small overnight-hour and duration groups need caution; the airport comparisons restrict attention to airports with more than 100 sample records.

These are **descriptive associations, not causal conclusions or validated feature importance**. The sample EDA was performed before a chronological split. It is not an untouched test set, and the findings still need validation on later, unseen flights.

## Prediction contract and leakage policy

- **Regression target:** `arr_delay` in minutes; negative values mean early arrival.
- **Classification target:** `arr_delay >= 15`, defined only where `arr_delay` is present. A missing outcome is not an on-time flight.
- **Initial modelling population:** completed, non-diverted flights with a defined target. Cancellation/diversion prediction is a separate future task.
- **Eligible candidates:** scheduled times, calendar fields, operating carrier, origin, destination, distance and scheduled elapsed time, subject to prediction-time availability checks.
- **Not eligible as pre-departure predictors:** actual departure/arrival times and delays, taxi/wheels/airborne timings, actual elapsed time, retrospective delay-cause fields, or final cancellation/diversion outcomes.

Outcome fields are inspected during EDA to understand the labels; that does not make them valid predictors. Future aggregates must use past data only, and preprocessing must be fitted on training data only.

## Explore or reproduce

The committed notebook is the **unchanged executed export** of Kaggle version 1 (version ID `347804282`), published on 6 September 2026. It contains 11 executed code cells and 14 saved plots. Its recorded runtime is Python 3.12.13.

For the original environment, open the [Kaggle notebook](https://www.kaggle.com/code/mohammedbenrebrit/notebooka582a385a1). The export retains Kaggle-specific input paths. See [data acquisition and local-running notes](data/README.md) before trying it on a laptop. This milestone does not yet provide a tested local environment or locked dependencies.

The PDF is preserved as the original milestone report. Read the accompanying [evidence and review notes](reports/EDA_NOTES.md) for scope clarifications, including one report finding that is not present in the published code.

## Roadmap

| Phase | Status |
|---|---|
| 1. Problem framing and sample EDA | Completed; notebook and report published |
| 2. Modelling dataset, local setup and quality checks | Next |
| 3. Chronological split and trivial baselines | Planned |
| 4. Preprocessing, linear and logistic regression | Planned |
| 5. Reusable modules and automated tests | Planned |
| Tree models, neural networks, weather/history features, API, Docker and dashboard | Future extensions |

See [PROJECT_PLAN.md](PROJECT_PLAN.md) for the detailed checklist. A duplicate/schema audit, verified dataset provenance and licence, full-data coverage checks and an untouched evaluation period remain open work.

## Repository contents

```text
FlightRisk/
|-- README.md
|-- PROJECT_PLAN.md
|-- notebooks/01_eda.ipynb
|-- reports/FlightRisk_Phase1_EDA_Report.pdf
|-- reports/EDA_NOTES.md
`-- data/README.md
```

Raw data, credentials, virtual environments and model artifacts are not committed. `src/` and `tests/` will be introduced when they contain implemented work.

## Learning context

FlightRisk is a personal learning project by Mohammed Benrebrit, developed while preparing for the JKU Artificial Intelligence bachelor's programme starting in October 2026 and building on [Supervised Machine Learning: Regression and Classification](https://coursera.org/verify/XDL7FP8XI4NM).

Progress is published incrementally. Completed analysis, planned engineering work and future model results are kept separate.

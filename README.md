# FlightRisk

FlightRisk is a learning-focused machine learning project for predicting flight arrival-delay risk using information available **before departure**.

The project currently uses a 10,000-flight sample from the 2024 Flight Delay Dataset and supports two prediction tasks:

- **Regression:** predict arrival delay in minutes.
- **Classification:** predict whether a flight will arrive at least 15 minutes late.

The project is being developed incrementally, with emphasis on understanding the full ML workflow rather than only training a model.

## Current status

### Phase 1 — Exploratory Data Analysis ✅

The first phase focused on understanding the dataset and defining a valid prediction problem.

Main findings from the 10,000-flight sample:

| Measure | Result |
|---|---:|
| Raw flight records | 10,000 |
| Raw variables | 35 |
| Valid arrival-delay targets | 9,836 |
| Cancelled flights | 122 |
| Diverted flights | 42 |
| Flights at least 15 minutes late | 2,119 |
| Significant-delay rate | 21.54% |
| Median arrival delay | -6 min |
| 99th percentile arrival delay | 217 min |
| Maximum observed delay | 2,014 min |

EDA showed differences across departure time, carrier, weekday, month and airports. The arrival-delay distribution is strongly right-skewed: most flights are close to schedule, while a smaller number of severe delays strongly affect the mean.

These findings are descriptive associations, not causal conclusions or validated feature importance.

[View Phase 1 EDA](notebooks/01_eda.ipynb) | [Read the EDA report](reports/FlightRisk_Phase1_EDA_Report.pdf) | [Original Kaggle notebook](https://www.kaggle.com/code/mohammedbenrebrit/notebooka582a385a1)

---

### Phase 2 — Modeling Dataset Preparation ✅

Phase 2 moved the project from exploratory analysis into a local, reproducible ML workflow.

The main work completed in this phase includes:

- Set up the project locally with Python 3.12 and a virtual environment.
- Added project dependencies through `requirements.txt`.
- Defined the modeling population as completed, non-diverted flights with a valid `arr_delay`.
- Reduced the modeling population from 10,000 to **9,836 valid observations**.
- Added assertions for important data assumptions.
- Checked exact duplicate records.
- Investigated flight identifiers and found a sample-level candidate composite key using:
  - flight date
  - operating carrier
  - flight number
  - origin airport
- Defined an explicit feature allowlist and separated future/outcome leakage variables.
- Created deterministic features such as:
  - month
  - weekday
  - scheduled departure hour
- Validated the scheduled HHMM time representation.
- Created both regression and classification targets.
- Built a chronological train/validation/test split.
- Added checks for row coverage, X/y alignment and temporal separation.

[View Phase 2 data preparation](notebooks/02_data_preparation.ipynb)

## Prediction contract

FlightRisk represents a prediction made **before departure**.

This means the model may use scheduled and pre-departure information such as:

- operating carrier
- origin and destination
- scheduled departure and arrival information
- scheduled duration
- distance
- calendar/time features

Information produced after the prediction point is excluded.

Examples of leakage variables include:

- actual departure time
- departure delay
- taxi and wheels-off/on times
- actual arrival time
- actual elapsed time
- air time
- retrospective delay-cause fields

Cancellation and diversion outcomes are used to define the historical modeling population, but they are not valid pre-departure predictors for this task.

## Chronological validation strategy

Instead of randomly mixing flights across the year, the current experiment follows time order:

| Split | Period | Rows |
|---|---|---:|
| Train | January–August | 6,483 |
| Validation | September–October | 1,764 |
| Test | November–December | 1,589 |

The classification target distribution also changes over time:

| Split | 15+ minute delay rate |
|---|---:|
| Train | 23.91% |
| Validation | 13.55% |
| Test | 20.77% |

This temporal variation is intentional rather than corrected away: future flight conditions may differ from historical training conditions.

The Phase 1 sample was explored before this split existed, so the current test period is protected from model fitting from Phase 2 onward, but it should not be described as a completely untouched holdout.

## Leakage and evaluation rules

The project follows several rules throughout modeling:

1. Only information available at prediction time may enter the feature set.
2. Missing targets are never converted into negative classification labels.
3. Future rows must not influence historical features or aggregates.
4. Preprocessing that learns from data must be fitted on the training set only.
5. Validation data may guide model-development decisions.
6. Test data is reserved for final evaluation after model decisions are made.
7. Model performance will always be compared against simple baselines.

## Next phase

### Phase 3 — Baselines and preprocessing

The next stage will prepare the features for machine learning and establish simple reference performance before training real models.

Planned topics include:

- numeric vs categorical features
- one-hot encoding
- scaling where appropriate
- `ColumnTransformer`
- preprocessing pipelines
- regression baseline
- classification baseline
- evaluation metrics

After that, the first ML models will be:

- **Linear Regression** for arrival-delay minutes
- **Logistic Regression** for 15+ minute delay classification

## Roadmap

| Phase | Status |
|---|---|
| Problem framing and prediction contract | Completed |
| Phase 1 — Exploratory Data Analysis | Completed |
| Local development environment | Completed |
| Phase 2 — Modeling Dataset Preparation | Completed |
| Phase 3 — Baselines and preprocessing | Next |
| Phase 4 — Linear and Logistic Regression | Planned |
| Phase 5 — Evaluation and error analysis | Planned |
| Reusable modules and automated tests | Planned |
| Tree models, richer historical/weather features | Future |
| API, Docker and reporting dashboard | Future |

## Repository structure

```text
FlightRisk/
├── README.md
├── PROJECT_PLAN.md
├── requirements.txt
├── data/
│   └── README.md
├── notebooks/
│   ├── 01_eda.ipynb
│   └── 02_data_preparation.ipynb
└── reports/
    ├── FlightRisk_Phase1_EDA_Report.pdf
    └── EDA_NOTES.md
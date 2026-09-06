# FlightRisk Project Plan

This document is the working roadmap for FlightRisk. It converts the original project plan into a format that can evolve alongside the repository.

> **Status (6 September 2026):** Phase 1 sample EDA completed and published. Phase 2 modelling-dataset preparation and local reproducibility are next. No trained-model results are claimed.

## 1. Goal

Build a reproducible machine learning project that estimates flight-delay risk **before departure**.

The project has two learning objectives:

- **Regression:** estimate arrival delay in minutes.
- **Classification:** estimate the probability that arrival delay is at least 15 minutes.

The first version is designed to demonstrate correct problem framing, leakage prevention, temporal validation, baseline comparison, and clear communication—not only model complexity.

## 2. Prediction contract

### Prediction time

Predictions must represent a decision made before departure. A feature is allowed only if it would be available at that moment.

### Initial targets

- Regression target: arrival delay in minutes.
- Classification target: `1` when arrival delay is at least 15 minutes, otherwise `0`, only for records with a defined arrival-delay target. Missing targets must never be converted to negative-class labels.

### Version 0.1 population

- Completed flights
- Non-diverted flights
- Records with sufficient target information

Cancellations and diversions will be handled as separate future problems rather than mixed into the first target definition.

## 3. Initial feature scope

The first baseline feature set may include:

- Scheduled departure and arrival information
- Calendar features derived from the schedule
- Airline or operating carrier
- Origin airport
- Destination airport
- Route distance
- Scheduled elapsed time in its original minutes

Possible later additions include weather and historical aggregate features. Any historical feature must be calculated using past information only.

## 4. Leakage rules

The following rules apply throughout the project:

1. Do not use actual departure, taxi, airborne, landing, or arrival information when predicting before departure.
2. Do not create aggregates using future rows or the complete dataset.
3. Fit preprocessing steps only on the training period.
4. Keep a feature-availability record explaining when each feature becomes known.
5. Treat unexpectedly strong validation results as a reason to investigate leakage.
6. Exclude retrospective delay-cause fields and final cancellation/diversion outcomes from pre-departure predictors. Their use in target investigation is not permission to use them as features.

## 5. Validation strategy

Random splitting can make flight data look easier than it is. FlightRisk will use chronological train, validation, and test periods so evaluation better represents prediction on future flights.

The exact date boundaries will be documented after the dataset and its coverage have been confirmed.

The Phase 1 notebook explored the supplied 10,000-row sample before splitting. Do not describe any of that sample as an untouched holdout. Record its dates and overlap when selecting later validation/test data; conduct subsequent feature selection and fitted preprocessing using the training period only.

## 6. Baseline-first modelling

Before more complex models, the project will establish simple reference points.

### Regression

- Constant or historical-average prediction
- Linear regression

### Classification

- Majority-rate or constant-probability prediction
- Logistic regression

More complex tree models or neural networks will be considered only after the data pipeline and baselines are trustworthy.

## 7. Development phases

Phase numbering now aligns with the published Phase 1 EDA report. The earlier foundation and data-understanding phases have been consolidated; modelling preparation is the new Phase 2.

### Phase 1 - problem framing and sample EDA (completed)

- [x] Define prediction timing and targets
- [x] Define initial scope and leakage policy
- [x] Locate the Kaggle sample, full-data file and data dictionary
- [x] Review dictionary fields and inspect sample dimensions (10,000 rows, 35 raw columns)
- [x] Trace 164 missing arrival-delay targets to 122 cancellations and 42 diversions
- [x] Inspect target counts, percentiles and long-tail behaviour
- [x] Compare carrier, departure hour, weekday, month, airports, distance and scheduled duration
- [x] Extract repeated grouped analysis and plotting into notebook helper functions
- [x] Publish the executed notebook, original report, evidence notes and data-acquisition instructions

### Phase 2 - modelling dataset and reproducibility (next)

- [ ] Establish and test a local environment; record dependency versions
- [ ] Record acquisition date, dataset version, file hashes, provenance and licence
- [ ] Audit actual raw dtypes, missingness, duplicates and flight identifiers (not just the supplied dictionary)
- [ ] Verify sample/full-data date coverage, representativeness and category cardinality
- [ ] Preserve raw data and explicitly define the valid-target modelling population
- [ ] Implement an allowlist of pre-departure features and assertions excluding outcome fields
- [ ] Validate scheduled HHMM values and handle any 2400/midnight cases explicitly
- [ ] Keep scheduled duration in minutes; do not automatically use visualization buckets as model features
- [ ] Resolve the report's day-of-month finding with an explicit analysis or remove that hypothesis
- [ ] Record chronological split boundaries and protect an evaluation period not used in exploration
- [ ] Construct aligned X/y data without inventing missing labels

### Phase 3 — baselines

- [ ] Implement chronological splits
- [ ] Build trivial regression baseline
- [ ] Build trivial classification baseline
- [ ] Define evaluation metrics and record assumptions

### Phase 4 — first machine learning models

- [ ] Build preprocessing pipelines
- [ ] Train linear regression
- [ ] Train logistic regression
- [ ] Compare models with baselines
- [ ] Analyse errors and data limitations

### Phase 5 — engineering quality

- [ ] Move reusable logic from notebooks into `src/`
- [ ] Add tests for data and feature logic
- [ ] Add configuration and reproducible commands
- [ ] Improve documentation and result reporting

### Future extensions

- Weather features
- Leakage-safe historical route, airport, or airline aggregates
- Tree-based models
- Neural-network experiments
- Separate cancellation or diversion models
- API and Docker packaging
- Reporting dashboard

## 8. Repository structure after Phase 1

```text
FlightRisk/
|-- README.md
|-- PROJECT_PLAN.md
|-- notebooks/01_eda.ipynb
|-- reports/FlightRisk_Phase1_EDA_Report.pdf
|-- reports/EDA_NOTES.md
`-- data/README.md
```

`src/` and `tests/` will be added when they contain real work. Raw, interim and processed data remain ignored. The notebook currently preserves its original Kaggle paths and execution outputs; a validated local entry point is still planned.

## 9. Progress and reporting rules

- Commit work incrementally with clear messages.
- Label unfinished work honestly.
- Do not publish performance numbers without a documented validation split.
- Record important assumptions and changes in the repository.
- Prefer a clear baseline and trustworthy evaluation over an unnecessarily complex model.

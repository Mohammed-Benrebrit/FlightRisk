# FlightRisk Project Plan

This document is the working roadmap for FlightRisk. It converts the original project plan into a format that can evolve alongside the repository.

> **Status:** Early development. Problem framing is complete; data exploration and environment setup are in progress. No model results are claimed yet.

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
- Classification target: `1` when arrival delay is at least 15 minutes, otherwise `0`.

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

Possible later additions include weather and historical aggregate features. Any historical feature must be calculated using past information only.

## 4. Leakage rules

The following rules apply throughout the project:

1. Do not use actual departure, taxi, airborne, landing, or arrival information when predicting before departure.
2. Do not create aggregates using future rows or the complete dataset.
3. Fit preprocessing steps only on the training period.
4. Keep a feature-availability record explaining when each feature becomes known.
5. Treat unexpectedly strong validation results as a reason to investigate leakage.

## 5. Validation strategy

Random splitting can make flight data look easier than it is. FlightRisk will use chronological train, validation, and test periods so evaluation better represents prediction on future flights.

The exact date boundaries will be documented after the dataset and its coverage have been confirmed.

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

### Phase 1 — foundation

- [x] Define prediction timing and targets
- [x] Define initial scope and leakage policy
- [ ] Confirm dataset source, schema, coverage, and licence
- [ ] Create reproducible local environment
- [ ] Establish repository structure

### Phase 2 — data understanding

- [ ] Audit columns and data types
- [ ] Check missing values and duplicates
- [ ] Inspect target distributions
- [ ] Review time coverage and category cardinality
- [ ] Document unusable and leakage-prone columns

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

## 8. Planned repository structure

```text
FlightRisk/
├── README.md
├── PROJECT_PLAN.md
├── notebooks/
├── src/
├── tests/
└── reports/
```

Directories will be added when they contain real work. Empty structure will not be committed only for appearance.

## 9. Progress and reporting rules

- Commit work incrementally with clear messages.
- Label unfinished work honestly.
- Do not publish performance numbers without a documented validation split.
- Record important assumptions and changes in the repository.
- Prefer a clear baseline and trustworthy evaluation over an unnecessarily complex model.

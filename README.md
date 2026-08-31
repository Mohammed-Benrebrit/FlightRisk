# FlightRisk

**FlightRisk** is an early-stage machine learning project that explores whether a flight's arrival delay can be estimated before departure using only information available at prediction time.

> **Current status:** Phase 1 — problem framing, data exploration, and local project setup. No trained-model results are being claimed yet.

## Project objectives

The project approaches flight-delay prediction in two complementary ways:

1. **Regression:** estimate arrival delay in minutes.
2. **Classification:** estimate the probability that a flight arrives at least 15 minutes late.

The intended prediction point is **before departure**. Features that become available only during or after a flight will not be used for training.

## Initial scope

Version 0.1 focuses on:

- Completed, non-diverted flights
- Schedule and calendar information
- Airline, origin, and destination
- Route distance
- A simple baseline before machine learning models
- Chronological train, validation, and test splits

Cancellations, diversions, weather, and historical aggregate features are possible later extensions rather than part of the initial version.

## Leakage policy

A feature is eligible only when it would genuinely be known at the intended prediction time. The project will document feature availability and reject post-departure information that could produce unrealistically strong validation results.

## Planned workflow

| Stage | Status |
|---|---|
| Problem definition and scope | Completed |
| Data exploration and environment setup | In progress |
| Data-quality checks and exploratory analysis | Planned |
| Trivial regression and classification baselines | Planned |
| Linear and logistic regression models | Planned |
| Reproducible preprocessing pipelines | Planned |
| Refactoring, tests, and documentation | Planned |
| Tree models, neural networks, API, and Docker | Future extensions |

## Repository contents

- `PROJECT_PLAN.md` — detailed learning and implementation roadmap derived from the original project plan
- `notebooks/` — exploratory work and experiments will be added as the project progresses
- `src/` — reusable project code will be introduced after the initial exploration
- `tests/` — automated tests will be added during refactoring

## Learning context

This project is being developed alongside an Artificial Intelligence bachelor's programme and structured machine-learning study.

- [Supervised Machine Learning: Regression and Classification — verified Coursera credential](https://coursera.org/verify/XDL7FP8XI4NM)

## Transparency

FlightRisk is intentionally published from the beginning so its progress can be followed through genuine, incremental commits. The repository currently documents the project design and roadmap; data exploration, baselines, models, and results will be added only when they have been implemented and verified.

# Phase 1 evidence and review notes

Reviewed for publication on 6 September 2026 against Kaggle notebook version 1 (ID `347804282`) and the supplied Phase 1 PDF.

## Preserved artifacts

- [Executed notebook](../notebooks/01_eda.ipynb): copied without modifying its code, Markdown or outputs. The older local download from 4 September was not used.
- [Original report](FlightRisk_Phase1_EDA_Report.pdf): preserved unchanged. Its repository-audit and recommended-update sections describe the state before this publication.

## Results directly supported by saved notebook outputs

- Sample shape: 10,000 rows and 35 original variables.
- Target groups: 6,092 early; 1,625 from zero to less than 15 minutes late; 2,119 at least 15 minutes late; 164 missing.
- All 164 missing-target rows fall into the displayed cancellation/diversion groups: 122 cancelled and 42 diverted.
- Therefore 9,836 targets are defined and the 15+ minute delay rate among them is 21.54% (2,119/9,836). The denominator is not all 10,000 flights.
- Arrival-delay quantiles: 90th = 47; 99th = 217; 99.5th = 310.075; maximum = 2,014 minutes.
- Saved grouped analyses cover carrier, scheduled departure hour, weekday, month, scheduled-duration buckets, origin and destination; distance is explored with a scatter plot.
- Reusable functions are present: `summarize_delay_by` and `ploting_summarize`.

## Scope clarifications

1. **Day of month:** the PDF discusses `day_of_month`, but the published notebook has no corresponding analysis cell or saved result. Do not highlight this as verified work until the analysis is added or the report is revised.
2. **Global mean and median:** the PDF reports +7.55 and -6 minutes. The published notebook does not print the global `describe()` result (that line is commented out). These remain report-level summaries rather than independently recomputed publication checks and are not used in the README's verified-results table.
3. **Feature strength:** phrases such as "strong candidate" in the report refer to exploratory hypotheses. They do not establish statistical significance, causal effects or out-of-sample predictive value.
4. **Time split:** the sample EDA precedes train/validation/test splitting. Chronological validation, train-only preprocessing and baseline comparisons are planned, not implemented.
5. **Data quality:** dictionary inspection and target-missingness analysis are complete for this milestone. A comprehensive duplicate/schema/coverage audit and full-dataset validation are not.
6. **Reproducibility:** the export records a completed Kaggle run and contains no error outputs. Its Python syntax was checked for publication. A new end-to-end local execution was not performed because the source CSVs and a validated local environment were not part of this update.

## Engineering follow-ups

- Make the grouped helper explicitly operate on a valid-target copy rather than mutating its input; preserve missing-label semantics with clear assertions.
- Validate scheduled HHMM ranges and any midnight representation before generalizing departure-hour extraction.
- Label plotting rates as observed fractions, not calibrated model probabilities, and include count context for sparse groups.
- Record a feature allowlist, raw input hashes, date coverage and dependency versions before the first baseline.

These are next-step improvements, not work claimed as already completed. No trained model, accuracy score or deployed system is implied by this milestone.

# Validation Strategy

## Facility Maintenance and Energy Decision Support Framework

### 1. Purpose

The purpose of the validation strategy is to define how the preliminary framework will be tested, challenged, and refined before any claim of practical facility applicability is made.

The current work is a proof of concept and has not been operationally validated.

### 2. Validation Principles

Validation will be treated as an iterative process rather than a single final test.

The project will distinguish among:

- verification that code executes as intended;
- internal model evaluation;
- sensitivity and robustness testing;
- external testing on additional datasets;
- comparison with historical or published outcomes where available; and
- facility specific validation if authorized operational data later becomes available.

No result from synthetic or simulated data will be represented as evidence of actual facility performance.

### 3. Data Validation Completed to Date

The AI4I source dataset has been evaluated for:

- dimensions and column structure;
- data types;
- missing values;
- duplicate observations;
- failure prevalence;
- individual failure mode frequency; and
- consistency between the overall machine failure target and individual failure mode indicators.

The analysis identified no missing values or duplicate rows.

It also identified a substantially imbalanced target, with machine failures representing 3.39% of observations.

A consistency review identified observations in which the general machine failure target and the individual failure mode indicators do not completely correspond. These observations were documented rather than silently removed or altered.

### 4. Failure Risk Model Evaluation Completed to Date

The current modeling stage compares a majority class baseline, logistic regression, decision tree, and random forest.

A stratified 80/20 training and test split is used so that failure prevalence remains similar in both datasets.

Evaluation does not rely on ordinary accuracy alone because of the low failure prevalence.

Current evaluation measures include:

- balanced accuracy;
- precision;
- recall;
- F1 score;
- ROC-AUC;
- average precision;
- confusion matrices; and
- precision-recall behavior.

Five fold stratified cross validation is also performed on the training data.

The individual failure mode indicators are excluded from predictors to reduce the risk of target leakage.

### 5. Current Model Findings

The baseline experiments demonstrate meaningful differences among model behaviors.

The majority class dummy classifier achieves high ordinary accuracy while detecting no failures, confirming that ordinary accuracy alone is unsuitable for this dataset.

The decision tree currently provides substantially greater failure recall than the random forest at the tested default classification settings.

The random forest currently provides stronger ROC-AUC, average precision, precision, and F1 performance, but lower recall at the default 0.50 classification threshold.

The current project therefore does not designate a universally superior final model.

Additional work is required on threshold selection, probability calibration, external testing, and comparison across additional datasets.

### 6. Prioritization and Allocation Testing Completed to Date

The proof of concept has demonstrated that estimated failure probabilities can be combined with simulated operational consequence variables to generate maintenance risk scores.

A constrained optimization model has also been executed under simulated maintenance budget, labor, and spare parts constraints.

The optimization has been compared with a simple risk ranking allocation.

These results verify that the proposed computational sequence can be implemented. They do not validate the simulated consequence values, resource limits, or maintenance recommendations for a real facility.

### 7. Sensitivity Analysis Completed to Date

Sensitivity testing has examined alternative consequence weights, maintenance budgets, labor availability, and combined resource scenarios.

Across five tested criticality and downtime weighting combinations, the highestranked portion of the current portfolio remained relatively stable. Nine assets appeared within the top ten under every tested weighting scenario, and the largest observed rank range was four positions.

Budget sensitivity showed that, under the current 240 hour labor constraint, increasing budget beyond a certain point did not continue to increase the total modeled risk addressed.

Labor sensitivity showed continuing gains over a wider range, followed by diminishing improvement at higher labor levels.

Combined resource testing confirmed that the limiting constraint can change depending on the available combination of budget and labor.

### 8. Next Validation Stages

Future validation will include testing on additional publicly available equipment datasets where suitable variables and outcome information are available.

The failure risk component will also require:

- probability calibration analysis;
- decision-threshold evaluation;
- comparison with additional model classes where justified;
- testing under alternative training and validation partitions;
- investigation of performance across equipment or operating subgroups; and
- assessment of stability across datasets.

The maintenance prioritization component will require:

- alternative asset portfolios;
- alternative consequence structures;
- alternative criticality and downtime weights;
- testing of priority thresholds;
- evaluation of maintenance cost and labor assumptions;
- additional resource allocation formulations; and
- comparison against historical maintenance outcomes where reliable records are available.

### 9. Facility Level Validation

If authorized facility data becomes available, the framework should be evaluated against historical equipment failures, inspection records, maintenance histories, downtime records, available cost information, and other relevant operating evidence.

Any such validation would require documentation of data provenance, data quality, facility context, assumptions, and limitations.

Facility specific implementation would also require appropriate engineering and operational review.

### 10. Energy System Validation

The energy system workstream has not yet been implemented.

When developed, validation is expected to include comparison with published benchmarks, historical load or equipment information where available, scenario consistency checks, sensitivity testing, and comparison against established engineering calculations or simulation results.

### 11. Reproducibility

The current project uses a public repository, documented source data, saved notebooks, saved result files, fixed random states where applicable, and explicit separation between source data and simulated demonstration inputs.

These practices are intended to make the current proof of concept reviewable and reproducible.

### 12. Current Validation Status

The existing work supports only the conclusion that a preliminary computational proof of concept has been implemented and tested internally.

It does not establish operational validity, safety suitability, regulatory compliance, or effectiveness at an actual industrial facility.

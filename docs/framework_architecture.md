# Framework Architecture

## Facility Maintenance and Energy Decision-Support Framework

### 1. Purpose

This document describes the current architecture of the Facility Maintenance and Energy Decision Support Framework.

The broader project is intended to develop a computational decision-support framework that connects equipment condition, maintenance prioritization, resource constraints, and facility energy system performance.

The current implementation is a preliminary proof of concept for the maintenance workstream. The facility energy system workstream has not yet been implemented.

### 2. Current Maintenance Architecture

The implemented maintenance proof of concept follows the computational sequence:

**equipment operating data → failure-risk estimation → consequence assessment → maintenance-risk scoring → maintenance prioritization → constrained resource allocation → sensitivity analysis**

Each stage is maintained separately so that assumptions, data sources, intermediate results, and limitations can be reviewed independently.

### 3. Data Layer

The current failure risk component uses the AI4I 2020 Predictive Maintenance Dataset.

The source dataset contains 10,000 observations and includes product type, air temperature, process temperature, rotational speed, torque, tool wear, a general machine failure indicator, and five individual failure mode indicators.

The dataset is synthetic and is not represented as operating data from an actual industrial facility.

Facility context variables required for the maintenance prioritization demonstration are not contained in AI4I. Operational criticality, expected downtime, maintenance cost, labor requirements, and spare parts availability are therefore stored separately as explicitly simulated demonstration variables.

This separation is intended to distinguish source data from project generated assumptions.

### 4. Data Evaluation Layer

The first analytical stage evaluates the structure and quality of the source data.

The implemented checks include:

- dataset dimensions and column structure;
- variable types;
- missing values;
- duplicate observations;
- failure prevalence;
- individual failure mode frequency;
- consistency between the general machine failure variable and individual failure mode indicators; and
- descriptive comparison of operating variables between failure and non failure observations.

The current dataset contains no missing values or duplicate rows. Machine failures represent 3.39% of observations, creating a substantially imbalanced classification problem.

### 5. Failure-Risk Estimation Layer

The preliminary failure-risk component uses the following predictors:

- product type;
- air temperature;
- process temperature;
- rotational speed;
- torque; and
- tool wear.

The general `Machine failure` variable is used as the prediction target.

The individual failure mode indicators are excluded from model inputs because they directly describe failure events and could introduce target leakage. `UDI` and `Product ID` are also excluded from prediction.

The current implementation compares:

- a majority-class dummy baseline;
- logistic regression;
- decision tree classification; and
- random forest classification.

Model evaluation includes accuracy, balanced accuracy, precision, recall, F1 score, ROC-AUC, average precision, confusion matrices, precision-recall analysis, and five-fold stratified cross-validation.

The random forest currently supplies continuous estimated failure probabilities to the maintenance prioritization demonstration. This does not establish it as the final model.

### 6. Consequence Assessment Layer

Failure probability alone is not treated as maintenance priority.

The present demonstration combines estimated failure probability with two simulated consequence factors:

- operational criticality; and
- expected downtime.

Both consequence variables are normalized before being combined.

The initial consequence index uses a demonstration weighting of 60% operational criticality and 40% expected downtime.

These weights are not presented as engineering standards or validated facility-specific values.

### 7. Maintenance-Risk Scoring Layer

The preliminary maintenance-risk calculation is:

**Maintenance Risk = Estimated Failure Probability × Consequence Index**

The resulting continuous value is converted to a 0-100 scale for presentation and ranking.

Illustrative priority categories are also generated for the proof of concept. These categories are not operational maintenance thresholds.

### 8. Resource-Allocation Layer

The framework then considers whether candidate maintenance actions can be performed under resource constraints.

The current optimization demonstration includes:

- maintenance cost;
- labor hours;
- spare-parts availability;
- total maintenance budget; and
- total available maintenance labor.

A binary mathematical optimization model selects candidate maintenance actions that maximize the total modeled maintenance-risk score addressed while satisfying the resource constraints.

The optimization is implemented using PuLP and the CBC solver.

A simple risk ranking allocation is also calculated for comparison.

### 9. Sensitivity Analysis Layer

The current implementation examines sensitivity to:

- alternative criticality and downtime weights;
- maintenance-budget limits;
- maintenance-labor limits;
- combined budget and labor constraints; and
- demonstration portfolio composition.

The sensitivity analysis is intended to identify assumptions that materially influence rankings and allocation decisions rather than to validate any particular operational parameter.

### 10. Current Implemented Flow

The current maintenance workstream can be summarized as:

**AI4I operating variables**

↓

**data-quality and exploratory analysis**

↓

**baseline failure-risk models**

↓

**estimated failure probability**

↓

**simulated operational consequence**

↓

**maintenance-risk score**

↓

**priority ranking**

↓

**budget, labor, and parts constraints**

↓

**resource allocation optimization**

↓

**sensitivity analysis**

### 11. Planned Energy-System Workstream

The broader framework is intended to include a separate facility energy system performance and reliability workstream.

Planned areas include representations of facility loads, grid supply, backup generation, renewable energy resources, storage, equipment outages, and disruption scenarios.

That workstream has not yet been implemented and no current repository result should be interpreted as an energy-system model.

### 12. Integration Objective

The longer term architecture is intended to connect the maintenance and energy workstreams so that equipment condition and maintenance timing can eventually be evaluated together with facility energy continuity and operational reliability.

The present repository represents an early development stage toward that broader integrated framework.

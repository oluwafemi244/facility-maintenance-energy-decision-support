# Preliminary Technical Report

## Development of a Maintenance Risk and Resource Allocation Component for a Facility Decision Support Framework

**Version:** 0.1
**Date:** August 22, 2026
**Status:** Preliminary research and development report

## Executive Summary

This report documents the first implemented maintenance component of a broader Facility Maintenance and Energy Decision Support Framework.

The purpose of the work is to develop a transparent computational process that can connect equipment condition information with failure risk estimation, maintenance priority, and limited maintenance resources. The present work focuses only on the maintenance component. The facility energy system component remains a later development stage.

The proof of concept was developed using the AI4I 2020 Predictive Maintenance Dataset from the UCI Machine Learning Repository. The dataset is synthetic and contains 10,000 observations. It was selected because it provides equipment operating and condition variables together with a machine failure outcome that can be used to test the initial modeling process.

The work completed so far includes data quality review, exploratory analysis, comparison of several failure risk models, generation of estimated failure probabilities, creation of a maintenance risk score, maintenance ranking, constrained resource allocation, and sensitivity testing.

The source dataset contains a substantial class imbalance. Only 339 of the 10,000 observations are labeled as machine failures, representing a failure rate of 3.39 percent. This made ordinary classification accuracy an unsuitable measure by itself.

Four classification approaches were evaluated: a majority class dummy model, logistic regression, a decision tree, and a random forest. The random forest produced the strongest ROC AUC, average precision, precision, and F1 score on the held out test data, while the decision tree produced substantially higher failure recall. The results therefore show a real tradeoff between identifying more failures and limiting false alerts.

For the maintenance prioritization demonstration, estimated failure probability was combined with simulated operational criticality and expected downtime. The resulting maintenance risk scores were then evaluated under simulated maintenance budget, labor, and spare parts constraints.

Under a demonstration budget of $120,000 and 240 available labor hours, the optimization model selected 11 maintenance actions. These actions used $83,656 of the simulated budget and 236 labor hours and addressed a total modeled maintenance risk score of approximately 107.77.

A simple risk ranking method selected 12 actions, used $95,572 and all 240 labor hours, while addressing the same total modeled risk score. This result does not establish that mathematical optimization is generally superior to ranking. It shows that a constrained allocation model can be implemented and compared directly with a simpler allocation approach.

Sensitivity analysis showed that the highest priority assets in the present portfolio were relatively stable when the weighting assigned to criticality and downtime was changed. Resource sensitivity also showed that additional budget or labor does not always improve the selected maintenance program once another constraint becomes limiting.

The present work is a proof of concept. The source equipment dataset is synthetic, the facility context variables are simulated, and the results have not been validated for operational use at an actual facility. The current result is a reproducible computational process that has been implemented and tested and can be expanded and evaluated with additional data.

## 1. Purpose and Scope

The broader project is intended to develop a computational decision support framework for infrastructure and energy intensive industrial facilities.

The planned framework has two main workstreams. The first concerns equipment condition, maintenance prioritization, and maintenance resource allocation. The second will examine facility energy system performance and reliability under different operating and disruption scenarios.

This report covers only the first workstream.

The current maintenance component addresses the following sequence:

**equipment operating information → failure risk estimation → consequence assessment → maintenance risk → priority ranking → resource allocation → sensitivity analysis**

The work is not intended to replace equipment inspection, manufacturer requirements, engineering judgment, facility maintenance procedures, safety requirements, or decisions by qualified personnel.

The present goal is narrower. It is to determine whether the planned analytical sequence can be implemented in a transparent and reproducible form and to identify the technical issues that must be addressed before later validation.

## 2. Development Approach

Development was divided into several stages.

The first stage established a public project repository, documented the planned framework, identified the initial source dataset, and created a data dictionary.

The second stage examined the source dataset for structure, missing values, duplicates, failure frequency, failure mode frequency, and inconsistencies between the general machine failure indicator and the individual failure mode variables.

The third stage developed baseline failure risk models and compared their behavior using several performance measures.

The fourth stage connected estimated failure probability with simulated operational consequence variables to create a preliminary maintenance risk score.

The fifth stage implemented a constrained resource allocation model using simulated maintenance cost, labor requirements, and spare parts availability.

The sixth stage tested the sensitivity of the results to different consequence weights, maintenance budgets, labor limits, and combined resource conditions.

Each stage is retained separately in the project repository so that the assumptions, code, outputs, and limitations can be reviewed.

## 3. Source Data

### 3.1 AI4I 2020 Predictive Maintenance Dataset

The present proof of concept uses the AI4I 2020 Predictive Maintenance Dataset published through the UCI Machine Learning Repository.

The dataset contains 10,000 observations and 14 variables.

The variables include:

1. UDI
2. Product ID
3. Type
4. Air Temperature
5. Process Temperature
6. Rotational Speed
7. Torque
8. Tool Wear
9. Machine Failure
10. Tool Wear Failure
11. Heat Dissipation Failure
12. Power Failure
13. Overstrain Failure
14. Random Failure

The dataset is synthetic. It is not represented as equipment data from an actual industrial facility.

### 3.2 Data Quality Review

The exploratory analysis found no missing values and no duplicate rows in the dataset.

Machine failures occurred in 339 of the 10,000 observations. This corresponds to a failure rate of 3.39 percent.

The individual failure mode counts were:

| Failure Mode             | Count |
| ------------------------ | ----: |
| Heat Dissipation Failure |   115 |
| Overstrain Failure       |    98 |
| Power Failure            |    95 |
| Tool Wear Failure        |    46 |
| Random Failure           |    19 |

A consistency check also identified 9 observations where `Machine failure` was equal to 1 but none of the five listed failure mode indicators was present.

The analysis identified another 18 observations where at least one individual failure mode indicator was present while `Machine failure` remained equal to 0.

These records were retained. The project does not silently remove or alter source records simply because the target variables do not correspond perfectly.

## 4. Failure Risk Modeling

### 4.1 Predictor Selection

The preliminary failure risk models use the following variables:

| Predictor           | Role                       |
| ------------------- | -------------------------- |
| Product Type        | Equipment category         |
| Air Temperature     | Operating variable         |
| Process Temperature | Operating variable         |
| Rotational Speed    | Operating variable         |
| Torque              | Operating variable         |
| Tool Wear           | Condition related variable |

`Machine failure` is used as the prediction target.

The five individual failure mode variables are excluded from the predictor set because they directly describe failure events and could introduce target leakage.

`UDI` and `Product ID` are also excluded because they primarily identify records rather than describe equipment operating condition.

### 4.2 Training and Test Data

The data was divided into training and test sets using an 80 percent training and 20 percent test split.

The split was stratified using the machine failure target so that the low failure prevalence remained approximately consistent in both sets.

The resulting training set contains 8,000 observations and the test set contains 2,000 observations.

The training failure rate was approximately 3.39 percent and the test failure rate was approximately 3.40 percent.

### 4.3 Models Evaluated

Four models were evaluated:

1. Majority class dummy classifier
2. Logistic regression
3. Decision tree classifier
4. Random forest classifier

The dummy classifier serves as an important reference because a model that predicts no failure for every observation can achieve high ordinary accuracy when failures are uncommon.

### 4.4 Test Set Results

The test set results were:

| Model               | Accuracy | Balanced Accuracy | Precision | Recall | F1 Score | ROC AUC | Average Precision |
| ------------------- | -------: | ----------------: | --------: | -----: | -------: | ------: | ----------------: |
| Dummy Baseline      |   0.9660 |            0.5000 |    0.0000 | 0.0000 |   0.0000 |  0.5000 |            0.0340 |
| Logistic Regression |   0.8245 |            0.8240 |    0.1418 | 0.8235 |   0.2419 |  0.9070 |            0.3818 |
| Decision Tree       |   0.9290 |            0.9065 |    0.3093 | 0.8824 |   0.4580 |  0.9111 |            0.5901 |
| Random Forest       |   0.9800 |            0.7272 |    0.9118 | 0.4559 |   0.6078 |  0.9636 |            0.7762 |

The dummy classifier achieved 96.60 percent ordinary accuracy while identifying no failures. This illustrates why ordinary accuracy is not sufficient for this problem.

The logistic regression detected approximately 82.35 percent of actual failures but had low precision.

The decision tree produced the highest test recall among the substantive models at approximately 88.24 percent. Its precision was approximately 30.93 percent, showing that the greater failure detection rate came with more false alerts.

The random forest produced the strongest ROC AUC, average precision, precision, and F1 score. Its precision was approximately 91.18 percent, but its recall at the default classification threshold was approximately 45.59 percent.

There is therefore no basis at this stage for describing one model as superior under every relevant measure.

### 4.5 Cross Validation

Five fold stratified cross validation was also performed on the training data.

The average results were:

| Model               | Balanced Accuracy | Precision | Recall |     F1 | ROC AUC | Average Precision |
| ------------------- | ----------------: | --------: | -----: | -----: | ------: | ----------------: |
| Logistic Regression |            0.8138 |    0.1374 | 0.8044 | 0.2346 |  0.8965 |            0.4409 |
| Decision Tree       |            0.9165 |    0.2990 | 0.9078 | 0.4495 |  0.9180 |            0.5694 |
| Random Forest       |            0.6912 |    0.8973 | 0.3839 | 0.5366 |  0.9669 |            0.7546 |

The cross validation results show the same general tradeoff seen in the held out test set.

The decision tree produced higher failure recall, while the random forest produced substantially higher precision and stronger ranking and discrimination measures.

### 4.6 Random Forest Variable Importance

The random forest variable importance values were:

| Variable            | Importance |
| ------------------- | ---------: |
| Rotational Speed    |     0.3022 |
| Torque              |     0.3002 |
| Tool Wear           |     0.1995 |
| Air Temperature     |     0.1040 |
| Process Temperature |     0.0746 |

The remaining importance was distributed across the encoded product type categories.

These values describe the behavior of the fitted random forest. They do not establish causal relationships between the variables and equipment failure.

### 4.7 Current Probability Model

The random forest probabilities were used as the preliminary failure likelihood input for the maintenance prioritization demonstration.

This choice was based on its stronger ROC AUC and average precision in the current experiments.

It does not mean that the random forest has been selected as the final operational model.

Probability calibration, threshold analysis, additional datasets, and further validation remain necessary.

## 5. Maintenance Consequence and Priority

### 5.1 Need for a Consequence Layer

Failure probability alone does not determine maintenance priority.

Equipment with similar failure probabilities may have very different operational consequences. A lower probability failure may also deserve greater attention where the equipment performs a more important function or where failure would produce substantial downtime.

For this reason, the current framework separates failure likelihood from failure consequence.

### 5.2 Simulated Facility Context

The AI4I dataset does not contain:

1. operational criticality;
2. expected downtime;
3. maintenance cost;
4. labor requirements;
5. spare parts availability; or
6. maintenance budget.

These variables were therefore created separately as simulated demonstration values.

They are not presented as measurements or records from an actual facility.

A fixed random seed was used so the generated values can be reproduced.

### 5.3 Consequence Index

The preliminary consequence index uses two factors:

**Operational Criticality**

and

**Expected Downtime**

The original demonstration assigns 60 percent of the consequence weight to operational criticality and 40 percent to expected downtime.

Both factors are normalized before they are combined.

These weights are assumptions used to demonstrate the calculation. They are not industry standards and have not been validated for a specific facility.

### 5.4 Maintenance Risk Score

The preliminary maintenance risk calculation is:

**Maintenance Risk = Estimated Failure Probability × Consequence Index**

The result is also converted to a scale from 0 to 100 for presentation.

The current demonstration uses illustrative priority categories. These categories are not maintenance standards or operational action thresholds.

## 6. Resource Allocation Demonstration

### 6.1 Demonstration Constraints

The initial allocation exercise uses the following simulated constraints:

| Resource           |                             Demonstration Limit |
| ------------------ | ----------------------------------------------: |
| Maintenance Budget |                                        $120,000 |
| Maintenance Labor  |                                       240 hours |
| Spare Parts        | Action permitted only where parts are available |

The purpose of these constraints is to test whether candidate maintenance actions can be selected when every action cannot be performed.

### 6.2 Simple Ranking

The first allocation method processes candidate actions in order of maintenance risk score and selects actions while sufficient budget and labor remain.

The simple ranking method selected 12 actions.

The selected actions used:

**$95,572 in simulated maintenance cost**

and

**240 labor hours**

The total modeled maintenance risk score addressed was approximately:

**107.77**

### 6.3 Constrained Optimization

A binary mathematical optimization model was then developed.

The model maximizes the total maintenance risk score associated with selected actions subject to:

1. the maintenance budget;
2. available labor hours; and
3. spare parts availability.

The optimization model reached an optimal solver status.

It selected 11 maintenance actions.

The selected actions used:

**$83,656 in simulated maintenance cost**

and

**236 labor hours**

The total modeled maintenance risk score addressed was approximately:

**107.77**

### 6.4 Comparison

The comparison was:

| Method                   | Actions Selected | Total Cost | Labor Used | Risk Score Addressed |
| ------------------------ | ---------------: | ---------: | ---------: | -------------------: |
| Simple Risk Ranking      |               12 |    $95,572 |        240 |               107.77 |
| Constrained Optimization |               11 |    $83,656 |        236 |               107.77 |

Both approaches addressed the same total modeled risk score in the present demonstration.

The optimization solution happened to use fewer actions, lower cost, and slightly less labor. However, the current optimization objective only maximizes risk addressed. It does not explicitly minimize cost after maximizing risk.

The result should therefore not be interpreted as proof that optimization is generally more efficient than simple ranking.

The important result at this stage is that the framework can take maintenance risk estimates and explicitly evaluate them under resource constraints.

## 7. Sensitivity Analysis

### 7.1 Consequence Weight Sensitivity

Five consequence weighting combinations were tested:

| Criticality Weight | Downtime Weight |
| -----------------: | --------------: |
|               0.20 |            0.80 |
|               0.40 |            0.60 |
|               0.50 |            0.50 |
|               0.60 |            0.40 |
|               0.80 |            0.20 |

Nine assets remained within the top ten under all five weighting combinations.

The maximum observed rank movement across the tested weighting scenarios was four positions.

For the current portfolio, this indicates that the highest ranked assets were reasonably stable when the relative importance assigned to criticality and downtime was changed.

This finding applies only to the current demonstration portfolio and does not establish that the ranking method will be equally stable in other datasets or facilities.

### 7.2 Budget Sensitivity

Labor availability was fixed at 240 hours while the maintenance budget was varied.

|   Budget | Actions Selected | Risk Score Addressed |
| -------: | ---------------: | -------------------: |
|  $40,000 |                6 |               104.09 |
|  $60,000 |                8 |               106.51 |
|  $80,000 |               11 |               107.56 |
| $100,000 |               11 |               107.77 |
| $120,000 |               11 |               107.77 |
| $150,000 |               11 |               107.77 |
| $180,000 |               11 |               107.77 |

Increasing the budget improved the modeled result at lower resource levels.

Once the budget reached $100,000, additional budget did not improve the result under the same labor and spare parts conditions.

This shows that budget was no longer the limiting resource in those scenarios.

### 7.3 Labor Sensitivity

The maintenance budget was fixed at $120,000 while available labor was varied.

| Available Labor | Actions Selected | Risk Score Addressed |
| --------------: | ---------------: | -------------------: |
|        80 hours |                3 |                96.65 |
|       120 hours |                6 |               104.09 |
|       160 hours |                8 |               105.86 |
|       200 hours |                9 |               106.82 |
|       240 hours |               11 |               107.77 |
|       300 hours |               14 |               108.60 |
|       360 hours |               15 |               108.74 |

Additional labor increased the total modeled risk addressed over a wider range than additional budget.

The improvement became small between 300 and 360 available hours, which indicates diminishing gains within the present portfolio.

### 7.4 Combined Resource Conditions

Budget and labor were also varied together.

The combined scenarios show that the limiting resource can change.

For example, when only 120 labor hours were available, increasing the budget did not improve the modeled result beyond what could be completed within the labor limit.

Likewise, at a $60,000 budget, increasing labor eventually stopped improving the result because the budget became limiting.

This supports treating maintenance planning as a combined resource allocation problem rather than evaluating equipment risk, budget, and labor separately.

### 7.5 Portfolio Composition

The current demonstration portfolio contains 60 assets.

Its preliminary priority composition is:

| Priority Category | Assets | Percentage |
| ----------------- | -----: | ---------: |
| Low               |     57 |     95.00% |
| Moderate          |      1 |      1.67% |
| High              |      2 |      3.33% |

No asset falls within the current illustrative Critical category.

This is an important limitation.

The portfolio demonstrates that the computational process executes, but it is heavily concentrated in low modeled risk observations and should not be treated as representative of the risk distribution of an actual facility.

## 8. Current Limitations

Several limitations restrict what can presently be concluded from this work.

First, the AI4I equipment dataset is synthetic. Model performance on this dataset does not establish performance on actual industrial equipment.

Second, the facility context variables used in the maintenance prioritization and allocation stages are simulated. Operational criticality, downtime, maintenance cost, labor requirements, spare parts availability, and resource limits were not obtained from an operating facility.

Third, the current consequence weights and priority thresholds are demonstration assumptions.

Fourth, the current failure models use a limited set of operating variables. Actual maintenance decisions may depend on inspection results, equipment age, maintenance history, vibration measurements, lubrication condition, redundancy, safety consequences, production requirements, and equipment specific engineering limits that are not represented here.

Fifth, no model has yet been established as the final failure risk model. The tested models show different strengths and weaknesses.

Sixth, the current optimization model does not account for technician skill categories, scheduling across time, maintenance sequencing, equipment dependencies, shutdown windows, production losses, uncertainty in maintenance cost, uncertainty in labor requirements, or interactions among equipment outages.

Seventh, the current portfolio contains mostly low priority observations.

Finally, the facility energy system component of the broader framework has not yet been implemented.

## 9. Reproducibility

The project is maintained in a public repository.

The repository contains:

1. the source AI4I dataset;
2. source and data documentation;
3. a data dictionary;
4. exploratory analysis code and saved outputs;
5. baseline failure risk modeling code;
6. model performance results;
7. failure probability outputs;
8. simulated facility context data stored separately from the source dataset;
9. maintenance priority results;
10. resource allocation results;
11. sensitivity analysis results;
12. framework architecture documentation;
13. validation strategy documentation; and
14. limitations documentation.

Fixed random states are used where applicable.

The distinction between source data and simulated project inputs is documented throughout the project.

This structure is intended to allow another reviewer to follow the current development process and reproduce the main calculations.

## 10. Current Development Status

The work completed so far establishes a functioning preliminary maintenance component.

The following sequence has been implemented:

**source data**

↓

**data review**

↓

**failure risk modeling**

↓

**failure probability**

↓

**consequence assessment**

↓

**maintenance risk scoring**

↓

**maintenance ranking**

↓

**constrained resource allocation**

↓

**sensitivity analysis**

This is not a completed operational system.

The current repository documents that development of the proposed framework has begun and that several core analytical components have been implemented and tested.

## 11. Next Development Stages

The next maintenance development work will focus on broader validation and refinement.

This will include testing additional public equipment datasets where suitable data is available, examining probability calibration, evaluating classification thresholds, testing alternative asset portfolios, and expanding the resource allocation formulation.

The current optimization approach can also be extended to evaluate additional constraints such as scheduling, maintenance windows, technician availability, and equipment dependencies.

A later project stage will begin the facility energy system workstream. That work will examine facility energy demand, supply resources, backup generation, storage, equipment availability, and disruption scenarios.

The longer term objective is to connect maintenance condition and timing with facility energy continuity in one documented decision support framework.

## 12. Conclusion

This report documents an initial implemented maintenance component of the planned Facility Maintenance and Energy Decision Support Framework.

The work demonstrates that equipment operating data can be processed through a reproducible sequence that estimates failure risk, adds operational consequence, produces maintenance priorities, and evaluates candidate actions under resource constraints.

The work also demonstrates why model accuracy, failure probability, consequence, and available resources cannot be considered independently.

The current results should be interpreted within their limitations. The source dataset is synthetic, the facility context is simulated, and the system has not been validated for operational use.

The value of the current stage is that the project has moved beyond a written concept. A working computational process, documented assumptions, saved outputs, sensitivity tests, and validation plan now exist and provide a concrete basis for continued development.

## References

AI4I 2020 Predictive Maintenance Dataset. (2020). UCI Machine Learning Repository. DOI: 10.24432/C5HS5C.

UCI Machine Learning Repository. AI4I 2020 Predictive Maintenance Dataset.
https://archive.ics.uci.edu/dataset/601/ai4i

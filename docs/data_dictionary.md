# Data Dictionary

## AI4I 2020 Predictive Maintenance Dataset

This document describes the variables used during the preliminary maintenance risk development stage of the Facility Maintenance and Energy Decision Support Framework.

The source dataset is the AI4I 2020 Predictive Maintenance Dataset published by the UCI Machine Learning Repository.

Official source:
https://archive.ics.uci.edu/dataset/601/ai4i

DOI:
https://doi.org/10.24432/C5HS5C

The dataset contains 10,000 observations and is synthetic. It is intended to reflect characteristics of industrial predictive-maintenance data.

## Original Variables

| Variable                | Type           | Unit    | Role                   | Description                                                                                                                    |
| ----------------------- | -------------- | ------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| UID                     | Integer        | None    | Identifier             | Unique observation identifier ranging from 1 to 10,000.                                                                        |
| Product ID              | Categorical    | None    | Identifier             | Product identifier containing the product quality class and a serial identifier.                                               |
| Type                    | Categorical    | None    | Input feature          | Product quality category. The dataset uses L, M, and H to represent low, medium, and high quality variants.                    |
| Air temperature [K]     | Continuous     | Kelvin  | Input feature          | Simulated ambient air temperature associated with the observation.                                                             |
| Process temperature [K] | Continuous     | Kelvin  | Input feature          | Simulated process temperature associated with the observation.                                                                 |
| Rotational speed [rpm]  | Integer        | rpm     | Input feature          | Rotational speed of the machine during the observation.                                                                        |
| Torque [Nm]             | Continuous     | Nm      | Input feature          | Applied torque during the observation.                                                                                         |
| Tool wear [min]         | Integer        | minutes | Input feature          | Accumulated tool wear time.                                                                                                    |
| Machine failure         | Binary integer | None    | Primary target         | Indicates whether a machine failure occurred for the observation. A value of 1 represents failure and 0 represents no failure. |
| TWF                     | Binary integer | None    | Failure mode indicator | Tool wear failure indicator.                                                                                                   |
| HDF                     | Binary integer | None    | Failure mode indicator | Heat dissipation failure indicator.                                                                                            |
| PWF                     | Binary integer | None    | Failure mode indicator | Power failure indicator.                                                                                                       |
| OSF                     | Binary integer | None    | Failure mode indicator | Overstrain failure indicator.                                                                                                  |
| RNF                     | Binary integer | None    | Failure mode indicator | Random failure indicator.                                                                                                      |

## Variables Used in the Preliminary Failure-Risk Model

The first proof-of-concept model will use the following variables as candidate predictors:

* Type
* Air temperature [K]
* Process temperature [K]
* Rotational speed [rpm]
* Torque [Nm]
* Tool wear [min]

The primary model outcome will be:

* Machine failure

The individual failure mode indicators will initially be retained for descriptive and diagnostic analysis but will not be used as predictors of `Machine failure`.

This distinction is important because the failure mode variables directly indicate whether particular failure conditions occurred. Using them as predictors of the overall failure label would introduce target leakage and would produce a misleading evaluation of model performance.

## Identifier Variables

`UID` and `Product ID` will not initially be used as numerical predictors of machine failure.

`UID` is an observation identifier and does not represent an operating condition.

`Product ID` contains product-identification information. The separate `Type` field already captures the product-quality category that may have analytical relevance.

The identifiers will be retained so that model outputs and individual observations can be traced during testing.

## Failure-Mode Indicators

The dataset contains the following failure-mode indicators:

* TWF: Tool Wear Failure
* HDF: Heat Dissipation Failure
* PWF: Power Failure
* OSF: Overstrain Failure
* RNF: Random Failure

These variables will be examined to understand how different failure conditions are distributed within the dataset.

They will not initially be included as predictor variables in the general failure risk model because they are directly related to the definition of the `Machine failure` target.

## Preliminary Analytical Roles

For the first development stage, the variables will be grouped as follows.

### Equipment and Product Information

* UID
* Product ID
* Type

### Operating and Condition Variables

* Air temperature [K]
* Process temperature [K]
* Rotational speed [rpm]
* Torque [Nm]
* Tool wear [min]

### Primary Outcome

* Machine failure

### Diagnostic Failure Modes

* TWF
* HDF
* PWF
* OSF
* RNF

## Data Quality Checks to Be Performed

Before model development, the dataset will be checked for:

1. total number of observations;
2. exact column names;
3. data types;
4. missing values;
5. duplicate observations;
6. invalid or unexpected category values;
7. minimum and maximum values;
8. distribution of the `Type` variable;
9. frequency of machine failures;
10. frequency of each individual failure mode; and
11. consistency between the general failure label and the individual failure-mode indicators.

The results of these checks will be documented in the exploratory analysis stage.

## Variables Not Present in the Source Dataset

The AI4I dataset does not provide several variables required for the complete maintenance prioritization and resource allocation framework.

These include:

* maintenance cost;
* replacement cost;
* expected downtime;
* safety consequence;
* operational criticality;
* available maintenance budget;
* maintenance labor requirements;
* spare-part availability;
* shutdown-window restrictions; and
* facility-level energy consequences.

No values for these variables will be attributed to the UCI dataset.

If such variables are needed for a later proof of concept demonstration, they will be created in a separate dataset and explicitly identified as simulated or hypothetical values.

## Current Limitation

This data dictionary describes the first public dataset selected for preliminary framework development. It does not define the final data requirements of the complete maintenance and energy-system framework.

Additional variables and datasets will be incorporated and documented as development progresses.

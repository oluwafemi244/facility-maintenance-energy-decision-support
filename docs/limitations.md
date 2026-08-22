# Current Limitations

## Facility Maintenance and Energy Decision Support Framework

### 1. Development Status

The repository contains a preliminary proof of concept for the maintenance workstream.

It is not a completed facility decision support system, and the facility energy system workstream has not yet been implemented.

### 2. Synthetic Source Dataset

The current failure risk analysis uses the AI4I 2020 Predictive Maintenance Dataset.

AI4I is synthetic. It does not represent operating records from a specific U.S. industrial or infrastructure facility.

Performance achieved on this dataset cannot therefore be assumed to represent performance on actual industrial equipment.

### 3. Limited Equipment Variables

The current failure risk models use product type, air temperature, process temperature, rotational speed, torque, and tool wear.

Actual facility maintenance decisions may depend on substantially more information, including inspection findings, equipment age, maintenance history, vibration measurements, lubrication condition, environmental conditions, safety consequences, redundancy, production requirements, and equipment specific engineering limits.

These variables are not represented in the current failure risk model.

### 4. Target Imbalance

Only 3.39% of AI4I observations are labeled as machine failures.

This substantial class imbalance means that ordinary classification accuracy can be misleading.

The project therefore reports additional metrics, but imbalance remains an important limitation when interpreting model results.

### 5. Source Target Consistency

The exploratory analysis identified records where the general machine failure indicator does not completely correspond with the five individual failure-mode indicators.

Those observations have been retained and documented.

The current proof of concept does not attempt to redefine or correct the source dataset's target labels.

### 6. Model Selection

No current model is established as the final failure risk model.

The random forest is presently used to generate demonstration failure probabilities because of its relatively strong discrimination and ranking performance in the baseline analysis.

At the default classification threshold, however, its failure recall is substantially lower than that of the tested decision tree.

Probability calibration and operational threshold selection have not yet been completed.

### 7. Simulated Facility Context

AI4I does not contain operational criticality, expected downtime, maintenance cost, labor requirements, spare parts availability, or facility budgets.

These variables were generated as explicitly simulated demonstration values.

They are not measurements, estimates, or records from an actual facility.

### 8. Consequence Weights

The current consequence index assigns 60% weight to operational criticality and 40% to expected downtime.

These values are demonstration assumptions.

They are not engineering standards, regulatory requirements, or validated facility specific weights.

### 9. Priority Categories

The current Low, Moderate, High, and Critical maintenance priority categories use illustrative proof of concept thresholds.

The thresholds have not been validated against maintenance outcomes or accepted industry criteria.

They should not be used to trigger actual maintenance actions.

### 10. Demonstration Portfolio Composition

The current 60 asset demonstration portfolio is heavily concentrated in the Low priority category.

Fifty seven assets, or 95% of the portfolio, are currently classified as Low priority. Two assets are High priority and one is Moderate priority.

This composition limits the conclusions that can be drawn from the present resource allocation comparison.

The portfolio is useful for testing computational execution but is not claimed to represent the risk distribution of an actual facility.

### 11. Optimization Scope

The current optimization model maximizes the total modeled maintenance risk score addressed subject to simulated budget, labor, and spare parts constraints.

It does not presently model:

- maintenance sequencing;
- multiple technician skill classes;
- crew scheduling;
- equipment dependencies;
- shutdown windows;
- maintenance duration across calendar time;
- uncertainty in maintenance cost;
- uncertainty in labor requirements;
- production losses;
- safety constraints;
- interactions among simultaneous equipment outages; or
- multi period planning.

The current optimization objective also does not explicitly minimize cost after maximizing risk addressed.

### 12. Sensitivity Analysis Scope

The current sensitivity analysis evaluates a limited set of consequence weights and resource scenarios.

Sensitivity analysis evaluates the behavior of the proof of concept under changed assumptions. It is not equivalent to validation against independent operational evidence.

### 13. External Validation

The current framework has not yet been tested against an independent real facility dataset.

It has also not been adopted, reviewed, or validated by a facility operator as an operational maintenance system.

Future work should test additional datasets and compare model outputs with historical outcomes where reliable evidence is available.

### 14. Energy System Component

No completed facility energy system reliability or scenario analysis model is included in the current implementation.

The maintenance workstream should therefore not be represented as an integrated maintenance and energy system at this stage.

### 15. Engineering and Operational Use

The current project is a computational research and development proof of concept.

It does not replace equipment inspections, preventive maintenance procedures, manufacturer requirements, engineering judgment, safety analysis, regulatory obligations, or decisions by qualified facility personnel.

No current output should be treated as an operational maintenance recommendation.

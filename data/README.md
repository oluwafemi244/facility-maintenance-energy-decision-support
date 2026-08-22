# Data Sources

This directory contains or documents datasets used during the development and testing of the Facility Maintenance and Energy Decision-Support Framework.

## AI4I 2020 Predictive Maintenance Dataset

The first dataset selected for preliminary development is the **AI4I 2020 Predictive Maintenance Dataset**, published through the UCI Machine Learning Repository.

### Source

UCI Machine Learning Repository
Dataset: AI4I 2020 Predictive Maintenance Dataset
DOI: 10.24432/C5HS5C

Official source:
https://archive.ics.uci.edu/dataset/601/ai4i

### Dataset Description

The AI4I 2020 dataset is a synthetic predictive maintenance dataset designed to reflect characteristics commonly encountered in industrial predictive maintenance applications.

The dataset contains 10,000 observations and includes operating and condition-related variables such as:

* air temperature;
* process temperature;
* rotational speed;
* torque;
* tool wear;
* machine failure; and
* individual failure mode indicators.

The dataset is being used during the preliminary development stage to test data processing procedures, equipment condition analysis, failure risk modeling, and the relationship between failure risk estimates and maintenance priority decisions.

### Important Limitation

The AI4I dataset is synthetic.

It does not represent operating records from an actual industrial facility, infrastructure operator, utility, or maintenance organization. Results produced from this dataset will therefore be treated as proof-of-concept results only.

The dataset will not be used to claim that the preliminary framework has been validated for operational use.

Future development will require additional testing using other public datasets, published case studies, simulated asset portfolios, and, where lawfully available, appropriately authorized facility data.

### License

The dataset is distributed by the UCI Machine Learning Repository under the Creative Commons Attribution 4.0 International license (CC BY 4.0).

The original dataset remains subject to the terms established by its publisher.

### Citation

AI4I 2020 Predictive Maintenance Dataset. (2020). UCI Machine Learning Repository. https://doi.org/10.24432/C5HS5C

### Current Use in This Project

The dataset will initially be used for:

1. data quality inspection;
2. exploratory analysis;
3. identification of equipment-condition variables;
4. examination of failure frequency and failure modes;
5. development of baseline failure risk models;
6. comparison of model performance measures; and
7. demonstration of how predicted failure risk may contribute to a later maintenance prioritization process.

No facility consequence ratings, maintenance budgets, labor constraints, spare part availability, or operational criticality information will be attributed to this dataset unless those variables are actually present in the source data.

Where such variables are required for framework demonstrations, they will be created separately as clearly labeled simulated or hypothetical data.

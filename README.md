# Loan default risk capstone

**How much work would a default-risk score send to an underwriting team?** I built a class-weighted Random Forest to flag loans for manual review. In the held-out test set it flagged **1,380 loans**. Of those, 787 defaulted and 593 were repaid. Another 2,186 defaults went unflagged. Those counts show the workload and the tradeoff a lender would face at the evaluated threshold.

[View the stakeholder presentation — portfolio edition](docs/loan-default-presentation.pdf) · [Read the full report — portfolio edition](docs/loan-default-report.pdf) · [Inspect the modeling notebook](notebooks/loan_default_risk_model.ipynb)

## Data and model development

The source workbook contains **148,670 historical records and 34 columns**. I selected 13 financial, loan and demographic predictors plus the default indicator, removed rows with missing values, and one-hot encoded eight categorical fields. That left **121,447 complete records**, including 19,816 defaults. Complete-case selection limits how well the results describe applicants with missing information. [Source workbook](data/Loan%20Default%20Data.xlsx)

I used a stratified 70/15/15 split. The training set contained 85,012 records. A 300-tree class-weighted baseline was evaluated on 18,217 validation records. I searched 48 parameter combinations using five-fold cross-validation on training data, selected by ROC-AUC, and evaluated the tuned model on 18,218 test records. The selected forest used 500 trees, square-root feature sampling, unrestricted depth and a minimum of two samples per leaf.

## Results

| Metric | Baseline on validation | Tuned model on test |
|---|---:|---:|
| ROC-AUC | 0.733 | 0.737 |
| Precision for default | 0.758 | 0.570 |
| Recall for default | 0.148 | 0.265 |
| F1 for default | 0.248 | 0.362 |
| Accuracy | 0.853 | 0.847 |

## Key takeaways

The test score ranked loans above chance, but the selected threshold caught only **26.5% of defaults**. A team considering a different threshold should weigh the cost of missed defaults against how many more applications its underwriters can inspect. The two columns report different splits, so they cannot isolate the effect of tuning.

This evaluation covers one historical dataset. A lending use case would also need external validation, subgroup performance checks and a review policy. Avoided losses were not measured.

## Public portfolio edition

The report and 14-slide presentation are revised portfolio editions. They retain the submitted model results, correct the removed-row count to 27,223, distinguish validation from test comparisons, and qualify significance, overfitting and financial-benefit claims. The saved notebook has no training-accuracy calculation or individual-feature significance test; these editions do not claim either was established. The original coursework submissions remain unchanged locally.

## Reproduce the analysis

Install the [requirements](requirements.txt) in a Jupyter environment and run the notebook with `notebooks/` as the working directory. The workbook is included. The parameter search performs 240 fits. For a quicker review, open the saved test results or the report. [Original report figures](assets/report-extracts/)

*Data Science Capstone for the M.S. in Data Analytics at Western Governors University (WGU), D606.*

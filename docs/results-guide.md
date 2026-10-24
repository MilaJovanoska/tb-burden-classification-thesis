# Guide to the exported results

The results directory preserves every supplied table and figure. It contains outputs from more than one model configuration, and several filenames use words such as `final` or `improved`. Use the model metadata and the final thesis to interpret these files; filenames alone are not a reliable experiment identifier.

## Main model reported in the thesis

The main interpretable model is **ordinary, unregularized logistic regression on Set B with 13 predictors**, implemented with `statsmodels.Logit`. It uses 146 training countries and 37 test countries.

| Evidence | File |
| --- | --- |
| Input dimensions and training-only screening | [19_final_modelling_input_summary.csv](../results/tables/19_final_modelling_input_summary.csv) |
| Model fitting details | [22_logistic_regression_set_b_model_fit_summary.csv](../results/tables/22_logistic_regression_set_b_model_fit_summary.csv) |
| Training and test metrics | [24_logistic_regression_set_b_evaluation_metrics.csv](../results/tables/24_logistic_regression_set_b_evaluation_metrics.csv) |
| Test confusion matrix | [26_logistic_regression_set_b_test_confusion_matrix.png](../results/figures/26_logistic_regression_set_b_test_confusion_matrix.png) |
| Test ROC curve | [27_logistic_regression_set_b_test_roc_curve.png](../results/figures/27_logistic_regression_set_b_test_roc_curve.png) |

At threshold 0.5, the test counts are TN=22, FP=6, FN=4, TP=5. Balanced accuracy is 0.6706 and ROC-AUC is 0.7619. These are the results highlighted in the repository README.

## Separate 28-predictor configuration

[38_final_model_selection_summary.csv](../results/tables/38_final_model_selection_summary.csv) describes a different model: 28 predictors, L1-regularized logistic regression, `saga` solver, `C=1`, and balanced class weights.

[39_final_test_evaluation_summary.csv](../results/tables/39_final_test_evaluation_summary.csv) reports TN=21, FP=7, FN=1, TP=8, balanced accuracy 0.8194, and ROC-AUC 0.7937 for that configuration.

These exports are preserved as supplementary experiment records. They are not the same fit as the thesis's ordinary 13-predictor model. The included notebook does not contain a verified workflow that reproduces this complete 28-predictor selection and evaluation run. Its chronology relative to other experiments cannot be established from filenames alone.

## Coverage and interpretation

- The final thesis includes comparisons with additional classifiers. The notebook contains the main statistical-screening and ordinary-logistic-regression workflow, but not a verified reproduction of every comparison table in the final thesis.
- Many tables are available in both CSV and XLSX; many figures have PNG and PDF alternatives. Both formats are retained.
- Labels claiming a held-out or first evaluation in individual exports are recorded metadata, not an independent verification of the complete experiment history.
- Permutation importance calculated on the test sample is a descriptive interpretation of that fitted model. Repeatedly selecting or revising models based on the same test results would weaken an independent evaluation claim.
- The archive was inspected for documentation and packaging, not rerun to establish numerical reproducibility.

## File preservation

[original-files-manifest.json](original-files-manifest.json) records the source, repository destination, byte count, and SHA-256 checksum of all 514 supplied files: 513 files from the ZIP plus the separately supplied final thesis. The older Word file is moved under `thesis/archive/`; original contents are unchanged.

The original ZIP also contained empty directories. They are not populated with invented data, models, or reference documents. Git tracks files rather than empty folders.

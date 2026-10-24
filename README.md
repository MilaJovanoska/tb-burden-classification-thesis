# Tuberculosis Burden Classification — Graduation Thesis

**Statistically supported and interpretable classification of countries by tuberculosis burden**

This repository accompanies my graduation thesis at the Faculty of Computer Science and Engineering (FINKI), Ss. Cyril and Methodius University in Skopje. It investigates how nutritional, human-development, socioeconomic, and vaccination indicators can help distinguish countries with higher tuberculosis (TB) incidence, while keeping the modelling process interpretable.

The study uses **183 countries**, a TB-incidence reference year of **2022**, and a statistically selected set of **13 predictors** for its main logistic-regression model. The emphasis is on transparent feature selection, model interpretation, and careful evaluation.

**Author:** Mila Jovanoska  
**Supervisor:** Prof. Dr. Biljana Tojtovska Ribarski  
**Thesis language:** Macedonian · **Document date:** June 2026

## Start here

- [Read the final thesis (Word)](<thesis/Дипломска работа-Мила Јованоска 221034.docx>) — the authoritative thesis document supplied for publication.
- [Explore the analysis notebook](notebooks/TB_Burden_Classification_Thesis.ipynb) — Python code with saved outputs.
- [Understand the datasets](data/README.md) — file roles, target definition, and source information.
- [Navigate the results](docs/results-guide.md) — thesis-aligned outputs and the distinction between different model configurations.
- [Run the notebook](#running-the-notebook) — environment and path setup.

## Research question

Can country-level nutritional, development, socioeconomic, and vaccination indicators distinguish countries with an analytically defined higher TB burden, using a transparent and interpretable classification workflow?

“High TB burden” is an **operational label for this study**: a country is assigned to the positive class when its 2022 TB incidence is at least the sample's third quartile, **152.5 cases per 100,000 people**. This produces 46 positive and 137 negative observations.

This sample-relative definition is not the WHO's official high-burden-country classification. The analysis concerns associations between national indicators; it does not estimate an individual's risk or establish causal effects.

## Data and methodology

The assembled dataset combines indicators attributed in the thesis to FAOSTAT, WHO, UNDP, and BCG vaccination coverage from Our World in Data based on WHO/UNICEF estimates. The supplied files contain the assembled dataset; raw source downloads and the complete upstream assembly workflow are not included.

The main analysis proceeds through the following stages:

1. **Audit and explore the data.** Examine distributions, missingness, and the balance between the target classes.
2. **Define predictor roles.** Exclude the country identifier, TB incidence used to define the target, target labels, and the BCG missingness flag from model inputs.
3. **Create a stratified split.** Use 146 countries for training and 37 for testing, with random seed 42.
4. **Screen predictors on training data.** Combine Welch's t-test and Mann–Whitney U tests with Benjamini–Hochberg false-discovery-rate correction and effect sizes, including Hedges' g and rank-biserial correlation.
5. **Reduce redundancy.** Use training-set Spearman correlations to select representative predictors, followed by variance inflation factor (VIF) diagnostics.
6. **Fit and evaluate models.** Use ordinary logistic regression on the resulting 13-predictor Set B as the main interpretable model, with the broader Set A and additional classifiers discussed as comparisons.
7. **Interpret the model.** Examine odds ratios and permutation importance. The latter describes the model's behaviour on the evaluated sample, rather than proving causal importance.

The final thesis also discusses a dummy baseline, Random Forest, Gradient Boosting, support vector machines, and k-nearest neighbours. The supplied notebook and results directory do not constitute a verified, end-to-end reproduction of every comparison in the thesis; see the [results guide](docs/results-guide.md).

## Main thesis results

The table below reports the **ordinary logistic-regression model on Set B (13 predictors)**, evaluated on 37 test countries at a probability threshold of 0.5.

| Metric | Test result |
| --- | ---: |
| Accuracy | 0.7297 |
| Balanced accuracy | 0.6706 |
| Precision, positive class | 0.4545 |
| Recall / sensitivity | 0.5556 |
| Specificity | 0.7857 |
| F1 score | 0.5000 |
| ROC-AUC | 0.7619 |
| Average precision | 0.4182 |

Source: [saved evaluation metrics](results/tables/24_logistic_regression_set_b_evaluation_metrics.csv), consistent with the main model reported in the final thesis.

The model correctly classified 22 of 28 negative-class countries and 5 of 9 positive-class countries, with 6 false positives and 4 false negatives. It therefore showed moderate discrimination, with a lower detection rate for the higher-burden class. Balanced accuracy is especially useful here because the classes are unequal in size.

The thesis identifies vitamin C supply from vegetables, the Human Development Index, and zinc supply from milk products among the strongest permutation-importance signals. These are model-dependent associations, not evidence that changing a particular indicator would change TB incidence.

**Results provenance:** the archive also contains a separately configured, 28-predictor regularized model. Its metrics are retained as supplied, but are not substituted for the thesis's 13-predictor model. Filenames containing `final` alone do not establish which configuration an output belongs to.

## Repository structure

```text
tb-burden-classification-thesis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── final/                    # Assembled input and labelled dataset
├── notebooks/
│   └── TB_Burden_Classification_Thesis.ipynb
├── results/
│   ├── figures/                  # All supplied visual outputs
│   └── tables/                   # All supplied tables and model summaries
├── thesis/
│   ├── README.md
│   ├── Дипломска работа-Мила Јованоска 221034.docx
│   └── archive/                  # Earlier Word document from the ZIP
└── docs/
    ├── results-guide.md
    └── original-files-manifest.json
```

All 513 files from the supplied project archive are preserved, together with the separately supplied final thesis. Original file contents are unchanged. CSV/XLSX and PNG/PDF alternatives are retained so readers can work with either data-oriented or presentation-oriented formats.

## Running the notebook

### Google Colab

The original notebook expects a project folder in Google Drive:

```text
/content/drive/MyDrive/TB_Burden_Classification_Thesis
```

1. Download and extract the repository.
2. Place its contents in a Google Drive folder named `TB_Burden_Classification_Thesis`, keeping the `data`, `notebooks`, and `results` structure intact.
3. Open `notebooks/TB_Burden_Classification_Thesis.ipynb` in Colab.
4. Run the import and Drive-mount cells, then check that `PROJECT_DIR` points to this folder.
5. Run the analysis cells in order. Install any missing packages listed in `requirements.txt` in the notebook environment.

### Local Jupyter

From the repository root, create a Python virtual environment and install the dependencies:

```bash
python -m venv .venv
# Linux/macOS:
source .venv/bin/activate
# Windows PowerShell alternative:
# .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open a working copy of the notebook. Skip the Google Drive import/mount cell, and replace the `PROJECT_DIR` assignment with the absolute path to your repository folder:

```python
PROJECT_DIR = Path("/absolute/path/to/tb-burden-classification-thesis")
```

The notebook writes into `data/final/` and `results/`. Run it in a separate copy if you want to preserve the published outputs unchanged.

**Reproducibility status:** dependencies were identified from the supplied notebook and its export operations. The original package versions were not recorded in an environment lockfile, and the notebook has not been rerun as part of preparing this repository. Installation instructions are a starting point, not a claim of exact environment reproduction.

## Limitations and future work

- **Country-level observations:** findings cannot be transferred directly to individual people.
- **Small evaluation sample:** the test set contains only 37 countries, including 9 positive cases; a few classification changes can materially affect metrics.
- **Single reference year:** the analysis does not establish performance on future years or independently collected country samples.
- **Sample-defined target:** the quartile threshold was derived from the assembled dataset before splitting, rather than from an external public-health rule.
- **Interpretation:** correlations, odds ratios, and permutation importance do not establish causality.
- **Reproduction coverage:** raw source downloads, a pinned environment, and code reproducing every archived experiment are not included.

Useful next steps include documenting source releases and indicator units more precisely, reproducing each reported model in a versioned environment, assessing feature-selection stability, and evaluating performance on additional years. Any future cross-validation workflow should fit feature selection and preprocessing within its training folds.

## Attribution and reuse

The final thesis contains the full bibliography. See the [data notes](data/README.md) for the source families used in the assembled dataset.

No repository-wide reuse licence has been assigned. Third-party datasets remain subject to their respective providers' terms; publication here does not grant additional rights over those materials. When discussing this work, attribute it to Mila Jovanoska and cite the thesis title and institution above.

# Data notes

The two supplied CSV files contain country-level observations, not patient records. Both contain 183 rows.

| File in `final/` | Columns | Role |
| --- | ---: | --- |
| `final_dataset_2022_who_faostat_hdi_bcg.csv` | 49 | Assembled input read by the notebook |
| `tb_burden_classification_dataset.csv` | 51 | Dataset with `high_tb_burden` and `tb_burden_group` added |

## Source families

The final thesis attributes nutritional supply indicators to FAOSTAT, TB incidence to WHO, human-development indicators to UNDP, and BCG coverage to Our World in Data using WHO/UNICEF estimates. Refer to references 7–10 and the data-preparation chapter of the [final thesis](<../thesis/Дипломска работа-Мила Јованоска 221034.docx>) for its descriptions.

The TB outcome refers to 2022. Do not infer that a publication year in the bibliography is also the observation year of every predictor. Exact source downloads, release identifiers, retrieval dates, and a complete raw-data reconstruction pipeline are not supplied in this archive.

## Important columns

| Column or family | Meaning / use |
| --- | --- |
| `Area` | Country identifier; excluded from predictors |
| Nutrient supply by food group | Nutritional supply indicators; preserve the supplied values and names |
| Development indicators | HDI value/rank, schooling, life expectancy, and GNI per capita |
| `BCG_coverage_pct` | BCG vaccination coverage, expressed as a percentage |
| `BCG_was_missing` | Missingness indicator; excluded from model predictors |
| `TB_incidence_2022` | Target-source incidence per 100,000 people; excluded from predictors |
| `high_tb_burden` | Binary target: 1 when incidence is at least 152.5; otherwise 0 |
| `tb_burden_group` | Human-readable target label; excluded from predictors |

GNI is labelled in the CSV as 2017 PPP dollars; schooling and life expectancy are labelled in years. Nutrient columns do not consistently encode units in their names. Consult the original indicator metadata before comparing scales, converting values, or interpreting them as individual dietary intake.

The threshold is the sample's third quartile and yields 46 high-burden and 137 non-high-burden observations. It is an analytical definition rather than an official WHO designation. The notebook excludes the outcome and its derived labels from predictors, leaving 46 candidate predictors before statistical screening.

## Preservation and reuse

Both CSV files are preserved without modification. The notebook can regenerate the labelled dataset from the assembled input, but this does not recreate the upstream collection and harmonisation process from raw source files.

Source-provider terms apply to third-party data. No separate permission or open-data licence is asserted by this repository. Preserve attribution and check the applicable source terms when reusing or redistributing the data.

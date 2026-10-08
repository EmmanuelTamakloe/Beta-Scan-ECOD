# Beta-Scan ECOD — Code, Simulations, and Reproducibility Materials

This repository contains the code, simulation notebooks, benchmark outputs, and derived figures/tables used for the manuscript:

**Beta-Scan ECOD: Likelihood-Based Adaptive Aggregation of Marginal Tail Evidence for Interpretable Anomaly Detection**

Authors: Jie Zhou, Weiqiang Dong, Hong Cheng, Emmanuel Tamakloe, and Shi Chen.

Beta-Scan ECOD is an unsupervised anomaly-detection method that calibrates featurewise ECOD contributions into empirical upper-tail probabilities and adaptively aggregates the strongest marginal evidence using a sparse Beta likelihood scan. The method returns an anomaly score together with an effective evidence dimension, a fitted tail-concentration parameter, and the feature subset producing the maximizing likelihood gain.

> **Publication status:** Please replace this note with the final journal citation and DOI after publication.

---

## Repository contents

The repository is organized around the analyses reported in the manuscript and Online Resource 1.

### `beta_scan_ecod_real_data_analysis_results/`

Main real-data benchmark analysis.

Contents include:

- `Beta_Scan_ECOD_Real_Data_Analysis.ipynb`
- the 14 benchmark datasets used in the study
- split-level benchmark results
- dataset-level summaries
- pairwise statistical comparisons
- Friedman-test results
- Beta-Scan likelihood diagnostics
- benchmark figures
- a reproducibility record

The main benchmark uses 20 stratified 60/40 reference/test splits with base seed `20260721`. Labels are used for stratification and evaluation only; detector fitting and score construction are performed without anomaly labels.

The notebook currently uses:

```python
DATA_DIR = Path(".")
OUTPUT_DIR = Path("beta_scan_ecod_no_wtk_results")
```

Therefore, when reproducing the benchmark, place the dataset files in the notebook's working directory or modify `DATA_DIR` as needed.

### `Beta_Scan_Gaussian_Copula_Dependence_Simulation/`

Controlled Gaussian-copula simulation used to study the effect of feature dependence on:

- the selected evidence dimension (`k_hat`)
- the fitted Beta shape (`a_hat`)
- the likelihood gain
- the maximized Beta-Scan score
- anomaly-detection ROC-AUC and average precision

The experiment keeps the marginal p-value distributions fixed while varying equicorrelation.

### `Empirical_PValue_Calibration_Simulation/`

Finite-sample study of the empirical upper-tail p-value calibration used by Beta-Scan.

The notebook examines:

- continuous exchangeable null contributions
- tied/discretized contributions
- held-out calibration
- leave-one-out training calibration
- cross-feature dependence
- comparison with the exact finite-sample rank benchmark

### `feature_support_recovery/`

Controlled raw-feature simulation used to evaluate intrinsic feature-level interpretation.

The experiment varies:

- support size `s ∈ {3, 5, 10}`
- mean shift `delta ∈ {1, 2, 3}`
- equicorrelation `rho ∈ {0.0, 0.3, 0.6}`

It reports support precision, recall, F1, Jaccard overlap, feature-ranking ROC-AUC, matched-size random baselines, and the relationship between the selected evidence dimension and the known generating support.

This folder also contains the paired **Beta-Scan ECOD vs. ordinary ECOD** experiment used to evaluate adaptive aggregation across different signal strengths and anomalous-feature proportions.

### `competing_method_sensitivity_practical/`

Auxiliary sensitivity analysis for eight tunable competing detectors:

- Isolation Forest
- OCSVM
- KNN
- LOF
- HBOS
- CBLOF
- LODA
- fast ABOD

The sensitivity experiment uses three prespecified splits from the main 20-split benchmark (`1`, `10`, and `20`) and three reasonable configurations per method. The same configurations are applied across datasets without dataset-specific label tuning.

The notebook searches recursively below its current working directory for the benchmark `.arff` and `.mat` files. If the repository contains more than one matching version of a dataset, use `FILE_OVERRIDES` in the notebook to select the intended file explicitly.

### `beta_scan_exact_mechanism_profiles/`

Deterministic illustration of the Beta-Scan scoring mechanism and exact likelihood profiles for representative sparse, diffuse, and null-like evidence configurations.

### `Derived_Tables/`

Code and derived outputs used to generate manuscript-level summary tables and figures from the real-data result files.

Typical inputs include:

- `overall_method_summary.csv`
- `dataset_auc_table.csv`
- `dataset_ap_table.csv`
- `dataset_inventory.csv`

---

## Software environment

The recorded environment for the main real-data analysis was:

- Python 3.12.4
- NumPy 1.26.4
- pandas 2.2.2
- SciPy 1.13.1
- scikit-learn 1.4.2
- PyOD 3.6.1

The notebooks also use Matplotlib and Jupyter.

A minimal installation is:

```bash
pip install numpy==1.26.4 pandas==2.2.2 scipy==1.13.1 scikit-learn==1.4.2 pyod==3.6.1 matplotlib jupyter
```

Exact numerical results can depend on package versions and platform details. The main benchmark folder includes `reproducibility_record.json` with the environment recorded when the supplied results were generated.

---

## Reproducing the analyses

Clone or download the repository, create a Python environment, and launch Jupyter:

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd <YOUR-REPOSITORY-NAME>

python -m venv .venv
```

Activate the environment and install the required packages.

On Windows:

```bash
.venv\Scripts\activate
pip install numpy==1.26.4 pandas==2.2.2 scipy==1.13.1 scikit-learn==1.4.2 pyod==3.6.1 matplotlib jupyter
jupyter lab
```

On macOS/Linux:

```bash
source .venv/bin/activate
pip install numpy==1.26.4 pandas==2.2.2 scipy==1.13.1 scikit-learn==1.4.2 pyod==3.6.1 matplotlib jupyter
jupyter lab
```

The simulation notebooks are self-contained and can be run independently.

For the real-data benchmark, ensure that the 14 benchmark files are visible to the notebook through `DATA_DIR`.

For the competing-method sensitivity notebook, ensure that exactly one intended version of each benchmark dataset is found beneath `DATA_DIR`, or specify the desired paths in `FILE_OVERRIDES`.

The supplied CSV and PNG files are the outputs used in the manuscript/revision analyses. Re-running the notebooks may overwrite or regenerate those files.

---

## Benchmark datasets

The real-data study uses **14 public anomaly-detection benchmark files**: nine ARFF benchmark variants associated with the DAMI outlier-evaluation collection and five MATLAB benchmark files obtained through the ODDS library.

### Dataset inventory used in this repository

| Dataset in the study | File used | Benchmark source credited |
|---|---|---|
| ALOI | `ALOI_withoutdupl_norm.arff` | Campos et al. / DAMI outlier-evaluation collection |
| Arrhythmia | `Arrhythmia_withoutdupl_norm_46.arff` | Campos et al. / DAMI outlier-evaluation collection |
| Cardiotocography | `Cardiotocography_withoutdupl_norm_10_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| InternetAds | `InternetAds_withoutdupl_norm_10_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| KDDCup99 | `KDDCup99_original.arff` | Campos et al. / DAMI outlier-evaluation collection |
| PageBlocks | `PageBlocks_withoutdupl_norm_05_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| Shuttle | `Shuttle_withoutdupl_norm_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| SpamBase | `SpamBase_withoutdupl_norm_10_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| Wilt | `Wilt_withoutdupl_norm_02_v10.arff` | Campos et al. / DAMI outlier-evaluation collection |
| Ionosphere | `ionosphere.mat` | ODDS Library |
| Lympho | `lympho.mat` | ODDS Library |
| Mammography | `mammography.mat` | ODDS Library |
| Musk | `musk.mat` | ODDS Library |
| Vertebral | `vertebral.mat` | ODDS Library |

The ARFF filenames identify benchmark variants that may include preprocessing such as duplicate removal, normalization, or anomaly-rate subsampling. They should therefore be treated as the specific benchmark versions used in this study rather than assumed to be identical to the corresponding original raw datasets.

### Credit for the DAMI benchmark collection

Please cite:

> Campos, G. O., Zimek, A., Sander, J., Campello, R. J. G. B., Micenková, B., Schubert, E., Assent, I., & Houle, M. E. (2016). On the evaluation of unsupervised outlier detection: measures, datasets, and an empirical study. *Data Mining and Knowledge Discovery, 30*(4), 891–927. https://doi.org/10.1007/s10618-015-0444-8

Benchmark collection:

https://www.dbs.ifi.lmu.de/research/outlier-evaluation/DAMI/

### Credit for the ODDS Library

Please cite:

> Rayana, S. (2016). *ODDS Library*. Stony Brook University, Department of Computer Science.

ODDS:

https://shebuti.com/outlier-detection-datasets-odds/

ODDS notes that its library contains datasets originating from multiple research groups and that some dataset pages may request additional dataset-specific citations. Users should retain any additional attribution requested by the original source.

### Dataset redistribution note

The datasets are credited to their respective benchmark repositories and original data providers; they are **not authored by the authors of Beta-Scan ECOD**.

Public availability of a benchmark does not automatically imply that every underlying dataset has the same redistribution license. Before publishing copies of the raw benchmark files in a GitHub repository, please verify the terms applicable to each source dataset and benchmark collection.

If redistribution rights are uncertain, the safest repository structure is to omit the raw dataset files and provide:

1. the benchmark filenames used in the study;
2. the source links above;
3. `dataset_inventory.csv` with dimensions, class counts, and SHA-256 hashes; and
4. instructions telling users where to place downloaded files before running the notebooks.

This approach preserves reproducibility while leaving data acquisition subject to the source repositories' terms.

---

## Reproducibility details

The main benchmark uses:

- 14 benchmark datasets
- 20 stratified splits per dataset
- 60% unlabeled reference / 40% held-out evaluation
- base seed `20260721`
- ROC-AUC and average precision as primary performance metrics
- dataset-level averaging before cross-dataset summaries
- Friedman tests for overall method differences
- paired Wilcoxon signed-rank comparisons with Holm multiplicity adjustment
- 5,000 bootstrap resamples for confidence intervals in the supplied benchmark analysis

The prespecified ABOD execution limit is `n_train <= 5000`; therefore, ABOD has coverage on 11 of the 14 benchmark datasets.

---

## Main output files

Important real-data outputs include:

- `split_level_results.csv` — split-level scores and metrics
- `dataset_method_summary.csv` — dataset-level method summaries
- `overall_method_summary.csv` — cross-dataset method summaries
- `dataset_auc_table.csv` — dataset-level ROC-AUC table
- `dataset_ap_table.csv` — dataset-level average-precision table
- `beta_scan_ecod_pairwise_tests.csv` — paired statistical comparisons
- `friedman_tests.csv` — Friedman-test results
- `beta_scan_dataset_diagnostics.csv` — dataset-level Beta-Scan diagnostics
- `beta_scan_split_diagnostics.csv` — split-level Beta-Scan diagnostics
- `dataset_inventory.csv` — benchmark metadata, labels, and SHA-256 hashes
- `reproducibility_record.json` — recorded software/reproducibility information

Each simulation directory similarly contains its notebook, raw or configuration-level CSV output, summary tables, and figures.

---

## Citation

If you use Beta-Scan ECOD, its code, or the supplied experimental results, please cite the associated paper after publication.

Temporary citation:

> Zhou, J., Dong, W., Cheng, H., Tamakloe, E., & Chen, S. *Beta-Scan ECOD: Likelihood-Based Adaptive Aggregation of Marginal Tail Evidence for Interpretable Anomaly Detection*. Manuscript submitted for publication.

A final BibTeX entry can replace the placeholder below when the article is published:

```bibtex
@article{zhou_betascan_ecod,
  title   = {Beta-Scan ECOD: Likelihood-Based Adaptive Aggregation of Marginal Tail Evidence for Interpretable Anomaly Detection},
  author  = {Zhou, Jie and Dong, Weiqiang and Cheng, Hong and Tamakloe, Emmanuel and Chen, Shi},
  journal = {International Journal of Data Science and Analytics},
  year    = {YEAR},
  volume  = {VOLUME},
  number  = {NUMBER},
  pages   = {PAGES},
  doi     = {DOI}
}
```

Please also cite the benchmark data sources listed in the **Benchmark datasets** section when using the supplied real-data experiments.

---

## Acknowledgments

We thank the maintainers and original contributors of the public anomaly-detection benchmark collections used in this study, including the DAMI outlier-evaluation collection associated with Campos et al. (2016) and the ODDS Library maintained by Shebuti Rayana.

We also acknowledge the developers and maintainers of the open-source scientific Python ecosystem and PyOD used in the benchmark implementation.

---

## Contact

For questions about the Beta-Scan ECOD methodology or the reproducibility materials:

**Jie Zhou**  
Department of Mathematics & Computer Science  
Southern Arkansas University  
Email: jzhou@saumag.edu

---

# Subject-Independent Comparison of SVM, KNN and Decision Tree for Smartphone-Based Human Activity Recognition

Code and outputs accompanying the manuscript *"Akıllı Telefon Sensör Verileriyle İnsan Aktivite Tanımada Klasik Sınıflandırıcıların Denek-Bağımsız Karşılaştırması: SVM, KNN ve Karar Ağacı"* (Etebeklioğlu & Aslan, 2026).

## What this repository contains

| Path | Content |
|---|---|
| `notebooks/01_main_pipeline.ipynb` | Main Colab pipeline: data audit, **data-leakage diagnostics**, subject-grouped hyperparameter search, test-set evaluation, statistical tests (McNemar, Friedman, Wilcoxon-Holm), PCA, feature ablation, noise robustness, efficiency |
| `notebooks/02_extension_experiments.ipynb` | Extension: Random Forest and Gradient Boosting baselines, subject-level (cluster) bootstrap CIs, repeated subject-grouped CV, leave-one-subject-out (LOSO) |
| `notebooks/03_leakage_and_grid_checks.ipynb` | Naive-vs-grouped CV leakage bias for all six models; extended-grid sensitivity check (outputs `har_checks_outputs.zip`) |
| `results/main/` | All tables (`.csv`, `.tex`) and figures (`.png`, `.pdf`) produced by notebook 01, plus `config_and_environment.json` |
| `results/extension/` | Outputs of notebook 02 (tables `Ext01`-`Ext13`, figures `FigureExt01`-`FigureExt04`) |
| `results/checks/` | Outputs of notebook 03 (tables `Ext14`-`Ext15`, `FigureExt05`) |
| `requirements.txt` | Package versions |

## Data

UCI "Human Activity Recognition Using Smartphones" dataset (Anguita et al., 2013), obtained through Kaggle
(`uciml/human-activity-recognition-with-smartphones`) with `kagglehub`. 30 subjects, 6 activities, 561 pre-computed features.
The predefined split is subject-independent: 21 training subjects (7,352 windows) and 9 test subjects (2,947 windows).
The data are **not** redistributed here.

## How to reproduce

1. Open `notebooks/01_main_pipeline.ipynb` in Google Colab and run all cells (CPU is sufficient; the hyperparameter searches alone took about 45 minutes in our run, KNN being the slowest). Set `FAST_MODE = True` for a quick trial.
2. The notebook writes `har_experiment_outputs.zip` with every table and figure.
3. Open `notebooks/02_extension_experiments.ipynb` and run all cells (expected to take about an hour; `FAST_MODE`, `RUN_LOSO`, `RUN_REPEATED_CV` switches at the top). It uses the best hyperparameters found in step 1 and does not repeat the grid searches.

4. Open `notebooks/03_leakage_and_grid_checks.ipynb` and run all cells (about 35 minutes; `FAST_MODE` switch at the top). It measures the naive-vs-grouped CV bias for all six models and re-runs the grid search on extended grids for the hyperparameters that were chosen at a grid boundary.

All random seeds are fixed (`SEED = 42`).

## Headline results (held-out test subjects, n = 2,947)

| Model | Accuracy (%) | Macro-F1 |
|---|---|---|
| SVM (linear) | 96.17 | 0.962 |
| SVM (RBF) | 95.86 | 0.958 |
| KNN | 90.60 | 0.902 |
| Decision Tree | 85.17 | 0.848 |

### Extension results (same test subjects)

| Model | Test accuracy (%) [subject-level 95% CI] | 3x10-fold CV (%) | LOSO (%) |
|---|---|---|---|
| SVM (linear) | 96.17 [93.83-97.94] | 95.47 | 95.63 +/- 4.18 |
| SVM (RBF) | 95.86 [93.75-97.62] | 95.12 | 95.30 +/- 4.53 |
| KNN | 90.60 [88.08-92.96] | 91.60 | 92.01 +/- 4.53 |
| Decision Tree | 85.17 [79.88-89.38] | 87.85 | 87.06 +/- 6.65 |
| Random Forest | 92.64 [89.41-95.56] | 93.49 | 93.47 +/- 5.77 |
| Gradient Boosting | 93.48 [89.04-96.92] | 94.65 | not run |

A naive subject-mixing 5-fold CV overestimates macro-F1 by 5.8 points (0.986 vs. 0.928) relative to subject-grouped CV (linear SVM, single seed). Over three repeats and six models (notebook 03) the bias is 5.4 (linear SVM), 5.8 (RBF SVM), 7.4 (KNN), 9.7 (Decision Tree), 7.2 (Random Forest) and 7.6 (Gradient Boosting) points.

## Citation

Etebeklioğlu, A., & Aslan, Z. (2026). *Akıllı telefon sensör verileriyle insan aktivite tanımada klasik sınıflandırıcıların denek-bağımsız karşılaştırması: SVM, KNN ve Karar Ağacı.* [Update with venue/DOI once published.]

## License

MIT License (see `LICENSE`). The UCI HAR dataset is subject to its own terms (Anguita et al., 2013).

## References

Anguita, D., Ghio, A., Oneto, L., Parra, X., & Reyes-Ortiz, J. L. (2013). A public domain dataset for human activity recognition using smartphones. *Proceedings of ESANN 2013*, 437-442.

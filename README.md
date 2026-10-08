# WOA-XGBoost for Lung Sound Classification — Reproducibility Code

Code to reproduce the results of the paper **"<An Optimization-Driven and Interpretable Ensemble Learning Framework for
Stethoscope-Based Lung Sound Classification>"**.

The study classifies six lung-sound types (coarse crackles, fine crackles, normal, pleural rub, rhonchi, wheezing) from acoustic features of 3-second audio segments, using an **XGBoost** classifier whose hyperparameters and feature subset were tuned with the **Whale Optimization Algorithm (WOA)**.

## What this repository reproduces

The finalized WOA-XGBoost model (hyperparameters and feature subset chosen by the optimizer and by voting) evaluated with the paper's 10-fold protocol.

| Item | Value |
|---|---|
| `learning_rate` | 0.009814178902222342 |
| `n_estimators` | 289 |
| `max_depth` | 12 |
| `min_child_weight` | 3 |
| Selected features | 6 of the available columns, at positions `{0, 2, 4, 6, 7, 8}` |
| Random seed | 42 |

### Expected results (mean of 10 folds, %)

| | Accuracy | F-score | Precision | Recall |
|---|---|---|---|---|
| Train | 96.93 | 96.93 | 96.95 | 96.93 |
| **Test** | **70.38** | **70.07** | **70.69** | **70.38** |

## Evaluation protocol
0. Download and load the preprocessed dataset (Lung_Sound_975_Audio_3s_Segment_Features.csv) and the ipynb file (Reproducibility_Code_WOA_XGBoost.ipynb).
1. Drop `Location`, `Lung Sound ID`, `Gender`; target is `Lung Sound Type`.
2. Shuffle all 975 rows once (`random_state=42`).
3. Ten splits: each test set is a window of 30 % of the rows (292 rows), shifted by 10 % (97 rows) per fold, wrapping around the end of the data; the remaining rows form the training set. Consecutive test windows therefore overlap.
4. Categorical columns are label-encoded using encoders fitted on the training split only.
5. Features are selected by position (after removing the target), and the XGBoost model is refit from scratch on every fold.

## Environment

Results were produced with the following versions (Google Colab, Linux x86-64, CPU):

| Package | Version |
|---|---|
| xgboost | 3.2.0 |
| scikit-learn | 1.6.1 |
| pandas | 2.2.3 |
| numpy | 2.1.3 |
| scipy | 1.16.3 |


Dataset: Torabi, Y., Shirani, S., & Reilly, J. (2025). *HLS-CMDS: Heart and Lung Sounds Dataset Recorded from a Clinical Manikin using Digital Stethoscope*. UCI Machine Learning Repository. <https://doi.org/10.1109/IEEEDATA.2025.3566012> (CC BY 4.0).

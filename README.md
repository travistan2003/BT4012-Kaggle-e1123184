# BT4012 Kaggle Competition (e1123184)

Code for my submission to the BT4012 Kaggle competition

## Files

| File | What it is |
|---|---|
| `e1123184.ipynb` | Full pipeline: graph features, out-of-time validation, model comparison, final models, and both submission files. Saved with all outputs. |
| `requirements.txt` | Python version and package versions used. |

## Data

The competition data is not included. Download the four files from the Kaggle competition page and put them in a
`data/` folder next to the notebook:

```
e1123184.ipynb
requirements.txt
data/
    train.csv
    test.csv
    txs_edgelist.csv
    sample_submission.csv
```

The notebook reads them from `data/` (set by `DATA_DIR` in the "Load the data" cell).

## How to run

1. Python 3.10.11: `pip install -r requirements.txt`
2. Put the data files in `data/` as shown above.
3. Run the notebook top to bottom, for example `jupyter execute e1123184.ipynb`, or Run All in Jupyter.

## Final submissions

The last code cell (Section 12) refits every model on all training data and writes both submissions I selected for
the private leaderboard:

| File written | Kaggle submission | Public score | Model |
|---|---|---|---|
| `submission_E3.csv` | `submission_E3_both_w100.csv` (final) | 0.96168 | Illicit-vs-rest ensemble alone (`FINAL_SET = "both"`, `FINAL_W = 1.0`) |
| `submission_B1.csv` | `submission_B1_sup_plus_illrest.csv` (hedge) | 0.95830 | Even logit blend (w = 0.5) of the supervised and illicit-vs-rest ensembles, default components |

* **Supervised ensemble:** LightGBM + XGBoost + CatBoost trained on the labelled transactions (illicit vs licit).
* **Illicit-vs-rest ensemble:** the same three models trained on all training transactions, with the unlabelled ones
  counted as negatives.
* **Blend:** `(1 - w) * logit(p_supervised) + w * logit(p_illicit_vs_rest)`.
* **E3** averages a default and a slower-learning version of each model and uses only the 165 provided features.
  With w = 1 the supervised ensemble gets no weight, so E3 is the illicit-vs-rest ensemble alone.
* **B1** uses the default version of each model. Its supervised half also uses the 30 structural graph features
  built in Section 3, as my first-round supervised model did.

## Reproducibility note

The submitted files were generated in a different environment, so the files the notebook writes are very close to them but not bit-identical (rank correlation 0.996 for E3 and 0.998 for B1). Gradient-boosting libraries can differ slightly across machines and versions; the versions in `requirements.txt` give the closest match.

# data/

This folder contains the dataset used for training, evaluating, and running inference with the Git Commit Message Generator models.

---

## Contents

The main file expected in this folder is:

### `cleaned_python_commit_dataset.csv`

A CSV file with two columns:

- `commit_message`: The original commit message written by a human.
- `diff`: The corresponding Git diff (code changes).

This dataset was curated to include only commits that:
- Modify Python files (`.py`) exclusively
- Have commit messages between 50–300 characters
- Avoid merge commits, metadata, or noisy logs (cleaned out)

---

## How to Get the Dataset

You can download the cleaned dataset from this link:  
[Download cleaned_python_commit_dataset.csv](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/EkQcXbLjIYFMiIpW6HXFnhQBW4b0aO_eV3HBy3bKOR1E1w?e=phvKRu)

Once downloaded, place it in:
- `decoder-only-transformer/data/`
- `gated-recurrent-unit/data/`

---

## Note

If you're planning to create your own dataset from scratch using the data pipeline, this file will be replaced or regenerated. Otherwise, this file is ready to use for training, inference, or evaluation.

---

Let me know if you'd like a preview script or helper functions to load this CSV cleanly in your notebook.

# Data Pipeline for Git Commit Dataset

This folder contains scripts to build a dataset of GitHub commit messages and their corresponding diffs, filtered down to Python-only changes. The goal is to get clean, meaningful data for training ML models that can generate commit messages.

> **This pipeline is specifically for loading and creating your own custom dataset from GitHub.**  
> If you're using the provided dataset, you can skip this step entirely.


---

## Overview

The pipeline runs in three parts:

### 1. **Collect Commits**

- Pulls commit metadata from a list of GitHub repos
- Filters for commits that:
  - Only touch `.py` files
  - Have commit messages between 50–300 characters
- Saves to `python_only_commits.csv`

> You'll need to plug in a GitHub token to avoid hitting the API rate limit.

---

### 2. **Fetch Diffs**

- Uses the commit URLs from the previous step
- Fetches each diff using GitHub’s API
- Uses threads to speed things up
- Saves results to `python_commit_dataset.csv` with two columns:  
  `commit_message`, `diff`

---

### 3. **Clean Messages**

- Cleans out merge boilerplate and metadata like:
  - `Signed-off-by`
  - `Merge pull request #...`
  - `PiperOrigin-RevId`
- Writes the cleaned data to `cleaned_python_commit_dataset.csv`


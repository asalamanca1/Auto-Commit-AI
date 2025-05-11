# Git Commit Message Generator

Generate Git commit messages from raw code diffs using deep learning.

This project implements and compares two approaches:
- A **GPT-2 decoder-only transformer** trained from scratch(flagship model).
- A **gated recurrent unit (GRU)**–based baseline model.

Each model is trained on real GitHub commits that only modify Python files. The goal is to automatically generate meaningful, readable commit messages directly from code changes.

> **Note:** All code and notebooks were developed to run in **Google Colab**. 

---

## Project Structure 
<pre>
Auto-Commit-AI/
│
├── data-pipeline/               # Scripts to collect, filter, and clean GitHub commit data
│
├── decoder-only-transformer/   # Notebooks and evaluation for the Transformer model
│
├── gated-recurrent-unit/       # GRU-based model training, inference, and evaluation
│
└── README.md                    # You’re here
 </pre>

---

## Models

### Decoder-Only Transformer

- Trained on a custom BPE tokenizer.
- Takes a code diff and generates a commit message autoregressively.
- Evaluation metrics include BLEU, ROUGE-L, METEOR, and BERTScore.

### GRU Language Model

- Simpler RNN-based model trained on the same dataset.
- Useful for comparing performance vs. the Transformer.

---

## Dataset

- Only includes commits that:
  - Touch Python files only (`.py`)
  - Have reasonably descriptive commit messages (50–300 chars)
- Includes both the raw commit message and the Git diff.
- Cleaned to remove metadata like `Signed-off-by`, PR merge boilerplate, etc.

---

## Getting Started

1. Run the data pipeline (in `data-pipeline/`) to collect and prepare the dataset.
2. Choose a model to train:
   - `decoder-only-transformer/Training-Git-Commit-Transformer.ipynb`
   - `gated-recurrent-unit/Training-Git-Commit-GRU.ipynb`
3. Evaluate and compare performance in the respective evaluation notebooks.

---

## Notes

- All code uses public GitHub data.
- GitHub token required to run data collection scripts (due to rate limits).
- Evaluation includes detailed metric plots and JSON output for analysis.

---

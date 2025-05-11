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
│   └── data/                    # Place dataset here
│   └── trained_model/          # Place transformer weights here
│
├── gated-recurrent-unit/       # GRU-based model training, inference, and evaluation
│   └── data/                    # Place dataset here
│   └── trained_model/          # Place GRU weights here
│
└── README.md                    # You’re here
 </pre>

---

## Download Links

Before running the models, download and place the following files in the specified folders:

### Dataset (`cleaned_python_commit_dataset.csv`)
Download: [Dataset Link](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/EkQcXbLjIYFMiIpW6HXFnhQBW4b0aO_eV3HBy3bKOR1E1w?e=phvKRu)  
Place in:
- `decoder-only-transformer/data/`
- `gated-recurrent-unit/data/`

> **Skip running the data pipeline if you're using this dataset.**  
> Run the pipeline only if you want to collect a custom dataset from GitHub.

---

### GRU Model Weights (`gru_model.pt`)
Download: [GRU Model Link](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/EgOSW8RSa4xKhyKJ6hLoZRgBkdQvNwxkBIUiWCBYIvwcGw?e=K2BH17)  
Place in:
- `gated-recurrent-unit/trained_model/`

---

### Transformer Model Weights  
Download: [Transformer Weights Link](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/El-lkqa_QxJFvHOLX2vYYiEB-odTJDs3Z7NLcN83k1Kz_g?e=tXEITm)  
Place all three files in:
- `decoder-only-transformer/trained_model/`

Files:
- `model.safetensors`
- `generation_config.json`
- `config.json`

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

> **Skip the data pipeline unless you specifically want to create your own dataset.**  
> The cleaned dataset is already provided and ready to use.

1. **(Optional)** Run the data pipeline in `data-pipeline/` to collect your own dataset.
   - If you're using the provided dataset, skip this step.

2. Download the dataset and model weights using the links above, and place them in the correct folders:

   - Place `cleaned_python_commit_dataset.csv` in:
     - `decoder-only-transformer/data/`
     - `gated-recurrent-unit/data/`

   - Place GRU and Transformer model weights in their respective `trained_model/` folders.

3. Choose your use case:

   - **Just want to run inference or evaluate?**
     - **Make sure to load:**
       - The cleaned dataset (`cleaned_python_commit_dataset.csv`) in the correct `data/` folder
       - The appropriate **pre-trained model weights** in the correct `trained_model/` folder
       - The **custom tokenizer** used during training (automatically loaded in the notebooks)
     - Then run:
       - `decoder-only-transformer/Inference-for-Git-Commit-Transformer.ipynb`
       - `gated-recurrent-unit/Inference_for_Git_Commit_GRU.ipynb`
     - Or evaluate performance with:
       - `decoder-only-transformer/Performance-Evaluation-for-Git-Commit-Transformer.ipynb`
       - `gated-recurrent-unit/Performance_Evaluation_for_Git_Commit_GRU.ipynb`

   - **Want to train a model from scratch?**
     - Load only the dataset — **do not load the pre-trained weights.**
     - Use:
       - `decoder-only-transformer/Training-Git-Commit-Transformer.ipynb`
       - `gated-recurrent-unit/Training-Git-Commit-GRU.ipynb`

---

## Notes

- All code uses public GitHub data.
- GitHub token required to run data collection scripts (due to rate limits).
- Evaluation includes detailed metric plots and JSON output for analysis.

---

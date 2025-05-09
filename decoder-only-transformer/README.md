# Git Commit Message Generator

This project builds a machine learning pipeline to generate Git commit messages from code diffs using a decoder-only transformer (GPT-2 architecture). The model is trained from scratch on high-quality Python-only commits from open-source repositories. It learns to map code changes (diffs) to natural language commit messages using a custom BPE tokenizer and a GPT-2 model configured with 125M parameters.

The project is organized into four modular notebooks:

- Data is collected directly from GitHub using the REST API.
- The model is trained using a custom tokenizer and transformer architecture.
- Inference is performed on both test data and user-supplied diffs.
- A separate evaluation notebook scores model outputs and visualizes metric distributions.

---

## Notebook Overview

### `Data-Pipeline-for-Git-Commit-Transformer.ipynb`
Handles data collection and cleaning:
- Uses the GitHub API to fetch Python-only commits from specified repositories.
- Filters commit messages by length and content.
- Downloads associated diffs for each commit.
- Applies regex-based cleaning to remove noise and metadata from commit messages.
- Outputs the final dataset as `data/cleaned_python_commit_dataset.csv`.

### `Training-Git-Commit-Transformer.ipynb`
Responsible for model and tokenizer training:
- Trains a Byte-Pair Encoding (BPE) tokenizer using the commit + diff dataset.
- Builds a GPT-2 language model with:
  - 20 transformer layers
  - 768 hidden size
  - 12 attention heads
  - 48k vocabulary size
  - 1024 context window
- Trains the model using cross-entropy loss with `AdamW`.
- Tracks and saves:
  - Training and validation loss
  - Weight norms
  - Loss curves (`loss_curve.png`)
  - Weight norm plots (`loss_vs_weight_norm.png`)
- Saves the trained model to `trained_model/`.

### `Inference-for-Git-Commit-Transformer.ipynb`
Performs inference on new or test Git diffs:
- Loads the trained GPT-2 model and tokenizer.
- Defines a function `generate_commit_message()` that:
  - Takes a Git diff as input
  - Appends `<endOfDiff>` and generates tokens until `<endOfCommitMessage>`
- Runs inference on:
  - A manually provided sample diff
  - A random example from the original validation set

### `Performance_Evaluation_for_Git_Commit_Transformer.ipynb`
Evaluates model performance using multiple metrics:
- Runs inference on 100 samples from the dataset.
- Scores each output using:
  - **BLEU**
  - **ROUGE-L**
  - **METEOR**
  - **BERTScore**
  - **RAGAs Answer Correctness** (requires OpenAI API key + paid credits)
- Saves the results to `sample_metrics_with_ragas_and_bertscore.json`
- Formats the results into a readable file: `all_diffs.txt`
- Visualizes the metric distributions and saves:
  - `bleu4_distribution.png`
  - `rouge_l_distribution.png`
  - `meteor_distribution.png`
  - `bert_score_f1_distribution.png`
  - `answer_correctness_distribution.png`

> Note: To run RAGAs, you must have an OpenAI API key set in `os.environ["OPENAI_API_KEY"]` and a paid plan or credits on your account.

---

## `custom_bpe_tokenizer.json`

This file contains the trained Byte-Pair Encoding (BPE) tokenizer used by the transformer model.

- Trained on the custom dataset produced by the data pipeline (`data/cleaned_python_commit_dataset.csv`)
- Uses a vocabulary size of 48,000 tokens
- Includes special tokens: `<pad>`, `<endOfDiff>`, `<endOfCommitMessage>`
- Uses byte-level pre-tokenization and decoding to match GPT-2 conventions

The tokenizer is saved in JSON format and loaded with Hugging Face’s `PreTrainedTokenizerFast`.  
It is required for both training and inference.

---

## `trained_model/`

This folder contains the saved weights and configuration of the trained GPT-2 decoder-only transformer.

- Trained from scratch for **10 epochs** on the custom commit dataset
- Ran for approximately **16 hours on a single A100 GPU**
- Architecture:
  - 20 transformer layers
  - 768 hidden size
  - 12 attention heads
  - 1024 token context window

Contents of the folder include:
- `config.json` — model architecture and hyperparameters
- `generation_config.json` — settings used during inference (e.g., max tokens, decoding strategy)
- `model.safetensors` — the trained model weights, stored in the `safetensors` format for safer, faster loading

This directory is required to run both `Inference-for-Git-Commit-Transformer.ipynb` and `Performance_Evaluation_for_Git_Commit_Transformer.ipynb`.



# Git Commit Message Generator

This project builds a machine learning pipeline to generate Git commit messages from code diffs using a decoder-only transformer (GPT-2 architecture). The model is trained from scratch on high-quality Python-only commits from open-source repositories. It learns to map code changes (diffs) to natural language commit messages using a custom BPE tokenizer and a GPT-2 model configured with 125M parameters.

---

## Model Summary

- **Architecture**: GPT-2 style transformer (decoder-only)
- **Tokenizer**: Custom Byte Pair Encoding (BPE) tokenizer trained on the dataset
- **Input**: Git diff
- **Output**: Autoregressively generated commit message
- **Training Objective**: Language modeling with start, diff delimiter, and end tokens

---

<pre>
## Project Structure
decoder-only-transformer/
│
├── custom_bpe_tokenizer.json # Tokenizer trained on cleaned diffs and messages
│
├── data/
│ └── cleaned_python_commit_dataset.csv # Preprocessed dataset of Git diffs and commit messages created from custom data pipeline
│
├── evaluation-metrics/
│ ├── all_diffs.txt # Inputs and generated outputs for 100 samples in readable format
│ ├── answer_correctness_distribution.png # Histogram of RAGAs Answer Correctness
│ ├── bert_score_f1_distribution.png # Histogram of BERTScore F1
│ ├── bleu4_distribution.png # Histogram of BLEU-4 scores
│ ├── meteor_distribution.png # Histogram of METEOR scores
│ ├── rouge_l_distribution.png # Histogram of ROUGE-L scores
│ ├── sample_metrics_with_*.json # Full evaluation results in JSON
│ └── README.md # Evaluation results documentation
│
├── trained_model/
│ ├── config.json # Model architecture configuration
│ ├── generation_config.json # Decoding parameters for inference
│ ├── model.safetensors # Final trained weights in safetensors format
│
├── Training-Git-Commit-Transformer.ipynb # Tokenizer + Transformer training
├── Inference-for-Git-Commit-Transformer.ipynb # Generates commit messages from diffs
├── Performance_Evaluation_for_Git_Commit_Transformer.ipynb # Evaluates model with multiple metrics
├── README.md # ← You are here
</pre>


---

## Notebook Breakdown

### `Training-Git-Commit-Transformer.ipynb`
- Trains a BPE tokenizer using the cleaned dataset.
- Builds and trains a GPT-2 model from scratch:
  - 20 layers, 768 hidden size, 12 attention heads, 1024 context window
- Saves weights to `trained_model/model.safetensors`.

### `Inference-for-Git-Commit-Transformer.ipynb`
- Loads tokenizer and trained model.
- Uses `generate_commit_message()` to produce predictions from new diffs.
- Supports inference on both custom and validation samples.

### `Performance_Evaluation_for_Git_Commit_Transformer.ipynb`
- Runs generation and scoring for 100 random samples from the dataset.
- Computes:
  - BLEU-4
  - ROUGE-L
  - METEOR
  - BERTScore
  - RAGAs Answer Correctness (LLM-based)
- Saves results in both JSON and visual formats (PNG plots, text outputs).

---

## OpenAI Key Required for RAGAs

> **To evaluate Answer Correctness (RAGAs)**:
>
> - You must set your OpenAI API key:
>   ```python
>   os.environ["OPENAI_API_KEY"] = "sk-..."  # Paid key required
>   ```
> - RAGAs uses GPT-3.5/GPT-4 under the hood and **requires credits**.

---

## Tokenizer Details

- Stored in `custom_bpe_tokenizer.json`
- Vocabulary size: 48,000 tokens
- Trained on both commit messages and diffs
- Uses GPT-2 compatible byte-level pre-tokenization

---

## Model Configuration

- Trained from scratch on ~75k samples for 10 epochs
- Hardware: Single A100 GPU (~16 hours)
- Specs:
  - 20 decoder-only layers
  - 768 hidden units
  - 12 attention heads
  - Context window of 1024 tokens

Saved in `trained_model/` with:
- `model.safetensors`
- `config.json`
- `generation_config.json`

---

## Evaluation Metrics

| Metric           | Purpose                                      |
|------------------|----------------------------------------------|
| **BLEU-4**        | Measures n-gram overlap                     |
| **ROUGE-L**       | Longest common subsequence recall           |
| **METEOR**        | Semantic alignment with synonyms            |
| **BERTScore**     | Embedding-based similarity                  |
| **RAGAs**         | GPT-graded factual alignment (LLM-based)    |

---

This decoder-only transformer serves as the **flagship model** in this project and **outperforms a GRU-based baseline** on both syntactic and semantic metrics. It is a fully custom pipeline, from tokenizer to evaluation, designed for the task of code-to-natural language generation.


# Gated Recurrent Unit for Git Commit Message Generation

This project trains and evaluates a baseline GRU-based language model to generate commit messages from Git diffs. The model uses a custom-trained Byte Pair Encoding (BPE) tokenizer and is evaluated using both standard NLP metrics and LLM-based factual scoring (RAGAs).

---

## Directory Structure
<pre>
gated-recurrent-unit/
│
├── custom_bpe_tokenizer.json # Custom tokenizer trained on commit message data
│
├── data/
│ └── cleaned_python_commit_dataset.csv # Preprocessed dataset of Git diffs and commit messages created from custom data pipeline
│
├── trained_model/
│ └── gru_model.pt # Final trained GRU model weights
│
├── evaluation-metrics/
│ ├── all_gru_diffs.txt # Inputs and generated outputs for 100 samples in readable format
│ ├── sample_metrics_with_...json # Full JSON with BLEU, METEOR, ROUGE, BERTScore, RAGAs
│ ├── gru_loss_curve.png # Training loss over epochs
│ ├── gru_bleu4_distribution.png # BLEU-4 histogram
│ ├── gru_meteor_distribution.png # METEOR score histogram
│ ├── gru_rouge_l_distribution.png # ROUGE-L histogram
│ ├── gru_bert_score_f1_distribution.png # BERTScore F1 histogram
│ ├── gru_answer_correctness_distribution.png # RAGAs factual correctness histogram
│
├── Training-Git-Commit-GRU.ipynb # Notebook to train the GRU model
├── Inference-for-Git-Commit-GRU.ipynb # Notebook to load model and generate predictions
├── Performance-Evaluation-Git-Commit-GRU.ipynb # Notebook to compute and visualize evaluation metrics
</pre>


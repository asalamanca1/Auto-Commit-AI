# trained_model/

This folder contains the saved weights and configuration files for the **decoder-only transformer** model used to generate Git commit messages from code diffs.

---

## Files

The following three files should be placed here:

- `model.safetensors`  
  → The actual trained model weights.

- `config.json`  
  → Configuration file specifying model architecture (e.g., number of layers, attention heads, embedding size, etc.).

- `generation_config.json`  
  → Settings used during inference (e.g., max length, temperature, top-k/top-p sampling, etc.).

---

## How to Get the Files

[Download Transformer Model Weights](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/El-lkqa_QxJFvHOLX2vYYiEB-odTJDs3Z7NLcN83k1Kz_g?e=tXEITm)

Unzip or extract the contents and place all three files in this folder.

---

## When to Use This Folder

Only needed if you're:
- Running **inference** with `Inference-for-Git-Commit-Transformer.ipynb`
- Running **evaluation** using the pre-trained model

**If you're training a new model from scratch**, you can ignore or overwrite the contents of this folder.

---

## Format

All files are saved using Hugging Face-compatible format. You can load the model with:

```python
from transformers import GPT2LMHeadModel

model = GPT2LMHeadModel.from_pretrained("decoder-only-transformer/trained_model")

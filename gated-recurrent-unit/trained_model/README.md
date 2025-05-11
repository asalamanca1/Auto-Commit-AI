# trained_model/

This folder contains the saved weights for the **GRU-based model** used to generate Git commit messages from code diffs.

---

## Files

- `gru_model.pt`  
  → PyTorch model checkpoint file containing the trained GRU model's weights.

---

## How to Get the File

[Download GRU Model Weights](https://minersutep-my.sharepoint.com/:f:/g/personal/asalamanca1_miners_utep_edu/EgOSW8RSa4xKhyKJ6hLoZRgBkdQvNwxkBIUiWCBYIvwcGw?e=K2BH17)

After downloading, place the file in this folder


---

## When to Use This Folder

Only needed if you're:
- Running **inference** using `Inference_for_Git_Commit_GRU.ipynb`
- Running **evaluation** with the pre-trained GRU model

**If you're training a new GRU model from scratch**, you can ignore or replace the file in this folder.

---

## Format

This file is saved using the standard PyTorch format. You can load it with:

```python
import torch
from model import GRULanguageModel  # adjust this import based on your code structure

model = GRULanguageModel(...)
model.load_state_dict(torch.load("trained_model/gru_model.pt", map_location="cpu"))
model.eval()



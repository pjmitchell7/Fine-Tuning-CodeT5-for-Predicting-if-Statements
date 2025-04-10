# README: Homework 2 — Fine-Tuning CodeT5 for Masked If-Condition Prediction

**Author**: Paul Mitchell  
**Class**: Generative AI for Software Development (CSCI 520)  
**Model**: `Salesforce/CodeT5-small`  
**Dataset Size**: 50,000 training / 5,000 validation / 5,000 test  
**Google Drive Folder**: [Project Drive Link](https://drive.google.com/drive/folders/190P9D_b6VcJMwnMvWUCljcETXmWAz7CQ?usp=sharing)

All code, data splits, and results are saved under this folder:  
`/content/drive/MyDrive/Gen_AI_Homework_2/`

---

## 📁 File Structure

### 1. `Preprocessing_HW2.ipynb`
This notebook covers the full data pipeline used to create the fine-tuning dataset:
- Clones and parses thousands of public GitHub repos.
- Extracts methods with if statements (up to 120,000 samples).
- Masks the first if condition inside the method body with `<mask>:`.
- Filters and cleans methods to form the final dataset.
- Saves the results to Google Drive as:
  - `cleaned_dataset.csv`
  - `train.csv`, `val.csv`, and `test.csv` (60k split: 50k / 5k / 5k)

---

### 2. `CodeT5_HW2.ipynb`
This is the training and evaluation notebook, adapted from the original `CodeT5_CTransl.ipynb`. Steps included:
- Mounts Drive and loads the split datasets.
- Loads the pretrained `CodeT5-small` model and tokenizer.
- Adds the `<mask>` token.
- Fine-tunes the model for 7 epochs using Hugging Face `Trainer`.
- Tracks training and validation loss via **Weights & Biases** (W&B used but not required for grading).
- Generates predictions for the test set.
- Evaluates predictions using:
  - **BLEU (SacreBLEU)**
  - **F1 Score (micro-average)**
  - **Exact Match**

---

### Key Files
- `train.csv`, `val.csv`, `test.csv` — cleaned and split datasets  
- `testset_predictions.csv` — raw input/prediction/ground truth results  
- `testset-results.csv` — final formatted submission file including:
  - Input function with masked if condition
  - Whether the prediction is correct (Exact Match)
  - Expected if condition
  - Predicted if condition
  - BLEU-4 prediction score
  - CodeBLEU score (`"N/A"` due to runtime compatibility issues)

---

## 📊 Final Results

| Metric       | Value |
|--------------|--------|
| **BLEU**     | 75.98 |
| **F1 Score** | 49.32 |
| **Exact Match** | 33.62 |

---

## Notes

- CodeBLEU was not included in the final scoring due to a `tree-sitter` dependency issue and broken packages.
- If working properly, CodeBLEU would have been used with rebalanced syntax weights to reflect structural correctness over token-level similarity.

---

## Reproducing the Experiment

To replicate this project end-to-end in **Google Colab** (with some local setup notes):

### Upload the project notebooks to your Google Drive:
- `Preprocessing_HW2.ipynb` – for dataset collection and formatting
- `CodeT5_HW2.ipynb` – for fine-tuning the model and generating results

### Open `CodeT5_HW2.ipynb` in Colab, then:
1. **Mount your Google Drive**  
2. **Set working directory to**:  
   `/content/drive/MyDrive/Gen_AI_Homework_2/`

---

### Note on Preprocessing:
- The first two code blocks of `Preprocessing_HW2.ipynb` (used to split `cleaned_dataset.csv` into `train.csv`, `val.csv`, and `test.csv`) were run on **Google Colab**.
- However, the remaining data collection and extraction code blocks were executed **locally**, due to their runtime and file system demands.
- As such, full data collection is **not one-to-one replicable** within Colab.

---

### Run the notebook cells in order:
- Use the pre-saved training/validation/test splits to fine-tune the model.
- Generate predictions on the test set.
- Evaluate results using BLEU, F1 Score, and Exact Match.

---

### Outputs should be saved to your Drive:
- `testset-results.csv` — formatted evaluation results  
- `testset_predictions.csv` — raw inputs/predictions/targets  
- Checkpoints, logs, and any additional outputs (e.g., W&B tracking)

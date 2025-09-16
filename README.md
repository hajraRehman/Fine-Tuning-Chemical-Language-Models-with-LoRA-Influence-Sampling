# Fine-Tuning Chemical Language Models with LoRA & Influence Sampling  
> *Parameter-Efficient Learning for Scientific Domains — NNTI Course Project, WS 2024/2025*  
> *By Hafiza Hajrah Rehman, Faria Alam, Muhammad Sabeeh Rehman*

[![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-1.13+-red?logo=pytorch)](https://pytorch.org)
[![Hugging Face](https://img.shields.io/badge/Hugging%20Face-Transformers-orange?logo=huggingface)](https://huggingface.co)
[![LoRA](https://img.shields.io/badge/LoRA-Parameter--Efficient-green)](https://arxiv.org/abs/2106.09685)

---

## 🎯 Project Overview

Fine-tuned the **MolFormer** chemical language model (pretrained on PubChem/ZINC) for **lipophilicity prediction** using three advanced strategies:

1. **Masked Language Modeling (MLM)** → Domain adaptation using SMILES strings  
2. **Influence Function-Based Data Selection** → Curated 70 high-impact external samples to boost performance  
3. **Parameter-Efficient Fine-Tuning (PEFT)** → Compared **LoRA, BitFit, iA3** vs full fine-tuning  

Demonstrated that **LoRA strikes the best balance** between performance and efficiency, while **influence scores require careful tuning** to avoid overestimation.

---

## 📊 Key Results

### 🧬 Influence Sampling (Task 2)
- Selected **top 70 positively influential samples** using influence functions (LiSSA approximation)  
- Achieved **synergistic performance boost** vs base model:  
  - **RMSE reduced by 12%**  
  - **R² increased from 0.64 → 0.68**  
- ⚠️ Top 100 *negatively* scored samples outperformed top 50 *positively* scored — highlighting sensitivity of influence estimation

### 🎯 Data Selection Strategies (Task 3a)
| Method              | RMSE   | R²     | Spearman ρ |
|---------------------|--------|--------|-------------|
| **Diversity Sampling** | 0.6939 | 0.6801 | 0.8260      |
| Stratified Sampling | 0.7006 | 0.6726 | 0.8094      |
→ **Diversity sampling** slightly outperformed due to better generalization from varied SMILES patterns.

### ⚙️ Parameter-Efficient Tuning (Task 3b)
| Method       | Trainable Params | R²   | RMSE  | Spearman ρ | Training Time |
|--------------|------------------|------|-------|-------------|---------------|
| Full FT      | 44.7M (100%)     | 0.72 | 0.65  | 0.85        | ⏳ Longest    |
| **LoRA**     | 885K (1.95%)     | 0.65 | 0.68  | 0.83        | ⏱️ Moderate   |
| BitFit       | 370K (0.82%)     | 0.55 | 0.75  | 0.78        | ⏱️ Fast       |
| iA3          | 295K (0.66%)     | 0.50 | 0.80  | 0.75        | ⏱️ Fastest    |

✅ **LoRA is the practical winner** — near-full-FT performance with 98% fewer parameters.  
⚠️ **iA3 is fastest but least accurate** — suitable only when speed >> accuracy.

---

## 🧰 Project Structure & Setup

```
chem-llm-lora/
├── Task1.ipynb           # Masked LM + Base Fine-tuning
├── Task2.ipynb           # Influence Function Scoring + Data Selection
├── Task3.ipynb
├── requirements.txt      # Dependencies for Task 3
│── data/                 # Selected external samples (from Task 2)
├── models/               # Saved models (Task 1 & 2)
├── External_data_with_influence_score           
└── README.md
```

---

## 🚀 How to Run

### ✅ Task 1: Masked LM & Base Fine-tuning
```bash
# Run in Colab or Jupyter
jupyter notebook Task1.ipynb
```
> Installs dependencies, trains MolFormer with MLM, saves models to `./models/`

---

### ✅ Task 2: Influence Score Calculation
```bash
# MUST run Task1.ipynb first!
jupyter notebook Task2.ipynb
```
> Computes influence scores for 300 external samples, saves top samples and scores to `./results/` and `./data/`

---

### ✅ Task 3: Parameter-Efficient Fine-Tuning
```bash
# Create new environment (recommended)
python -m venv nnti-env
source nnti-env/bin/activate  # Linux/Mac
# nnti-env\Scripts\activate   # Windows

# Install dependencies
pip install -r Task3/requirements.txt

# Run training (e.g., LoRA)
cd Task3
python train.py --method lora --data_path ../data/selected_samples.csv

# Other methods: --method bitfit, --method ia3, --method full
```

> ✅ Ensure `Task3/data/` contains selected samples from Task 2.

---

## 📚 Dataset

- **Primary**: MoleculeNet Lipophilicity (4,200 SMILES strings + normalized logP values)  
- **External**: 300 additional SMILES samples for influence-based selection  
- **Source**: [Hugging Face - MoleculeNet Lipophilicity](https://huggingface.co/datasets/scikit-fingerprints/MoleculeNet_Lipophilicity)

---

## 👩‍💻 About Me

**Hafiza Hajrah Rehman**  
M.Sc. Data Science & AI @ Saarland University  
Specializing in Trustworthy AI, Adversarial ML, Model Interpretability  
🐙 [GitHub](https://github.com/hajraRehman) | ✉️ hafizahajra6@gmail.com

---

📄 **View Full Report PDF**: [NNTI_2025_project_report.pdf](NNTI_2025_project_report.pdf)

_Last updated: Sept 2025_

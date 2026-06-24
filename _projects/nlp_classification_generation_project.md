---
layout: page
title: NLP — Classification & Title Generation
description: Fine-tuning pretrained language models for fake news detection and article title generation
img: assets/img/NLP-image-couv.jpg
importance: 4
category: academic
github: https://github.com/mahamat9/Intro-NLP
---

## Overview

**Type**: Academic project — Conference/Workshop
**Supervisor**: Batcouzé I.
**Duration**: February 2025

---

## Context

Two complementary NLP tasks exploring both **supervised classification** and **generative** capabilities of modern language models.

| Task                         | Model                 | Dataset                | Metric           |
| ---------------------------- | --------------------- | ---------------------- | ---------------- |
| **Fake News Classification** | Custom CNN + Word2Vec | ~45k articles (Kaggle) | Accuracy, F1     |
| **Title Generation**         | T5-small (fine-tuned) | TitleGen (Kaggle)      | ROUGE-1, ROUGE-2 |

Both were carried out as part of the Batcouzé I. conference-workshop, February 2025.

---

## Part I — Fake News Classification

### Dataset

**~45,000 articles** labeled `FAKE` / `REAL` from [Kaggle Fake News Dataset](https://www.kaggle.com/datasets/ruudseven/fake-news-detection/data).

### Pipeline

```
Text → BPE Tokenizer (25k vocab) → Word2Vec Embedding (100d) → Classifier
```

- **BPE** : Learned subword segmentation from scratch
- **Word2Vec** : Skip-gram model (Gensim, `vector_size=100`, `window=5`, `min_count=2`)
- **Classifier** : 1D CNN or Linear classifier on pooled embeddings

### Two Architectures

**Mean Pooling :**

$$h = \frac{1}{T}\sum_{t=1}^{T} e_t$$

**L2-Norm Pooling :**

$$h = \frac{1}{T}\sum_{t=1}^{T} e_t \bigg/ \bigg\|\frac{1}{T}\sum_{t=1}^{T} e_t\bigg\|_2$$

<details>
<summary>CNN Classifier snippet</summary>

```python
class CNNClassifier(nn.Module):
    def __init__(self, vocab_size, embed_dim=100, num_classes=2):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, embed_dim, padding_idx=0)
        self.conv = nn.Conv1d(embed_dim, 256, kernel_size=3, padding=1)
        self.fc = nn.Linear(256, num_classes)

    def forward(self, x):
        x = self.embedding(x)                          # (B, T, E)
        x = x.permute(0, 2, 1)                         # (B, E, T)
        x = F.relu(self.conv(x))                       # (B, 256, T)
        x = F.adaptive_max_pool1d(x, 1).squeeze(-1)    # (B, 256)
        return self.fc(x)
```

</details>

### Training Configuration

| Hyperparameter   | Value                 |
| ---------------- | --------------------- |
| Optimizer        | Adam                  |
| Learning Rate    | `1e-3`                |
| Batch Size       | `64`                  |
| Epochs           | `15`                  |
| Early Stopping   | Patience = `6` epochs |
| Max Token Length | `256` tokens          |

### Visualizations

- Training / Validation loss curves
- Confusion matrix
- Per-class precision / recall / F1 bar chart
- t-SNE of learned Word2Vec embeddings

---

## Part II — Title Generation with T5

### Dataset

**~7,000 article-title pairs** — titles used as ground-truth targets for generation.

### Model

T5 adopts a **text-to-text** paradigm: both inputs and outputs are raw text strings.

```
Input:  "Generate a title: The article body text here..."
Output: "Predicted Article Title"
```

```
Input: "Generate a title: <article text>"
         |
    ┌─────────────────┐
    │  T5 Encoder     │  →  contextualized representations
    └─────────────────┘
         |
    ┌─────────────────┐
    │  T5 Decoder     │  →  auto-regressive token generation
    └─────────────────┘
         |
Output: "<generated title>"
```

| Property       | Value                           |
| -------------- | ------------------------------- |
| Architecture   | T5-small encoder-decoder        |
| Parameters     | ~60M                            |
| Pre-trained    | `google/t5-small` (HuggingFace) |
| Special Prefix | `"Generate a title: "`          |

### Training Configuration

| Hyperparameter     | Value  |
| ------------------ | ------ |
| Optimizer          | Adam   |
| Epochs             | `15`   |
| Learning Rate      | `1e-4` |
| Batch Size         | `4`    |
| Beam Size          | `4`    |
| Repetition Penalty | `2.5`  |

### Deployment

Model on HuggingFace Hub: [Ivanhoe9/finetune_T5_small_title_generation_NLP_cours](https://huggingface.co/Ivanhoe9/finetune_T5_small_title_generation_NLP_cours)

---

## Results Summary

| Task                        | Accuracy | F1       | ROUGE-1  | ROUGE-2  |
| --------------------------- | -------- | -------- | -------- | -------- |
| Fake News Classification    | **0.92** | **0.91** | —        | —        |
| Title Generation (T5-small) | —        | —        | **0.42** | **0.18** |

---

## Tools & Stack

| Tool                         | Role                                |
| ---------------------------- | ----------------------------------- |
| **PyTorch**                  | Model implementation & training     |
| **HuggingFace Transformers** | T5-small, tokenizers, trainer       |
| **Gensim**                   | Word2Vec skip-gram pretraining      |
| **tokenizers**               | Custom BPE tokenizer (from scratch) |
| **TensorBoard**              | Training metrics visualization      |
| **pandas / NumPy**           | Data loading & preprocessing        |
| **scikit-learn**             | Metrics (F1, confusion matrix), PCA |
| **matplotlib / seaborn**     | Plots                               |

---

<div style="display:flex; gap:1rem; justify-content:center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Intro-NLP" class="btn btn-primary" role="button" target="_blank">View on GitHub</a>
</div>

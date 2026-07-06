---
layout: page
title: NLP — Classification & Generation
description: Applied NLP pipeline combining supervised misinformation classification and transformer-based abstractive title generation.
img: assets/img/NLP-image-couv.jpg
importance: 4
category: academic
scientific_category: applied-ml-dl
github: https://github.com/mahamat9/Intro-NLP
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type**: Practical coursework during Conference/Workshop  
**Author:** <u>M. Mahamat</u>  
**Duration**: February 2025

> Two complementary tasks: binary classification of misinformation, and abstractive
> title generation from long-form articles.

---

## Context

Two complementary NLP tasks exploring both **supervised classification** and **generative** capabilities of modern language models.

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Task</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Model</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Dataset</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Metrics</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Fake News Classification</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Custom CNN + Word2Vec</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">~45 k articles (Kaggle)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Accuracy, F1</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Title Generation</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">T5-small (fine-tuned)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">TitleGen (Kaggle)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">ROUGE-1, ROUGE-2</td>
    </tr>
  </tbody>
</table>

Both were carried out as part of the Batcouzé I. conference-workshop, February 2025.

---

## Part I — Fake News Classification

### Dataset

**~45,000 articles** labeled `FAKE` / `REAL` from [Kaggle Fake News Dataset](https://www.kaggle.com/datasets/ruudseven/fake-news-detection/data).

### Pipeline

$$
\begin{array}{c}
\boxed{\text{Text Input } x \in \mathbb{R}^{n_{\text{tokens}}}} \\
\downarrow \\
\boxed{\text{BPE Tokenizer (25k vocab)}} \\
\downarrow \\
\boxed{\text{Word2Vec Embedding (100d)}} \\
\downarrow \\
\boxed{\text{Classifier}} \\
\downarrow \\
\boxed{\hat{y} \in \{0, 1\}} \quad \text{(Real / Fake)}
\end{array}
$$

- **BPE** : Learned subword segmentation from scratch
- **Word2Vec** : Skip-gram model (Gensim, `vector_size=100`, `window=5`, `min_count=2`)
- **Classifier** : 1D CNN or Linear classifier on pooled embeddings

### Two Architectures

**Mean Pooling :**

$$h = \frac{1}{T}\sum_{t=1}^{T} e_t$$

**L2-Norm Pooling :**

$$h = \frac{1}{T}\sum_{t=1}^{T} e_t \bigg/ \bigg\|\frac{1}{T}\sum_{t=1}^{T} e_t\bigg\|_2$$

### Hyperparameters

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Parameter</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Optimizer</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Adam</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Learning Rate</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>1e-3</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Batch Size</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>64</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Epochs</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>15</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Early Stopping</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Patience = <code>6</code> epochs</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Max Sequence Length</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>256</code> tokens</td>
    </tr>
  </tbody>
</table>

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

#### Architecture

$$
\begin{array}{c}
\boxed{\text{Input: } \text{"Generate a title: "} + \text{article}} \\
\downarrow \\
\boxed{\text{Tokenizer}} \\
\downarrow \\
\boxed{\text{T5-small Encoder}} \quad \text{(contextualized representations)} \\
\downarrow \\
\boxed{\text{T5-small Decoder}} \quad \text{(auto-regressive generation)} \\
\downarrow \\
\boxed{\text{Beam Search}(k=4)} \quad \text{(select top-4 candidates)} \\
\downarrow \\
\boxed{\text{Output: generated title}}
\end{array}
$$

#### Model Configuration

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Property</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Architecture</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">T5-small encoder–decoder</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Parameters</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">~60M</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Pre-trained Checkpoint</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>google/t5-small</code> (HuggingFace)</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Task Prefix</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>"Generate a title: "</code></td>
    </tr>
  </tbody>
</table>

### Fine-tuning Hyperparameters

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Parameter</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Optimizer</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Adam</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Learning Rate</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>1e-4</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Batch Size</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>4</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Epochs</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>15</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Beam Size</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>4</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Repetition Penalty</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>2.5</code></td>
    </tr>
  </tbody>
</table>

### Deployment

Model on HuggingFace Hub: [Ivanhoe9/finetune_T5_small_title_generation_NLP_cours](https://huggingface.co/Ivanhoe9/finetune_T5_small_title_generation_NLP_cours)

---

## Results Summary

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Task</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Accuracy</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">F1</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">ROUGE-1</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">ROUGE-2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Fake News Classification</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>0.92</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>0.91</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">—</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">—</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Title Generation</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">—</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">—</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>0.42</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>0.18</strong></td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>

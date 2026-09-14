---
layout: page
title: Diffusion Generative Models #project 1
#description: Research thesis on stochastic foundations and implementation of denoising diffusion probabilistic models for generative learning.
description: Master's thesis on the stochastic foundations of denoising diffusion probabilistic models, with implementation and comparative evaluation against GANs and VAEs.
img: assets/img/diffusion_illustration.png
importance: 1
category: Academic
scientific_category: applied-ml-dl
#related_publications: true
github: https://github.com/mahamat9/diffusion_model_M2DSUA
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type:** Master 2 Research Thesis  
**Authors:** <u>M. Mahamat</u> · T. Gipteau · A. Gonin  
**Supervisor:** [Pr. Fabien Panloup](https://blog.univ-angers.fr/panloup/) — University of Angers, LAREMA  
**Duration**: November 2024 – March 2025

---

Diffusion models have become the dominant paradigm in generative modelling, yet their success rests on a mathematical construction that is rarely made explicit in applied work: a **stochastic process that progressively destroys data, and a learned reversal of that process**.

## Theoretical study

The forward process is formulated as a **stochastic differential equation (SDE)**, whose solution progressively transforms the data distribution into a Gaussian distribution. Generation amounts to integrating the **time-reversed SDE**, which requires only one unknown quantity: the **score** $$\nabla_x \log p_t(x)$$ of the intermediate distributions.

The thesis follows this thread explicitly:

- **Forward diffusion** as an SDE, and its discrete DDPM counterpart
- **Time reversal**, and why the reverse process remains a diffusion
- **Score matching**, and how the intractable score is estimated by denoising; the step that turns an abstract SDE into a trainable neural objective

This is where the theoretical interest lies: the loss actually minimised in practice (predicting the injected noise) is not an approximation of convenience, but the direct consequence of the reversal formula.

---

## Experimental comparison

Diffusion, GAN, and VAE models were implemented in **PyTorch** and trained on **MNIST** under comparable budgets. Evaluation focused on a question that visual inspection cannot settle: **do the models cover the data distribution, or only part of it?**

A generative model can produce sharp, convincing samples while silently ignoring entire regions of the data distribution, a failure known as **mode collapse**. On MNIST this is directly measurable: a model that has collapsed will generate the digits it finds easiest far more often than the rest, or omit some classes altogether. Since the training set is absolutely uniform across the ten digits, any departure from uniformity in the generated samples is a symptom rather than a property of the data.

Two complementary measures were used:

- **Class coverage** : the empirical distribution of generated digits over the ten classes, obtained by labelling a large batch of samples with an independently trained classifier, then compared to the uniform reference.
- **Mean Max Logit (MML)** : the average confidence of that same classifier on the generated samples, used as a proxy for how recognisable each sample is.

The two are deliberately paired: MML alone rewards a model that produces one perfect digit endlessly,
while coverage alone rewards a model that spreads uniformly over unrecognisable noise. Read together, they
separate _fidelity_ from _diversity_.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ter_generative_class_coverage.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Distribution of generated samples across the ten MNIST classes for each model. Deviations from uniformity reveal partial mode collapse.
</div>

The comparison confirmed the expected qualitative picture, diffusion producing the most balanced class coverage, GANs the sharpest but least diverse samples, at a **significantly higher sampling cost**, given that generation requires multiple sequential network evaluations rather than just one.

<!-- ici je veux un img illustrant la couverture de classe -->

---

## Scope and limitations

The experiments are deliberately confined to **MNIST**, at a scale that runs with few resources. This is sufficient to observe mode collapse and coverage effects, but not to draw conclusions about high-resolution image synthesis, where architectural choices dominate.

The contribution of this work is therefore **pedagogical and methodological** rather than novel: a self-contained derivation from SDE to training objective, paired with a controlled experiment that makes the trade-offs between the three families measurable rather than anecdotal.

---

## Takeaway

Working through diffusion models from their stochastic formulation clarified something that carries beyond this project: **the training objective of a generative model is inherited from its probabilistic assumptions**, not chosen freely. This is the perspective I have since tried to keep when using generative methods on real data.

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
  <a href="https://github.com/mahamat9/diffusion_model_M2DSUA/blob/main/rapport/TER_Diffusion_Model.pdf"
     class="btn btn-secondary" role="button" target="_blank">
    Read the report
  </a>
</div>

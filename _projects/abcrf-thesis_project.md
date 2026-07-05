---
layout: page
title: ABC-RF : Approximate Bayesian Computation via Random Forests
description: Statistical inference project on likelihood-free Bayesian estimation with Random Forest summaries for genomic applications.
img: assets/img/ma-2_abc.png
importance: 9
category: academic
scientific_category: statistics
github: https://github.com/mahamat9/Projet-de-recherche-ABC-RF
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** January – May 2024  
**Supervisors:** Charles-Elie Rabier — University of Angers  
**Authors:** <u>M. Mahamat</u>, M. Charbonneau, R. Jaffal  
**Type:** Master 1 Research Thesis  

---

## Context

Bayesian model selection in population genetics requires computing posterior probabilities over competing demographic scenarios, a task that is **analytically intractable** when the likelihood function is unavailable or too costly to evaluate.

**Approximate Bayesian Computation (ABC)** bypasses this by replacing likelihood evaluation with **simulation-based comparison** of summary statistics. The **ABC-RF** extension (Pudlo et al., 2015 ; Raynal et al., 2019) further replaces the rejection step with a **Random Forest**, achieving superior accuracy and scalability.

---

## Mathematical Framework

#### ABC

Let $\theta$ be the parameter of interest, $y_\text{obs}$ the observed data, and $s(\cdot)$ a summary statistic. ABC approximates:

$$
\pi(\theta \mid y_\text{obs}) \approx
\pi\!\left(\theta \mid s(y) \approx s(y_\text{obs})\right)
$$

by drawing $$(\theta^{(i)}, y^{(i)}) \sim \pi(\theta)\,p(y\mid\theta)$$
and retaining simulations where
$$\|s(y^{(i)}) - s(y_\text{obs})\| \leq \varepsilon.$$

#### From ABC to ABC-RF

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Step</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Classic ABC</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">ABC-RF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Model selection</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Rejection + ratio</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Random Forest classifier</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Parameter estimation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Weighted quantiles</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">RF regression + local weights</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Tolerance \(\varepsilon\)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Required</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Not needed</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Reference table</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Discarded</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Full table used</td>
    </tr>
  </tbody>
</table>

The RF is trained on a **reference table**
$$\mathcal{T} = \{(m^{(i)},\, s(y^{(i)}))\}_{i=1}^{N}$$
where $m^{(i)}$ is the simulated model index.  
The **posterior probability** of model $m$ is estimated via the proportion of trees voting for $m$ in the forest.

---

## Applications

#### 1 - Reproduction of Pudlo et al. (2015)

We replicated the **human demographic scenario selection** benchmark from the founding paper using the R package `abcrf`:

- Simulated reference table under competing demographic models
- Trained a RF classifier on summary statistics
- Compared posterior error rates against the original paper

#### 2 - Asian rice evolutionary history

Using **phylogenetic networks** and genomic data from _Oryza sativa_ subspecies:

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Task</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Method</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Scenario encoding</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Summary statistics on allele frequency spectra</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Model selection</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>abcrf::abcrf()</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Posterior estimation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;"><code>abcrf::postpr()</code></td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Network visualisation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Phylogenetic network reconstruction</td>
    </tr>
  </tbody>
</table>

---

### Reproducibility

This work emphasizes **reproducible research**:

- All simulations seeded and version-controlled
- R scripts modular and documented
- Results traceable from raw data to final figures

---

### Tools & Methods

- **Language:** R
- **Key package:** `abcrf`
- **Simulation:** custom R scripts
- **Phylogenetics:** phylogenetic network tools

---

#### Key References

> - Pudlo, P. et al. (2015). _Reliable ABC model choice via random forests._ **Bioinformatics**, 32(6), 859–866.
> - Raynal, L. et al. (2019). _ABC random forests for Bayesian parameter inference._ **Bioinformatics**, 35(10), 1720–1728.

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>
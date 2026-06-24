---
layout: page
title: ABC-RF — Approximate Bayesian Computation via Random Forests
description: Mathematical foundations and genomic applications of ABC-RF — Master 1 research thesis
img: assets/img/ma-2_abc.png
importance: 8
category: academic
github: https://github.com/mahamat9/Projet-de-recherche-ABC-RF
---

## Overview

**Duration:** January – May 2024  
**Supervisors:** Charles-Elie Rabier — Université d'Angers  
**Co-authors:** M. Charbonneau, R. Jaffal  
**Type:** Master 1 Research Thesis  
**Stack:** R · `abcrf` · phylogenetic networks · reproducible research

---

## Context

Bayesian model selection in population genetics requires computing posterior probabilities over competing demographic scenarios, a task that is **analytically intractable** when the likelihood function is unavailable or too costly to evaluate.

**Approximate Bayesian Computation (ABC)** bypasses this by replacing likelihood evaluation with **simulation-based comparison** of summary statistics. The **ABC-RF** extension (Pudlo et al., 2015 ; Raynal et al., 2019) further replaces the rejection step with a **Random Forest**, achieving superior accuracy and scalability.

---

## Mathematical Framework

### ABC

Let $\theta$ be the parameter of interest, $y_\text{obs}$ the observed data, and $s(\cdot)$ a summary statistic. ABC approximates:

$$
\pi(\theta \mid y_\text{obs}) \approx
\pi\!\left(\theta \mid s(y) \approx s(y_\text{obs})\right)
$$

by drawing $$(\theta^{(i)}, y^{(i)}) \sim \pi(\theta)\,p(y\mid\theta)$$
and retaining simulations where
$$\|s(y^{(i)}) - s(y_\text{obs})\| \leq \varepsilon.$$

### From ABC to ABC-RF

| Step                    | Classic ABC        | ABC-RF                        |
| ----------------------- | ------------------ | ----------------------------- |
| Model selection         | Rejection + ratio  | Random Forest classifier      |
| Parameter estimation    | Weighted quantiles | RF regression + local weights |
| Tolerance $\varepsilon$ | Required           | Not needed                    |
| Reference table         | Discarded          | Full table used               |

The RF is trained on a **reference table**
$$\mathcal{T} = \{(m^{(i)},\, s(y^{(i)}))\}_{i=1}^{N}$$
where $m^{(i)}$ is the simulated model index.  
The **posterior probability** of model $m$ is estimated via the proportion of trees voting for $m$ in the forest.

---

## Applications

### 1 - Reproduction of Pudlo et al. (2015)

We replicated the **human demographic scenario selection** benchmark from the founding paper using the R package `abcrf`:

- Simulated reference table under competing demographic models
- Trained a RF classifier on summary statistics
- Compared posterior error rates against the original paper

### 2 - Asian rice evolutionary history

Using **phylogenetic networks** and genomic data from _Oryza sativa_ subspecies:

| Task                  | Method                                         |
| --------------------- | ---------------------------------------------- |
| Scenario encoding     | Summary statistics on allele frequency spectra |
| Model selection       | `abcrf::abcrf()`                               |
| Posterior estimation  | `abcrf::postpr()`                              |
| Network visualisation | Phylogenetic network reconstruction            |

---

## Reproducibility

This work emphasizes **reproducible research**:

- All simulations seeded and version-controlled
- R scripts modular and documented
- Results traceable from raw data to final figures

---

## Key References

- Pudlo, P. et al. (2015). _Reliable ABC model choice via random forests._
  **Bioinformatics**, 32(6), 859–866.
- Raynal, L. et al. (2019). _ABC random forests for Bayesian parameter
  inference._ **Bioinformatics**, 35(10), 1720–1728.

---

## Tools & Stack

- **Language:** R
- **Key package:** `abcrf`
- **Simulation:** custom R scripts
- **Phylogenetics:** phylogenetic network tools
- **Version control:** Git / GitHub

---

<div style="display:flex; gap:1rem; justify-content:center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Projet-de-recherche-ABC-RF"
     class="btn btn-primary" role="button" target="_blank">
    View on GitHub
  </a>
</div>

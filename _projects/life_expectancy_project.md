---
layout: page
title: Econometric Analysis of Life Expectancy Drivers
description: Multi-method econometric analysis identifying key determinants of global life expectancy across 193 countries (2000–2015)
img: assets/img/econometrics_illus.png
importance: 6
category: academic
github: https://github.com/mahamat9/Econometric-Life-Expectancy
---

<p align="center">
  <img src="https://img.shields.io/badge/Python-3-blue?logo=python"/>
  <img src="https://img.shields.io/badge/R-grey?logo=r" />
  <img src="https://img.shields.io/badge/Econometrics-OLS%20%7C%20LME%20%7C%20IV%20%7C%20PSM-blue" />
  <img src="https://img.shields.io/badge/Topic-Life%20Expectancy-red" />
</p>

---

## Overview

| Field            | Detail                                                                     |
| ---------------- | -------------------------------------------------------------------------- |
| **Type**         | Academic project — End of course                                           |
| **Course**       | Méthodes Économétriques (C. Daniel) · M2 Data Science, Université d'Angers |
| **Authors**      | Mahamat-Nour Bachar Mahamat                                                |
| **Duration**     | March – June 2025                                                          |
| **Deliverables** | Report (PDF) · Python notebook · R notebook                                |

---

## Context

Life expectancy is one of the most fundamental indicators of human development — reflecting simultaneously the state of a population's health, access to care, and socioeconomic conditions. This project uses WHO Global Health Observatory data across **193 countries over 2000–2015** (≈2 938 observations, 22 variables) to empirically identify the structural drivers of longevity, deploying a rigorous multi-method econometric framework that addresses endogeneity, omitted variable bias, and selection effects.

---

## Dataset

| Feature            | Detail                                                             |
| ------------------ | ------------------------------------------------------------------ |
| **Source**         | WHO Global Health Observatory (GHO) · United Nations Economic Data |
| **Coverage**       | 193 countries, 2000–2015 (16-year panel)                           |
| **Observations**   | ~2 938 rows × 22 variables                                         |
| **Missing values** | Imputed by country mean (then global mean fallback)                |

**Variable categories:**

| Category           | Examples                                                                   |
| ------------------ | -------------------------------------------------------------------------- |
| Immunization       | Hepatitis B, Polio, Diphtheria vaccine coverage (%)                        |
| Mortality          | Adult mortality rate, infant deaths, under-five deaths, HIV/AIDS           |
| Economic           | GDP per capita, total health expenditure (% GDP), Income Composition Index |
| Social / Lifestyle | Schooling (years), BMI, Alcohol consumption, Thinness prevalence           |

---

## Methods

Four complementary econometric approaches were applied sequentially, each addressing a different source of bias or assumption violation.

### 1 · Ordinary Least Squares (OLS)

A baseline linear regression of life expectancy on all predictors, with polynomial feature expansion. A custom exhaustive search tested all degree-1 and degree-2 combinations to select the optimal model via **AIC/BIC**.

> **Key finding:** GDP and Schooling are the dominant drivers — but their effects are non-linear (saturating beyond a threshold). Adult mortality rate and HIV/AIDS carry the strongest negative loadings.

### 2 · Linear Mixed-Effects Model (LMM)

Panel data (countries × years) require a model that captures **group-level heterogeneity**. The LMM introduces random intercepts by country and year:

$$\text{LifeExp}_{it} = X_{it}^\top \beta + u_i + v_t + \varepsilon_{it}$$

_Fixed effects_ (β) estimate the average effect of each covariate; _random effects_ (u_i, v_t) absorb unobserved country/year heterogeneity. This dramatically reduces omitted-variable bias compared to pooled OLS.

> **Key finding:** After accounting for country-level heterogeneity, the GDP effect shrinks — confirming prior endogeneity. The Income Composition Index remains the most robust predictor.

### 3 · Instrumental Variables (IV)

GDP is **endogenous**: richer countries may invest more in health, but healthier populations also drive higher GDP (reverse causality). An **instrumental variable** (`GDP_noisy` = GDP + ε noise) is used, estimated via two-stage least squares (`ivreg` in R).

> **Key finding:** IV estimation reveals a larger GDP coefficient than naive OLS — confirming a downward bias from reverse causality. Education (Schooling) remains highly significant in all specifications.

### 4 · Propensity Score Matching (PSM)

Comparing developed vs. developing countries is confounded by many factors. **Propensity Score Matching** pairs each developed country with a closest developing country on observed characteristics (GDP, Schooling, Income Composition), then estimates the average treatment effect (ATE) of development status on life expectancy.

> **Key finding:** Matching reduces the raw developed/developing gap by ~3 years, isolating the true causal premium attributable to development status alone.

---

## Key Results

| Variable                 | OLS β          | Interpretation                                          |
| ------------------------ | -------------- | ------------------------------------------------------- |
| GDP per capita           | +0.00028 / USD | Positive but diminishing returns                        |
| Schooling                | +0.62 yrs      | Each additional school year ≈ +0.6 yr life expectancy   |
| Income Composition Index | +6.8 pts       | Strongest predictor; holistic measure of development    |
| Adult Mortality          | −0.027 /‰      | Strongest negative; proxy for healthcare system quality |
| HIV/AIDS                 | −0.44 /‰       | Devastating impact on population longevity              |
| Diphtheria Vaccine       | +0.038 /%      | Robust public health signal                             |

---

## Stack

```
Python          pandas · numpy · matplotlib · seaborn
                statsmodels · sklearn
R               ggplot2 · lme4 · ivreg · corrplot · MatchIt
                modelsummary · ggrepel
Visualization   Correlation matrix · Scatter plots · Coefficient bar charts
Report          LaTeX (memoir class) · A4, 31 pages
```

---

## GitHub Repository

<p align="center">
  <a href="https://github.com/mahamat9/Econometric-Life-Expectancy">
    <img src="https://img.shields.io/badge/GitHub-View%20Repository-grey?logo=github" />
  </a>
</p>

_Contains: full report (PDF), Python notebook, R notebook, and data preprocessing scripts._

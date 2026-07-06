---
layout: page
title: Econometric Analysis of Life Expectancy Drivers
description: Panel-data econometric analysis of structural determinants of life expectancy across 193 countries over 2000-2015.
img: assets/img/econometrics_illus.png
importance: 7
category: academic
scientific_category: statistics
github: https://github.com/mahamat9/Econometric-Methods
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type:** Academic project  
**Course:** Méthodes Économétriques (C. Daniel)  
**Author:** <u>M. Mahamat</u>  
**Duration:** June 2025  
**Deliverables**: Report (PDF) · Python notebook · R notebook

---

## Context

Life expectancy is one of the most fundamental indicators of human development ; reflecting simultaneously the state of a population's health, access to care, and socioeconomic conditions. This project uses WHO Global Health Observatory data across **193 countries over 2000–2015** (22 variables) to empirically identify the structural drivers of longevity, deploying a rigorous multi-method econometric framework that addresses endogeneity, omitted variable bias, and selection effects.

---

## Dataset

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b; color:white;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Feature</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Detail</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Source</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">WHO Global Health Observatory (GHO) · United Nations Economic Data</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Coverage</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">193 countries, 2000–2015 (16-year panel)</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Missing values</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Imputed by country mean (then global mean fallback)</td>
    </tr>
  </tbody>
</table>

**Variable categories:**

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b; color:white;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Category</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Examples</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Immunization</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Hepatitis B, Polio, Diphtheria vaccine coverage (%)</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Mortality</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Adult mortality rate, infant deaths, under-five deaths, HIV/AIDS</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Economic</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">GDP per capita, total health expenditure (% GDP), Income Composition Index</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Social / Lifestyle</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Schooling (years), BMI, Alcohol consumption, Thinness prevalence</td>
    </tr>
  </tbody>
</table>

---

## Methods

Four complementary econometric approaches were applied sequentially, each addressing a different source of bias or assumption violation.

### 1 · Ordinary Least Squares (OLS)

A baseline linear regression of life expectancy on all predictors, with polynomial feature expansion. A custom exhaustive search tested all degree-1 and degree-2 combinations to select the optimal model via **AIC/BIC**.

> **Key finding:** GDP and Schooling are the dominant drivers — but their effects are non-linear (saturating beyond a threshold). Adult mortality rate and HIV/AIDS carry the strongest negative loadings.

### 2 · Linear Mixed-Effects Model (LMM)

Panel data (countries × years) require a model that captures **group-level heterogeneity**. The LMM introduces random intercepts by country and year:

$$\text{LifeExp}_{it} = X_{it}^\top \beta + u_i + v_t + \varepsilon_{it}$$

_Fixed effects_ ($\beta$) estimate the average effect of each covariate; _random effects_ (u$_i$, v$_t$) absorb unobserved country/year heterogeneity. This dramatically reduces omitted-variable bias compared to pooled OLS.

> **Key finding:** After accounting for country-level heterogeneity, the GDP effect shrinks — confirming prior endogeneity. The Income Composition Index remains the most robust predictor.

### 3 · Instrumental Variables (IV)

GDP is **endogenous**: richer countries may invest more in health, but healthier populations also drive higher GDP (reverse causality). An **instrumental variable** (`GDP_noisy` = GDP + $\epsilon$ noise) is used, estimated via two-stage least squares (`ivreg` in R).

> **Key finding:** IV estimation reveals a larger GDP coefficient than naive OLS — confirming a downward bias from reverse causality. Education (Schooling) remains highly significant in all specifications.

### 4 · Propensity Score Matching (PSM)

Comparing developed vs. developing countries is confounded by many factors. **Propensity Score Matching** pairs each developed country with a closest developing country on observed characteristics (GDP, Schooling, Income Composition), then estimates the average treatment effect (ATE) of development status on life expectancy.

> **Key finding:** Matching reduces the raw developed/developing gap by ~3 years, isolating the true causal premium attributable to development status alone.

---

## Key Results

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b; color:white;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Variable</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">OLS $\beta$</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Interpretation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>GDP per capita</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">+0.00028 / USD</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Positive but diminishing returns</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Schooling</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">+0.62 yrs</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Each additional school year ≈ +0.6 yr life expectancy</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Income Composition Index</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">+6.8 pts</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Strongest predictor; holistic measure of development</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Adult Mortality</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">−0.027 %</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Strongest negative; proxy for healthcare system quality</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>HIV/AIDS</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">−0.44 %</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Devastating impact on population longevity</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Diphtheria Vaccine</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">+0.038 %</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Robust public health signal</td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>

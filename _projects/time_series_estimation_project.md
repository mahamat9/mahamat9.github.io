---
layout: page
title: Estimation in Dependent Stationary Series
description: Asymptotic statistical study of estimator bias and variance in dependent stationary time-series under mixing assumptions.
img: assets/img/ts_estimation_illustration.png
importance: 8
category: academic
scientific_category: statistics
github: https://github.com/mahamat9/Time-Series-Estimation
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type**: Academic mini-project  
**Course**: Time Series — Master 2 Data Science, University of Angers  
**Author:** <u>M. Mahamat</u> · R. Jaffal · J. Malard  
**Duration**: December 2024  
**Deliverables**: Slides (PDF) · Notebook

---

## Context

Estimating parameters in dependent time series is non-trivial: under dependence, the **variance of the sample mean no longer scales as $1/n$** as in the i.i.d. case ; covariance terms between distant observations inflate it. The **$\alpha$-mixing framework** (Rosenblatt, 1956) provides the theoretical backbone to characterize this dependence and derive valid asymptotic results.

---

## Theory

### Weak Dependence & Mixing Conditions

**$\alpha$-mixing (strongly mixing):** For a stationary series $(X_t)_{t\in\mathbb{Z}}$, the $\alpha$-mixing coefficient is

$$
\alpha(n) = \sup_{j\in\mathbb{Z}} \alpha\big(\mathcal{F}_{-\infty}^j,\ \mathcal{F}_{j+n}^{\infty}\big), \quad \alpha(\mathcal{A},\mathcal{B}) = \sup_{A\in\mathcal{A}, B\in\mathcal{B}} |P(A\cap B) - P(A)P(B)|.
$$

A series is **$\alpha$-mixing** iff $\alpha(n) \to 0$ as $n\to\infty$. It is **m-dependent** if all events separated by more than $m$ time steps are independent — equivalently $\alpha(n) = 0$ for all $n > m$.

### Key Theorems

**DMR Condition** (Mokkadem, 1987): If

$$M_{2,\alpha}(Q_0) = \int_0^1 \alpha^{-1}(u)\, Q_0(u)^2\, du < \infty,$$

then $\mathrm{Var}(S_n) \leq 4n\,M_{2,\alpha}(Q_0)$ and $\mathrm{Var}(S_n)/n \to \sigma^2 \leq 4M_{2,\alpha}(Q_0)$, where $S_n = \sum_{i=1}^n X_i$.

**Generalized CLT under $\alpha$-mixing:** If $\alpha(n) = O(n^{-p})$ with $p>1$ and $\sqrt{n}(\bar{X}_n - \mu) \overset{d}{\to} \mathcal{N}(0, \sigma^2)$, then for any differentiable estimator $\hat{\theta}_n = g(\bar{X}_n)$:

$$\sqrt{n}\big(\hat{\theta}_n - g(\mu)\big) \overset{d}{\to} \mathcal{N}\big(0,\ \sigma^2\, g'(\mu)^2\big).$$

### Estimators Studied

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b; color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Estimator</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Expression</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Bias under dependence</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Asymptotic variance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Sample mean</strong> $\bar{X}_N$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$\frac{1}{N}\sum_i X_i$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$E[\bar{X}_N] = \mu$ (unbiased)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$$\dfrac{1}{N}\mathrm{Var}(X_0) + \dfrac{2}{N^2}\sum_{k=1}^{N-1}(N-k)\mathrm{Cov}(X_0,X_k)$$</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Sample variance</strong> $S_N^2$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$$\frac{1}{N}\sum_i (X_i-\bar{X}_N)^2$$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$$E[S_N^2] = \sigma^2 + O\left(\frac{1}{N}\sum_k \gamma_k\right)$$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$\approx \dfrac{1}{N^2}\sum_{j,i} \gamma_{j-i}^2$</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Quantiles</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$Q_\alpha = F^{-1}(\alpha)$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Affected by mixing strength</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Controlled via DMR condition on $\alpha(n)$</td>
    </tr>
  </tbody>
</table>

---

## Notebook — Simulation Experiments

- **AR(1) convergence:** Sample mean convergence of $\sqrt{N}(\bar{X}_N - \mu)$ vs. the $\mathcal{N}(0,1)$ standard normal; histogram overlay confirms CLT holds for AR(1) with $\phi=0.8$.
- **m-dependent series:** Generation and verification that autocorrelation drops to zero for lags $> m$; correlation check confirms approximate independence.
- **$\alpha$-mixing coefficients — AR(1):** Computed $\alpha(n)$ for AR(1) ($\phi=0.5$); exponential decay observed as $n$ increases.
- **$\alpha$-mixing coefficients — MA(1) & Gaussian noise:** MA(1) shows slower decay than AR(1); independent Gaussian noise yields $\alpha(n)\approx 0$ from the start.

---

## Key Results

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b; color:#fff;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Result</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Finding</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Sample mean CLT</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$\sqrt{N}(\bar{X}_N - \mu) \xrightarrow{d} \mathcal{N}\left(0, \sigma_\infty^2\right)$ under AR(1) dependence</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Variance bias</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$E[S_N^2] = \sigma^2 + O\big(\frac{1}{N}\sum \gamma_k\big)$ — negligible if autocorrelation decays fast</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>m-dependence</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$\alpha(n)=0$ for $n > m$ → DMR condition satisfied if $\mathbb{E}[X_0^2] < \infty$</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Mixing decay rates</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">AR(1): exponential; MA(1): polynomial; Gaussian noise: $\alpha$-mixing</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Mixing decay</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">AR(1) decays exponentially; MA(1) slower; Gaussian noise already $\alpha$-mixing</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Variance estimator consistency</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Requires $\sum_k \gamma_k^2 < \infty$; fails for long-memory processes</td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>
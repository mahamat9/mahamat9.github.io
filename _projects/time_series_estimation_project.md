---
layout: page
title: Estimation in Dependent Stationary Series
description: Bias and asymptotic variance of estimators (mean, variance, quantiles) under mixing conditions
img: assets/img/ts_estimation_illustration.png
importance: 7
category: academic
github: https://github.com/mahamat9/Time-Series-Estimation
---

<p align="center">
  <img src="https://img.shields.io/badge/Python-3-blue?logo=python"/>
  <img src="https://img.shields.io/badge/Time%20Series-Stationary%20Series-red"/>
</p>

---

## Overview

**Type**: Academic mini-project
**Course**: Time Series — Master 2 Data Science, Université d'Angers
**Co-authors**: R. Jaffal · J. Malard
**Duration**: December 2024
**Deliverables**: Slides (PDF) · Notebook

---

## Context

Estimating parameters in dependent time series is non-trivial: under dependence, the **variance of the sample mean no longer scales as $1/n$** as in the i.i.d. case — covariance terms between distant observations inflate it. The **α-mixing framework** (Rosenblatt, 1956) provides the theoretical backbone to characterize this dependence and derive valid asymptotic results.

---

## Theory

### Weak Dependence & Mixing Conditions

**α-mixing (strongly mixing):** For a stationary series $(X_t)_{t\in\mathbb{Z}}$, the α-mixing coefficient is

$$\alpha(n) = \sup_{j\in\mathbb{Z}} \alpha\big(\mathcal{F}_{-\infty}^j,\ \mathcal{F}_{j+n}^{\infty}\big), \quad \alpha(\mathcal{A},\mathcal{B}) = \sup_{A\in\mathcal{A}, B\in\mathcal{B}} |P(A\cap B) - P(A)P(B)|.$$

A series is **α-mixing** iff $\alpha(n) \to 0$ as $n\to\infty$. It is **m-dependent** if all events separated by more than $m$ time steps are independent — equivalently $\alpha(n) = 0$ for all $n > m$.

### Key Theorems

**DMR Condition** (Mokkadem, 1987): If

$$M_{2,\alpha}(Q_0) = \int_0^1 \alpha^{-1}(u)\, Q_0(u)^2\, du < \infty,$$

then $\mathrm{Var}(S_n) \leq 4n\,M_{2,\alpha}(Q_0)$ and $\mathrm{Var}(S_n)/n \to \sigma^2 \leq 4M_{2,\alpha}(Q_0)$, where $S_n = \sum_{i=1}^n X_i$.

**Generalized CLT under α-mixing:** If $\alpha(n) = O(n^{-p})$ with $p>1$ and $\sqrt{n}(\bar{X}_n - \mu) \overset{d}{\to} \mathcal{N}(0, \sigma^2)$, then for any differentiable estimator $\hat{\theta}_n = g(\bar{X}_n)$:

$$\sqrt{n}\big(\hat{\theta}_n - g(\mu)\big) \overset{d}{\to} \mathcal{N}\big(0,\ \sigma^2\, g'(\mu)^2\big).$$

### Estimators Studied

| Estimator | Expression | Bias under dependence | Asymptotic variance |
|---|---|---|---|
| **Sample mean** $\bar{X}_N$ | $\frac{1}{N}\sum_i X_i$ | $E[\bar{X}] = \mu$ | $\frac{1}{N}\mathrm{Var}(X_0) + \frac{2}{N^2}\sum_{k=1}^{N-1}(N-k)\mathrm{Cov}(X_0,X_k)$ |
| **Sample variance** $S_N^2$ | $\frac{1}{N}\sum_i (X_i-\bar{X})^2$ | $E[S_N^2] = \sigma^2 + \frac{2}{N^2}\sum\gamma_{j-i}$ | $\approx \frac{1}{N^2}\sum\gamma_{j-i}^2$ |
| **Quantiles** | $Q_\alpha = F^{-1}(\alpha)$ | Affected by mixing | Controlled via DMR |

---

## Notebook — Simulation Experiments

- **AR(1) convergence:** Sample mean convergence of $\sqrt{N}(\bar{X}_N - \mu)$ vs. the $\mathcal{N}(0,1)$ standard normal; histogram overlay confirms CLT holds for AR(1) with $\phi=0.8$.
- **m-dependent series:** Generation and verification that autocorrelation drops to zero for lags $> m$; correlation check confirms approximate independence.
- **α-mixing coefficients — AR(1):** Computed $\alpha(n)$ for AR(1) ($\phi=0.5$); exponential decay observed as $n$ increases.
- **α-mixing coefficients — MA(1) & Gaussian noise:** MA(1) shows slower decay than AR(1); independent Gaussian noise yields $\alpha(n)\approx 0$ from the start.

---

## Key Results

| Result | Finding |
|---|---|
| Sample mean CLT | $\sqrt{N}(\bar{X}_N - \mu)$ converges to $\mathcal{N}(0,1)$ under AR(1) dependence |
| Variance bias | $E[S_N^2] = \sigma^2 + O\big(\frac{1}{N}\sum\gamma_k\big)$ — negligible if autocorrelation decays fast |
| m-dependence | $\alpha(n)=0$ for $n>m$ → DMR condition satisfied if $X_0^2$ integrable |
| Mixing decay | AR(1) decays exponentially; MA(1) slower; Gaussian noise already $\alpha$-mixing |
| Variance estimator consistency | Requires $\sum_k \gamma_k^2 < \infty$; fails for long-memory processes |

---

<p align="center">
  <a href="https://github.com/mahamat9/Time-Series-Estimation">View on GitHub</a>
</p>
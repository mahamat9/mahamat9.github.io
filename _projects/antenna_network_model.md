---
layout: page
title: Antenna Network Model
description: Mathematical modeling and asymptotic analysis of received power in multi-antenna network configurations.
img: assets/img/antenna_network.png
importance: 12
category: academic
scientific_category: math
github: https://github.com/mahamat9/Modele_d-antennes
---

<style>
  h2 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** October 2023 – December 2023  
**Authors:** <u>M. Mahamat</u>, M. Charbonneau, A. Gonin  
**Source of the topic:** External Agrégation Exam 2017 — Modeling Problem

---

## Problem Statement

We consider a linear network of $n$ antennas equally spaced by a distance $d$, each with nominal power $x_i$. The **effective power** received at antenna $i$ is the sum of contributions from all antennas, weighted by a distance-dependent power loss function:

$$
\pi_i = \sum_{j=1}^{n} W\!\left(|i-j|\,\tau\right) x_j, \qquad \tau = \frac{d}{r_0}
$$

where the loss function is:

$$
W(r) = \frac{1}{1 + r^2}
$$

In matrix form: $$\boldsymbol{\pi} = A\mathbf{x},$$ where $A$ is a **symmetric positive definite Toeplitz matrix**:

$$
A_{ij} = W(|i-j|\,\tau), \qquad A = \begin{pmatrix} 1 & W(\tau) & W(2\tau) & \cdots \\ W(\tau) & 1 & W(\tau) & \cdots \\ \vdots & & \ddots & \end{pmatrix}
$$

---

## Mathematical Questions

### Questions 1–2 — Theoretical Analysis

Study of the matrix $A$:

- **Symmetry** and **positive definiteness** via the spectral structure of Toeplitz matrices
- Analysis of the **condition number** as $$n \to \infty$$ and $$\tau \to 0$$

Key reference used:

> **Gray, R.M.** — _Toeplitz and Circulant Matrices: A Review_, Foundations and Trends in Communications and Information Theory, 2006.

---

### Question 3 — LU Decomposition

The inverse problem: given target effective powers $\boldsymbol{\pi}$, find nominal powers $\mathbf{x}$ solving $A\mathbf{x} = \boldsymbol{\pi}$.

Three implementations of the matrix $A$ were compared:

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Version</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">n = 100</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">n = 1000</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">v1 (upper tri + symmetry)</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">5.41 ms</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">548 ms</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">v2 (full double loop)</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">8.91 ms</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">1.15 s</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">v3 (upper tri only)</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">5.98 ms</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">643 ms</td>
    </tr>
  </tbody>
</table>

**Version 1** is fastest. Resolution via hand-coded **LU factorization**:

$$
A = LU \implies L\mathbf{y} = \boldsymbol{\pi}, \quad U\mathbf{x} = \mathbf{y}
$$

<table style="width:50%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">n</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">LU time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">20</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">3.41 ms</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">1000</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">7.57 s</td>
    </tr>
  </tbody>
</table>

---

### Question 4 — Toeplitz-Adapted Algorithm

Since $A$ is **Toeplitz**, we exploit its structure via the **Levinson-Durbin** algorithm, avoiding explicit matrix construction:

**Step 1** — Build the sequence $$(f_1, \dots, f_n)$$ recursively:

$$
d_k = \sum_{j=1}^{k} t_j f_{k+1-j}, \qquad
\begin{pmatrix} 1 & d_k \\ d_k & 1 \end{pmatrix}
\begin{pmatrix} a_k \\ b_k \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \end{pmatrix}
$$

$$
f_{k+1} = a_k \tilde{f}_k^{\leftarrow} + b_k \tilde{f}_k
$$

**Step 2** — Update solution vector $\mathbf{x}$ incrementally.

<table style="width:60%; border-collapse:collapse; font-size:0.95rem; margin-bottom:2rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">n</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">LU</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:center;">Toeplitz</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">20</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">3.41 ms</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">3.25 ms</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;">1000</td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>7.57 s</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px; text-align:center;"><strong>396 ms</strong></td>
    </tr>
  </tbody>
</table>

**≈ 19× speedup** at $n=1000$, no matrix storage required.

---

### Questions 5–6 — Partial Network (Faulty Antennas)

Some antennas are out of order (set $I_P$). Only antennas in $$I = \{0,\dots,n-1\} \setminus I_P$$ are active.

The constrained problem becomes:

$$
P_I A P_I \, \mathbf{x} = P_I \boldsymbol{\pi}, \quad \text{with } x_i = 0 \text{ for } i \in I_P
$$

where $P_I$ is the projection onto active indices.

**Question 6** extends this to minimize $$\|A\mathbf{x} - \boldsymbol{\pi}\|^2,$$ leading to the normal equations:

$$
A^\top A \, \mathbf{x} = A^\top \boldsymbol{\pi} \implies A^2 \mathbf{x} = A\boldsymbol{\pi}
$$

(using symmetry of $A$).

**Test case:** $n=20$, $\tau=1$, faulty antennas: $$\{7, 8, 15, 16, 17, 18\}$$

<div class="row justify-content-center">
  <div class="col-sm-8">
    {% include figure.liquid path="assets/img/antenna_network.png"
       title="Nominal vs effective power per antenna"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Nominal power \(x_i\) (blue) vs effective power \(\pi_i\) (red) per antenna.
  Faulty antennas have zero nominal power; remaining antennas compensate.
</div>

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on numerical part on GitHub
  </a>
</div>
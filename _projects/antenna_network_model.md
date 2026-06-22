---
layout: page
title: Antenna Network Model
description: Mathematical study of signal power in antenna networks — Agrégation 2017 modeling problem
img: assets/img/antenna_network.png
importance: 11
category: academic
github: https://github.com/mahamat9/Modele_d-antennes
---

## Overview

**Duration:** October 2023 – December 2023  
**Co-authors:** M. Charbonneau, A. Gonin  
**Source:** External Agrégation Exam 2017 — Modeling Problem

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

| Version                   | $$n=100$$ | $$n=1000$$ |
| ------------------------- | --------- | ---------- |
| v1 (upper tri + symmetry) | 5.41 ms   | 548 ms     |
| v2 (full double loop)     | 8.91 ms   | 1.15 s     |
| v3 (upper tri only)       | 5.98 ms   | 643 ms     |

**Version 1** is fastest. Resolution via hand-coded **LU factorization**:

$$
A = LU \implies L\mathbf{y} = \boldsymbol{\pi}, \quad U\mathbf{x} = \mathbf{y}
$$

| $$n$$ | LU time |
| ----- | ------- |
| 20    | 3.41 ms |
| 1000  | 7.57 s  |

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

| $$n$$ | LU         | Toeplitz   |
| ----- | ---------- | ---------- |
| 20    | 3.41 ms    | 3.25 ms    |
| 1000  | **7.57 s** | **396 ms** |

**≈ 19× speedup** at $n=1000$ — no matrix storage required.

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

## Tools & Methods

- Toeplitz matrix theory & spectral analysis
- LU factorization (hand-coded)
- Levinson-Durbin algorithm
- Constrained least squares via projection operators
- NumPy, Matplotlib

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Modele_d-antennes" class="btn btn-primary" role="button" target="_blank">
    More details on numerical part on GitHub
  </a>
</div>

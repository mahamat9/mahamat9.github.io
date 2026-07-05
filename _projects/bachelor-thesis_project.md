---
layout: page
title: Real Roots of Real Polynomials
description: Theoretical analysis of algebraic criteria ensuring that all roots of a real polynomial are real-valued.
img: assets/img/polynomials_roots.png
importance: 13
category: academic
scientific_category: math
mailto: mmahamatnour99@gmail.com
---

<style>
  h2 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** January 2023 – May 2023  
**Supervisors:** Mohammed El Amrani — University of Angers  
**Authors:** <u>M. Mahamat</u>, K. Le Bihan, M. Hedde  

---

## Problem Statement

Given a real polynomial $$P(X) = \sum_{k=0}^{n} a_k X^k$$, under what conditions are **all its roots real**?

A classical result due to Newton provides a **necessary condition**: if all roots of $P$ are real, then its coefficients satisfy

$$
a_k^2 - \frac{n-k+1}{n-k} \cdot \frac{k+1}{k} \, a_{k-1} a_{k+1} \geq 0, \quad k \in [\![1, n-1]\!]
$$

However, this condition is **not sufficient** ; the polynomial

$$
P(X) = 5X^3 + 39X^2 + 92X + 58 = (X+1)(5X^2 + 34X + 58)
$$

satisfies Newton's inequalities yet has a pair of non-real complex roots.

---

## Main Results

#### Necessary Condition : Newton's Inequalities (Theorem 2.1)

If $P \in \mathbb{R}_n[X]$ has $n$ real roots $\lambda_1, \dots, \lambda_n$, then necessarily:

$$
\forall k \in [\![1, n-1]\!], \quad a_k^2 - \frac{n-k+1}{n-k} \cdot \frac{k+1}{k} \, a_{k-1} a_{k+1} \geq 0
$$

The proof relies on **elementary symmetric functions** and the **Cauchy-Schwarz inequality**, applied inductively via the AM-GM inequality.

---

#### Sufficient Condition (Theorem 3.2)

Let $$P(X) = \sum_{k=0}^{n} a_k X^k \in \mathbb{R}[X]$$ with $a_n = 1$. If:

$$
(H_1) \quad \forall k \in \{0, \dots, n-1\},\ a_k > 0
$$

$$
(H_2) \quad \forall k \in \{1, \dots, n-1\},\ a_k^2 - 4a_{k-1}a_{k+1} \geq 0
$$

then **all roots of $P$ are simple and real**.

The proof proceeds by induction on $\deg P$ : factoring out $X$ from $Q = P - a_0$ yields a degree-$(n{-}1)$ polynomial satisfying the same hypotheses; a continuity/compactness argument on the perturbed family $Q_t = Q + t$ then shows that any loss of a real root forces $a_1^2 - 4\alpha a_2 \leq 0$ for some $\alpha > a_0$, contradicting $(H_2)$. $\blacksquare$

---

#### Optimality of the Constant 4 (Theorem 3.3)

For any $\gamma \in \,]0, 4[$, there exists a monic polynomial with positive coefficients satisfying

$$
a_k^2 - \gamma \, a_{k-1} a_{k+1} > 0 \quad \forall k
$$

yet admitting **non-real complex roots** ; proving that the constant $4$ in $(H_2)$ **cannot be reduced**.

The construction is inductive: for $n=2$ one exhibits explicit coefficients; the inductive step uses the product $$P_t(X) = (tX+1)B(X)$$ and the limit

$$
\lim_{t \to +\infty} \Theta(P_t, k) = \Theta(B, k-1), \qquad \lim_{t \to +\infty} \Theta(P_t, 1) = +\infty
$$

where $$\Theta(A, k) = \frac{a_k^2}{(a_{k-1} a_{k+1})}.$$

---

## Summary Table

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Result</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Condition</th>
      <th style="border:1px solid #ccc; padding:10px 14px; text-align:left;">Conclusion</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Newton</strong> (necessary)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">All roots real</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">
        $$a_k^2 \geq \dfrac{n-k+1}{n-k}\dfrac{k+1}{k}\,a_{k-1}a_{k+1}$$
      </td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Kurtz</strong> (sufficient)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$$a_k > 0 \text{ and } a_k^2 \geq 4\,a_{k-1}a_{k+1}$$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">All roots real and simple</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Optimality</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">$\gamma < 4$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Condition insufficient</td>
    </tr>
  </tbody>
</table>

---

## Methods

- Elementary symmetric functions & Newton's identities
- Cauchy-Schwarz and AM-GM inequalities
- Intermediate Value Theorem & compactness arguments
- Induction on polynomial degree

> **Reference:** Kurtz, D.C. — _A sufficient condition for all the roots of a polynomial to be real_, The American Mathematical Monthly, Vol. 99, No. 3, Mar. 1992, pp. 259–263.

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="mailto:{{ page.mailto }}?subject=Request%20—%20Bachelor%27s%20Thesis%20%22Real%20Roots%20of%20Real%20Polynomials%22&body=Hello%2C%0A%0AI%20would%20like%20to%20receive%20your%20Bachelor%27s%20thesis%20entitled%20%22Real%20Roots%20of%20Real%20Polynomials%22.%0A%0AThank%20you."
     class="btn btn-primary" role="button">
    Request the thesis
  </a>
</div>
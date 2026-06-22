---
layout: page
title: Real Roots of Real Polynomials
description: Bachelor's thesis — A necessary and sufficient condition for all roots of a real polynomial to be real
img: assets/img/polynomials_roots.png
importance: 12
category: academic
mailto: mmahamatnour99@gmail.com
---

## Overview

**Duration:** January 2023 – May 2023  
**Supervisors:** Mohammed El Amrani — Université d'Angers  
**Co-authors:** K. Le Bihan, M. Hedde  
**Reference:** Kurtz, D.C. — *A sufficient condition for all the roots of a polynomial to be real*, The American Mathematical Monthly, Vol. 99, No. 3, Mar. 1992, pp. 259–263.

---

## Problem Statement

Given a real polynomial $$P(X) = \sum_{k=0}^{n} a_k X^k$$, under what conditions are **all its roots real**?

A classical result due to Newton provides a **necessary condition**: if all roots of $P$ are real, then its coefficients satisfy

$$
a_k^2 - \frac{n-k+1}{n-k} \cdot \frac{k+1}{k} \, a_{k-1} a_{k+1} \geq 0, \quad k \in [\![1, n-1]\!]
$$

However, this condition is **not sufficient** — the polynomial

$$
P(X) = 5X^3 + 39X^2 + 92X + 58 = (X+1)(5X^2 + 34X + 58)
$$

satisfies Newton's inequalities yet has a pair of non-real complex roots.

---

## Main Results

### Necessary Condition — Newton's Inequalities (Theorem 2.1)

If $P \in \mathbb{R}_n[X]$ has $n$ real roots $\lambda_1, \dots, \lambda_n$, then necessarily:

$$
\forall k \in [\![1, n-1]\!], \quad a_k^2 - \frac{n-k+1}{n-k} \cdot \frac{k+1}{k} \, a_{k-1} a_{k+1} \geq 0
$$

The proof relies on **elementary symmetric functions** and the **Cauchy-Schwarz inequality**, applied inductively via the AM-GM inequality.

---

### Sufficient Condition (Theorem 3.2)

Let $$P(X) = \sum_{k=0}^{n} a_k X^k \in \mathbb{R}[X]$$ with $a_n = 1$. If:

$$
(H_1) \quad \forall k \in \{0, \dots, n-1\},\ a_k > 0
$$

$$
(H_2) \quad \forall k \in \{1, \dots, n-1\},\ a_k^2 - 4a_{k-1}a_{k+1} \geq 0
$$

then **all roots of $P$ are simple and real**.

#### Key steps of the proof

**Step 1 — Fundamental Lemma (Lemma 3.1):**  
If $P$ has only negative roots and admits a multiple root, then there exists $$k \in [\![1, n-1]\!]$$ such that $$a_k^2 - 4a_{k-1}a_{k+1} \leq 0.$$

**Step 2 — Perturbation argument:**  
Set $Q = P - a_0$, so $$Q(X) = X \cdot R(X)$$ where $R$ satisfies the same hypotheses in degree $n-1$. By induction, $R$ has $n-1$ simple real roots, hence $Q$ has $n$ simple real roots, all $\leq 0$.

**Step 3 — Continuity / compactness:**  
Consider $Q_t(X) = Q(X) + t$ for $t \geq 0$. Define

$$
\alpha = \inf \{ t \geq 0 : N_t < n \}
$$

where $N_t$ = number of distinct real roots of $Q_t$.  
One shows $\alpha > 0$ (by the stability result **R1**), and that $Q_\alpha$ has all its roots real and negative, with at least one **multiple root**.

**Step 4 — Contradiction:**  
Applying the Fundamental Lemma to $Q_\alpha$ yields $k=1$ and $$a_1^2 - 4\alpha a_2 \leq 0.$$
Since $\alpha > a_0$, this contradicts $(H_2)$, forcing $\alpha > a_0$ — i.e., $P = Q + a_0$ has $n$ simple real roots. $$\blacksquare$$

---

### Optimality of the Constant 4 (Theorem 3.3)

For any $\gamma \in \,]0, 4[$, there exists a monic polynomial with positive coefficients satisfying

$$
a_k^2 - \gamma \, a_{k-1} a_{k+1} > 0 \quad \forall k
$$

yet admitting **non-real complex roots** — proving that the constant $4$ in $(H_2)$ **cannot be reduced**.

The construction is inductive: for $n=2$ one exhibits explicit coefficients; the inductive step uses the product $$P_t(X) = (tX+1)B(X)$$ and the limit

$$
\lim_{t \to +\infty} \Theta(P_t, k) = \Theta(B, k-1), \qquad \lim_{t \to +\infty} \Theta(P_t, 1) = +\infty
$$

where $$\Theta(A, k) = a_k^2 / (a_{k-1} a_{k+1})$$.

---

## Summary Table

| Result | Condition | Conclusion |
|---|---|---|
| Newton (necessary) | All roots real | $$a_k^2 \geq \frac{n-k+1}{n-k}\frac{k+1}{k} a_{k-1}a_{k+1}$$ |
| Kurtz (sufficient) | $$a_k > 0$$ and $$a_k^2 \geq 4a_{k-1}a_{k+1}$$ | All roots real and simple |
| Optimality | $$\gamma < 4$$ | Condition insufficient |

---

## Tools & Methods

- Elementary symmetric functions & Newton's identities  
- Cauchy-Schwarz and AM-GM inequalities  
- Intermediate Value Theorem & compactness arguments  
- Induction on polynomial degree

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="mailto:{{ page.mailto }}?subject=Request%20—%20Bachelor%27s%20Thesis%20%22Real%20Roots%20of%20Real%20Polynomials%22&body=Hello%2C%0A%0AI%20would%20like%20to%20receive%20your%20Bachelor%27s%20thesis%20entitled%20%22Real%20Roots%20of%20Real%20Polynomials%22.%0A%0AThank%20you."
     class="btn btn-primary" role="button">
    Request the thesis
  </a>
</div>
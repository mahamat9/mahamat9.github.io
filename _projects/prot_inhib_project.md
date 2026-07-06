---
layout: page
title: Molecule–Protein Inhibition Prediction
description: Statistical learning challenge for pIC50 prediction using molecular fingerprints and nonlinear regression models.
#description: Drug discovery data challenge — predicting pIC50 with Morgan fingerprints & XGBoost
img: assets/img/prot_inhib_cover.jpg
importance: 6
category: academic
scientific_category: challenge #applied-ml-dl
github: https://github.com/mahamat9/Challenge-prot-inhib
---

<style>
  h2, { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Duration:** December 2024 – January 2025  
**Course:** Statistics in High Dimension  
**Platform:** ENS Paris-Saclay Data Challenge — [challengedata.ens.fr](https://challengedata.ens.fr/)  
**Task type:** Supervised regression — predict **pIC50** (protein inhibition strength) of small molecules

Screening thousands of molecules computationally before any lab experiment and that's the promise of **ML-assisted drug discovery**.

---

## The Problem

Predicting **protein–ligand binding affinity** is a cornerstone of rational drug design. The target variable here is **pIC50**, the negative logarithm of the molar concentration of a molecule required to inhibit 50% of a given protein ; the standard potency measure in medicinal chemistry. Higher pIC50 = stronger inhibition. Our dataset contains **4,400 training molecules** (SMILES strings) and **2,934 test molecules** from the challenge platform.

---

## Pipeline at a Glance

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin:1rem 0;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <td style="border:1px solid #ccc; padding:10px 14px;">Step</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Approach</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Molecular representation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Morgan fingerprints (ECFP, radius=2, 2048 bits) via RDKit</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Model</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">XGBoost gradient boosting regressor</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Tuning</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">5-fold GridSearchCV, n_estimators, learning_rate, max_depth, L1/L2 regularization</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Evaluation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">R² score  5-fold cross-validation + challenge leaderboard</td>
    </tr>
  </tbody>
</table>

---

## Why Morgan Fingerprints?

**SMILES strings** are textual molecular representations — unreadable directly by ML models. Morgan fingerprints (a.k.a. **ECFP — Extended-Connectivity Fingerprints**) convert each SMILES into a fixed-length **2048-bit binary vector** by enumerating local chemical environments around each atom (radius=2). No 3D structure or quantum chemistry needed: the topology alone captures pharmacophoric patterns. They are the standard **baseline descriptor** in molecular ML (Rogers & Hahn, 2010) and remain competitive against deep-learning alternatives on small-to-medium datasets.

---

## Stack

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin:1rem 0;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <td style="border:1px solid #ccc; padding:10px 14px;">Library</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Role</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">RDKit</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">SMILES → fingerprint encoding (Morgan/ECFP)</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">XGBoost</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Gradient boosting regressor</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">scikit-learn</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">GridSearchCV, 5-fold cross-validation, custom estimator wrapper</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">NumPy / Pandas</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Data loading and handling</td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>

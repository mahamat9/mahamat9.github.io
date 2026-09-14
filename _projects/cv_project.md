---
layout: page
title: Deep image processing
description: Applied computer vision study covering object detection, segmentation, and transfer-learning based evaluation protocols.
#description: Applied computer vision coursework covering classification, segmentation, and object detection, from fine-tuned CNNs to foundation models.
img: assets/img/cv_illustration.jpg
importance: 2
category: Academic
scientific_category: applied-ml-dl
#giscus_comments: true
github: https://github.com/mahamat9/deep-processing-images
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type:** Practical coursework — _D. Rousseau_, University of Angers  
**Author:** <u>M. Mahamat</u>  
**Date:** February 2025  
**Code:** [github.com/mahamat9/deep-processing-images](https://github.com/mahamat9/deep-processing-images)

---

Three assignments covering the classical progression of computer vision tasks: **what is in the image** (classification), **which pixels belong to it** (segmentation), and **how many and/or where** (detection).

---

## Image classification

_[Repository](https://github.com/mahamat9/deep-processing-images/tree/main/CNN%20%2B%20TP%20clf)_

A convolutional classifier trained end-to-end, used as a baseline.

- CNN trained from scratch
- Evaluation: accuracy

---

## Blood cell segmentation

_[Repository](https://github.com/mahamat9/deep-processing-images/tree/main/TP%20Segmentation)_

Pixel-level segmentation of blood cells, comparing a purpose-built architecture against a frozen pretrained encoder.

- **U-Net**: encoder–decoder, designed for biomedical segmentation from few images
- **VGG16 encoder (frozen) + decoder**: pretrained feature extractor reused as a backbone
- Data augmentation (rotations, flips, elastic deformations, brightness jitter) applied to the transfer model to counter the overfitting observed without it
- Loss: **BCE** · Metric: **Dice score**

<!-- %- Data augmentation (rotation, flips, elastic deformation) #Augmentation de données (rotations, déformations élastique, variations de luminosité $\ldots$) appliquée au modèle par transfert pour compenser le sur-apprentissage observé ; Dice de $0{,}97$ (U-Net) contre $0{,}90$ (VGG16 \textit{fine-tuné}).-->

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blood_cell_unet.jpg" title="U-Net Segmentation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blood_cell_vgg16.jpg" title="VGG16 Segmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: U-Net trained from scratch. Right: VGG16 encoder with transfer learning.
</div>

The U-Net reached a **Dice score of 0.97**, against **0.90** for the fine-tuned VGG16; a reminder that on a domain far from ImageNet, an architecture designed for the task can outperform a transferred representation.

The metric choice matters here more than anywhere else: on images dominated by background, accuracy is meaningless, since predicting "background" everywhere already scores high. **Dice** measures overlap with the actual cell regions, which is what determines whether boundaries are recovered.

---

## Object detection

_[Repository](https://github.com/mahamat9/deep-processing-images/tree/main/TP%20Object%20Detection)_

Detection of apples in orchard imagery; a setting with heavy occlusion, variable lighting and objects that are small relative to the frame.

- **YOLO11** fine-tuned on the **MinneApple** dataset
- **DINO**, a self-supervised foundation model, applied zero-shot
- Comparison of a supervised detector against a general-purpose visual representation

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/object_detection_yolo.jpg" title="YOLO Detection Results" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/object_detection_dino.jpg" title="DINO Detection Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: YOLO11 fine-tuned on MinneApple. Right: DINO foundation model.
</div>

The contrast here is the most instructive of the three: a fine-tuned detector is strong on the distribution it was trained on, while a foundation model trades that precision for generality. Which one is preferable depends less on benchmark numbers than on whether the deployment distribution is known in advance.

---

This is the domain I find most compelling, and the reason it appears elsewhere in my work such as the [diffusion thesis](/projects/diffusion/) and the [ECG denoising benchmark](/projects/ecg-denoising/) both revisit the same question in another setting.

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>

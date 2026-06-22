---
layout: page
title: Generative Models — GAN & VAE
description: Implementation and experimentation of GAN and VAE architectures on MNIST and CelebA
img: assets/img/gan-vae_architectures.png
importance: 3
category: academic
github: https://github.com/mahamat9/Generative-Models
---

## Overview

**Type:** Practical coursework — _S. Lamprier_  
**Stack:** Python · PyTorch · NumPy · Matplotlib
Duration: November 2024 – December 2024

---

## Context

This project covers the implementation and experimentation of two fundamental **deep generative model** families:

- **GAN** — Generative Adversarial Networks
- **VAE** — Variational Autoencoders

Both approaches are trained on standard benchmarks (**MNIST**, **CelebA**) and evaluated through visual inspection of generated samples and latent space analysis.

---

## Generative Adversarial Network

### Principle

A GAN frames generation as a **two-player minimax game** between:

| Network           | Role                                  | Objective    |
| ----------------- | ------------------------------------- | ------------ |
| Generator $G$     | Maps noise $z \sim p_z$ to data space | Fool $D$     |
| Discriminator $D$ | Classifies real vs. generated         | Detect fakes |

The training objective is:

$$
\min_G \max_D \;
\mathbb{E}_{x \sim p_\text{data}}\!\left[\log D(x)\right]
+
\mathbb{E}_{z \sim p_z}\!\left[\log(1 - D(G(z)))\right]
$$

### Architecture

<p align="center">
  <img src="assets/img/a-gan-architecture.png" alt="GAN Architecture" width="80%"/>
</p>

> The Generator takes a noise vector $z$ and progressively upsamples
> it into a realistic image. The Discriminator acts as a binary
> classifier between real and generated samples.

### Variant explored

| Variant   | Key idea                                               |
| --------- | ------------------------------------------------------ |
| **DCGAN** | Convolutional layers, batch norm, more stable training |

### Generated samples — MNIST

<p align="center">
  <img src="assets/img/gan_mnist.png" alt="GAN MNIST samples" width="70%"/>
</p>

### Training dynamics

<p align="center">
  <img src="https://i.stack.imgur.com/YyCu2.gif" alt="GAN training animation" width="55%"/>
</p>

> Samples evolve from pure noise toward structured digits as training
> progresses — illustrating the adversarial dynamic between $G$ and $D$.

### Implementation highlights

- Fully connected and convolutional architectures
- Training instability analysis (mode collapse, vanishing gradients)
- Visual inspection of generated samples across epochs

---

## Variational Autoencoder

### Principle

A VAE is a **probabilistic generative model** that learns a structured latent space by maximising the **Evidence Lower Bound (ELBO)**:

$$
\mathcal{L}(\theta, \phi) =
\mathbb{E}_{q_\phi(z \mid x)}\!\left[\log p_\theta(x \mid z)\right]
- D_\text{KL}\!\left(q_\phi(z \mid x) \;\|\; p(z)\right)
$$

| Term                    | Role                                                    |
| ----------------------- | ------------------------------------------------------- |
| Reconstruction term     | Forces the decoder to recover $x$                       |
| KL divergence           | Regularises the latent space toward $\mathcal{N}(0, I)$ |
| Reparametrisation trick | Enables backprop through sampling                       |

### Architecture

<p align="center">
  <img src="assets/img/a-vae-architecture.png" alt="VAE Architecture" width="80%"/>
</p>

> The encoder outputs parameters $(\mu, \sigma)$ of a Gaussian
> distribution. A latent vector $z$ is sampled via the reparametrisation
> trick and decoded back into data space.

### Generated samples — MNIST

<p align="center">
  <img src="assets/img/vae_mnist.png" alt="VAE MNIST samples" width="70%"/>
</p>

### Implementation highlights

- Encoder / decoder architecture in PyTorch
- Latent space visualisation (2D projections)
- Interpolation in latent space
- Comparison of sample quality vs. GAN

---

## GAN vs. VAE — Comparison

| Property            | GAN                | VAE             |
| ------------------- | ------------------ | --------------- |
| Sample sharpness    | High               | Blurry          |
| Training stability  | Unstable           | Stable          |
| Latent structure    | No explicit space  | Structured      |
| Likelihood estimate | Implicit           | Via ELBO        |
| Mode coverage       | Mode collapse risk | Better coverage |

---

## Tools & Stack

- **Language:** Python
- **Framework:** PyTorch
- **Datasets:** MNIST, CelebA
- **Visualisation:** Matplotlib, Seaborn
- **Notebooks:** Jupyter (`.ipynb`)

---

<div style="display:flex; gap:1rem; justify-content:center; margin: 2.5rem 0;">
  <a href="https://github.com/mahamat9/Generative-Models"
     class="btn btn-primary" role="button" target="_blank">
    View on GitHub
  </a>
  <a href="https://github.com/mahamat9/Generative-Models/blob/main/Mod%C3%A8les_Generatifs_report.pdf"
     class="btn btn-secondary" role="button" target="_blank">
    View report
  </a>
</div>

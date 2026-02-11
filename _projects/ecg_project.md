---
layout: page
title: ECG Denoising with Deep Learning
description: Comparative study of a convolutional denoising autoencoder and a diffusion model on PTB-XL ECG signals.
img: assets/img/deep_ecg_illustration.png
importance: 1 # or 3
category: personal
#related_publications: true
github: https://github.com/mahamat9/deep-denoising-ecg
---

## Overview
This project benchmarks two denoising families for 12-lead ECG signals: a U-Net-style convolutional denoising autoencoder (CDAE) and a denoising diffusion probabilistic model (DDPM). The goal is to restore clean ECG waveforms from realistic clinical noise.

## Dataset
- PTB-XL 12-lead ECG dataset
- Cached preprocessing for faster experiments
- Train/val/test split: 80/10/10

## Noise Model
We simulate common ECG perturbations by mixing Gaussian noise, baseline wander, power-line interference, and muscle artifacts. Noise intensity is controlled with a target SNR range.

## Models
- CDAE: U-Net-style 1D convolutional encoder-decoder with residual learning
- DDPM: diffusion model trained to predict noise across timesteps

## Training and Evaluation
- Optimizer: Adam with ReduceLROnPlateau scheduler
- Early stopping on validation loss
- Metrics: MSE, Cross-correlation and SNR improvement

## Code and Assets
- GitHub repository: https://github.com/mahamat9/deep-denoising-ecg

---
layout: page
title: ECG Denoising with Deep Learning
description: Comparative benchmarking of convolutional and diffusion-based denoising models for multilead PTB-XL ECG reconstruction.
img: assets/img/deep_ecg_illustration.png
importance: 1 # or 3
category: Personal
scientific_category: applied-ml-dl
#related_publications: true
github: https://github.com/mahamat9/deep-denoising-ecg
---

## Overview

**Type:** Personal project  
**Data:** PTB-XL — 12-lead clinical ECG  
**Code:** [github.com/mahamat9/deep-denoising-ecg](https://github.com/mahamat9/deep-denoising-ecg)  

---

A clinical ECG is rarely clean. Electrode motion, muscle activity, respiration and mains interference overlap with the signal in both time and frequency; which is what makes classical filtering unsatisfactory: the noise bands and the diagnostically relevant bands are not disjoint. Denoising an ECG is therefore not a filtering problem but a **reconstruction** problem, where the model must know what a plausible cardiac waveform looks like.

This project compares two ways of encoding that prior: a **convolutional denoising autoencoder (CDAE)**, which learns a direct noisy-to-clean mapping, and a **denoising diffusion probabilistic model (DDPM)**, which learns the noise distribution itself and removes it iteratively. It is also a deliberate follow-up to my [master's thesis on diffusion models](/projects/diffusion/).

---

## Data and noise model

**PTB-XL** provides 12-lead clinical recordings, split 80/10/10 with preprocessing cached to keep the experimental loop short.

Since paired clean/noisy recordings do not exist, degradation is **simulated** by superimposing four perturbations that reproduce the standard artefacts of ambulatory acquisition:

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Perturbation</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Physical origin</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Spectral signature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Baseline wander</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Respiration, electrode motion</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Low frequency ($\lt$ 1 Hz)</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Power-line interference</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Mains coupling</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Narrowband, $50/60$ Hz</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Muscle artefact (EMG)</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Skeletal muscle activity</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Broadband</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>Gaussian noise</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Instrumentation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Uniform</td>
    </tr>
  </tbody>
</table>

Intensity is controlled through a target **SNR range**, so that performance can be reported as a function of degradation severity rather than at a single arbitrary point.

The choice matters, and is a **limitation**: simulated noise is additive and independent of the underlying signal, whereas real artefacts are often correlated with cardiac and respiratory activity. Results should be read as an upper bound.

---

## Models

**CDAE** : a 1D U-Net-style encoder–decoder with residual learning. Skip connections preserve the temporal resolution needed to keep *QRS morphology* intact; the residual formulation makes the network predict the *noise* rather than the signal, which is the easier target when the SNR is already moderate.
**One forward pass per reconstruction.**

**DDPM** : trained to predict the injected noise across timesteps, then run in reverse to reconstruct the signal. The model learns a distribution over plausible ECGs rather than a point-wise mapping, which in principle handles ambiguous segments better. **Cost: multiple sequential evaluations per reconstruction.**

The comparison is therefore not only about accuracy, but about **what that accuracy costs**; a decisive question for a signal acquired continuously in ambulatory settings.

---

## Evaluation

Training used Adam with `ReduceLROnPlateau` and early stopping on validation loss. Three complementary metrics were tracked:

- **MSE** : global reconstruction error; sensitive to amplitude.
- **SNR improvement** : the gain actually delivered relative to the degraded input, reported across the SNR range rather than averaged into a single figure.
- **Cross-correlation** : waveform shape agreement, largely invariant to amplitude scaling.

The three are needed together because MSE is a poor proxy for clinical usability here: a model can lower it substantially while smoothing the QRS complex or attenuating the P wave; exactly the structure a cardiologist reads. Cross-correlation partly guards against that, though neither metric replaces expert's validation.

**Quantitative results are reported in the [repository]({{ page.github }}).**

---

## Scope and limitations

- Noise is **synthetic and additive**; validation on annotated real-world artefacts remains open.
- Evaluation is **signal-level**, not diagnostic: no downstream test verifies that clinical measurements (PR interval, ST segment, QT) survive denoising; the criterion that would actually matter.
- The DDPM was not optimised, so the reported latency gap reflects the naive setting.

---

## Takeaway

Beyond the benchmark itself, the project made a trade-off concrete: the diffusion model's richer generative prior comes at a sampling cost that is hard to justify on a signal meant to be processed continuously, unless the reconstruction is genuinely ambiguous.

---

<div style="text-align: center; margin: 2.5rem 0;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>
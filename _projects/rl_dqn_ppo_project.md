---
layout: page
title: Reinforcement Learning
description: Comparative reinforcement learning implementation of value-based and policy-gradient agents on control and Atari benchmarks.
img: assets/img/q-learning-rl.png
importance: 5
category: academic
scientific_category: applied-ml-dl
github: https://github.com/mahamat9/RL-DQN-PPO
---

<style>
  h2, h3 { text-align: center; margin-top: 2rem; margin-bottom: 2rem; }
</style>

## Overview

**Type:** Practical coursework of *S. Lamprier*  
**Author:** <u>M. Mahamat</u>  
**Duration:** November 2024 – December 2024

---

## Context

Reinforcement Learning (RL) is a learning paradigm where an **agent** learns to make decisions by interacting with an **environment**. The agent observes state $s_t$, selects action $a_t$, receives reward $r_t$, and transitions to $s_{t+1}$. The goal: learn a **policy** $\pi(a\|s)$ that maximizes expected cumulative return.

This project implements two foundational deep RL algorithms and evaluates them on classic control tasks:

<table style="width:100%; border-collapse:collapse; font-size:0.95rem; margin:1rem 0;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Algorithm</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Type</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Policy Update</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>DQN</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Value-based</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Off-policy</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>PPO</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Policy-based</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">On-policy</td>
    </tr>
  </tbody>
</table>

<p align="center" style="margin:1.5rem 0;">
  <img src="https://gymnasium.farama.org/_images/cart_pole.gif" width="35%" style="margin-right:3%;" alt="CartPole"/>
  <img src="https://gymnasium.farama.org/_images/lunar_lander.gif" width="35%;" alt="LunarLander"/>
</p>

---

## Mathematical Foundations

### Markov Decision Process (MDP)

An RL problem is formally defined as an MDP, a 4-tuple $(\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R})$:

- $\mathcal{S}$: state space
- $\mathcal{A}$: action space
- $\mathcal{P}(s'\|s,a)$: transition dynamics
- $\mathcal{R}(s,a,s')$: reward function

The agent's goal is to maximize the **expected return**:

$$G_t = \sum_{k=0}^{\infty} \gamma^k r_{t+k+1}, \quad \gamma \in [0,1]$$

where $\gamma$ is the **discount factor**.

### The Bellman Equation

The action-value function (Q-function) under policy $\pi$ satisfies:

$$Q^\pi(s,a) = \mathbb{E}\!\left[r + \gamma \max_{a'} Q^\pi(s',a')\right]$$

This recursive relationship is the foundation of Q-learning and DQN.

<p align="center">
  <img src="https://www2.isye.gatech.edu/~fferdinando3/classes/cis467/fa20/img/value.png" 
       width="40%" alt="GridWorld value function"/>
  <br><em>GridWorld — Value function learned by Q-learning</em>
</p>

---

## Deep Q-Network

### Principle

DQN approximates the Q-function using a deep neural network. Instead of learning Q-values for every state-action pair in a table, DQN generalizes across states by sharing weights. Key innovations:

- **Experience Replay**: Store transitions $(s,a,r,s')$ in a replay buffer and sample uniformly at each update, breaking temporal correlation.
- **Target Network**: A separate network with frozen weights provides stable targets, reducing divergence during training.

### Main Equations

The Q-learning update rule is:

$$\theta_{t+1} = \theta_t + \alpha \nabla_\theta \mathcal{L}(\theta)$$

where the loss function uses the **TD error**:

$$\mathcal{L}(\theta) = \mathbb{E}_{(s,a,r,s')\sim \mathcal{B}}\!\left[\left(r + \gamma \max_{a'} Q_{\theta^-}(s',a') - Q_\theta(s,a)\right)^2\right]$$

where $\theta^-$ are the weights of the **target network** (updated periodically).

### Architecture

$$

\begin{array}{c}
\boxed{\text{Input: } s \in \mathbb{R}^n} \\
\downarrow \\
\boxed{\text{Dense}(256) \to \text{ReLU}} \\
\downarrow \\
\boxed{\text{Dense}(256) \to \text{ReLU}} \\
\downarrow \\
\boxed{Q_\theta(s, a) \in \mathbb{R}^{|\mathcal{A}|}} \quad \text{(Q-values for all actions)} \\
\downarrow \\
\boxed{\pi(s) = \arg\max_a Q_\theta(s, a)} \quad \text{(greedy policy)}
\end{array}

$$

### Variants

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Variant</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Innovation</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Benefit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Prioritized Replay</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Sample by TD-error magnitude</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Faster convergence</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Double DQN</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Decouple action selection & evaluation</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Reduce overestimation</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Dueling DQN</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Decompose $Q(s,a) = V(s) + A(s,a)$</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Better generalization</td>
    </tr>
  </tbody>
</table>

---

## Proximal Policy Optimization

### Principle

PPO is a **policy gradient** method that alternates between collecting trajectories and optimizing a clipped surrogate objective. It addresses the instability of vanilla policy gradient by constraining policy updates.

### Clipped Surrogate Objective

PPO optimizes the objective:

$$L^{\text{CLIP}}(\theta) = \mathbb{E}_t\!\left[\min\!\left(r_t(\theta)\hat{A}_t,\; \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon)\hat{A}_t\right)\right]$$

where the **probability ratio** is:

$$r_t(\theta) = \frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{\text{old}}}(a_t|s_t)}$$

The clip operation prevents overly large policy changes, stabilizing training.

### Actor-Critic Architecture

$$

\begin{array}{c}
\boxed{\text{state } s \in \mathbb{R}^n} \\
\downarrow \\
\boxed{\begin{array}{c}\text{Shared Encoder} \\ \text{(Conv / Dense)}\end{array}} \\
\downarrow \\
\boxed{\phi(s)} \\
\downarrow\quad\downarrow \\
\begin{array}{cc}
\boxed{\pi_\theta(a|s)} & \boxed{V_\theta(s)} \\
\text{Actor head} & \text{Critic head}
\end{array}
\end{array}

$$

The **advantage estimator** $\hat{A}_t$ is computed via GAE (Generalized Advantage Estimation):

$$\hat{A}_t^{\text{GAE}(\lambda)} = \sum_{l=0}^{T-t-1} (\gamma\lambda)^l \delta_{t+l}$$

where $\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t).$

### Variants

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Variant</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Modification</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Effect</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">PPO-Clip (base)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Clipped surrogate objective</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Stable, reliable training</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">PPO + Entropy</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Add $c \cdot H(\pi_\theta)$ term</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Encourage exploration</td>
    </tr>
  </tbody>
</table>

---

## Environments Tested

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Environment</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Domain</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">DQN</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">PPO</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>GridWorld</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Discrete / tabular</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>CartPole-v1</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Classic control (4D)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;"><strong>LunarLander-v3</strong></td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Classic control (8D)</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">✓</td>
    </tr>
  </tbody>
</table>

---

## Stack

<table style="width:100%; border-collapse:collapse; font-size:0.95rem;">
  <thead>
    <tr style="background-color:#5b5b5b;">
      <th style="border:1px solid #ccc; padding:10px 14px;">Component</th>
      <th style="border:1px solid #ccc; padding:10px 14px;">Technology</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Language</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Python 3.10+</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Deep Learning</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">PyTorch 2.0+</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">RL Environments</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">Gymnasium</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:10px 14px;">Numerics & plots</td>
      <td style="border:1px solid #ccc; padding:10px 14px;">NumPy · Matplotlib</td>
    </tr>
  </tbody>
</table>

---

<div style="text-align: center; margin: 2.5rem 0; display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap;">
  <a href="{{ page.github }}" class="btn btn-primary" role="button" target="_blank">
    <i class="fab fa-github"></i> More details on GitHub
  </a>
</div>
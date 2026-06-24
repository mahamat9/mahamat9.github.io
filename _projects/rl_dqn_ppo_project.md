---
layout: page
title: Reinforcement Learning — DQN & PPO
description: Implementation of Deep Q-Network and Proximal Policy Optimization on classic control and Atari environments
img: assets/img/q-learning-rl.png
importance: 5
category: academic
github: https://github.com/mahamat9/RL-DQN-PPO
---

## Overview

**Type:** Practical coursework — _S. Lamprier_  
**Stack:** Python · PyTorch · Gymnasium · NumPy · Matplotlib
Duration: November 2024 – December 2024

---

## Context

Reinforcement Learning (RL) is a paradigm where an **agent** learns to make decisions by interacting with an **environment**. The agent observes the state $s_t$, selects an action $a_t$, receives a scalar reward $r_t$, and transitions to a new state $s_{t+1}$. The agent's goal is to learn a **policy** $\pi(a|s)$ that maximizes the expected cumulative return.

This project implements two deep RL algorithms:

| Algorithm | Type         | Category   |
| --------- | ------------ | ---------- |
| **DQN**   | Value-based  | Off-policy |
| **PPO**   | Policy-based | On-policy  |

---

<p align="center">
  <img src="https://gymnasium.farama.org/_images/cart_pole.gif" width="30%"/>
  <img src="https://gymnasium.farama.org/_images/lunar_lander.gif" width="30%"/>
</p>

---

## Mathematical Foundations

### Markov Decision Process (MDP)

An RL problem is formally defined as an MDP — a 4-tuple $(\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R})$:

- $\mathcal{S}$: state space
- $\mathcal{A}$: action space
- $\mathcal{P}(s'|s,a)$: transition dynamics
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

### Key Equations

The Q-learning update rule is:

$$\theta_{t+1} = \theta_t + \alpha \nabla_\theta \mathcal{L}(\theta)$$

where the loss function uses the **TD error**:

$$\mathcal{L}(\theta) = \mathbb{E}_{(s,a,r,s')\sim \mathcal{B}}\!\left[\left(r + \gamma \max_{a'} Q_{\theta^-}(s',a') - Q_\theta(s,a)\right)^2\right]$$

where $\theta^-$ are the weights of the **target network** (updated periodically).

### Architecture

```
Input: state s in R^n
  |
  v
Dense(256) -> ReLU
  |
  v
Dense(256) -> ReLU
  |
  v
Dense(|A|)           <- Q(s, a) for all actions
  |
  v
argmax_a Q(s, a)     <- greedy policy pi(s)
```

### Variants

| Variant                                      | Innovation                         | Benefit                |
| -------------------------------------------- | ---------------------------------- | ---------------------- |
| **Prioritized Replay** (Schaul et al., 2016) | Sample by TD-error magnitude       | Faster convergence     |
| **Double DQN** (van Hasselt et al., 2015)    | Decouple selection and evaluation  | Reduces overestimation |
| **Dueling DQN** (Wang et al., 2016)          | Decompose $Q(s,a) = V(s) + A(s,a)$ | Better generalization  |

### Environments Tested

| Environment     | Domain             | Complexity        |
| --------------- | ------------------ | ----------------- |
| **GridWorld**   | Tabular / discrete | Low (custom grid) |
| **CartPole**    | Classic control    | Low (4D state)    |
| **LunarLander** | Classic control    | Medium            |

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

```
         +---------------------+
state s ->|   Shared Encoder    |-> shared features
         |  (optional: Conv)   |
         +---------+-----------+
                   |       |
                   v       v
              Actor head   Critic head
              pi(a|s)      V(s)
```

The **advantage estimator** $\hat{A}_t$ is computed via GAE (Generalized Advantage Estimation):

$$\hat{A}_t^{\text{GAE}(\lambda)} = \sum_{l=0}^{T-t-1} (\gamma\lambda)^l \delta_{t+l}$$

where $\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$.

### Variants

| Variant                    | Innovation                                 | Benefit            |
| -------------------------- | ------------------------------------------ | ------------------ |
| PPO-Clip                   | Base clipped surrogate objective           | Stable, reliable   |
| **PPO with Entropy Bonus** | Add $c \cdot H(\pi_\theta(\cdot \vert s))$ | Better exploration |

### Environments Tested

| Environment        | Domain             | Actor-Critic Suitable  |
| ------------------ | ------------------ | ---------------------- |
| **GridWorld**      | Tabular / discrete | Yes (discrete actions) |
| **CartPole-v1**    | Classic control    | Yes                    |
| **LunarLander-v3** | Classic control    | Yes                    |

---

## Tools and Stack

| Tool             | Role                          |
| ---------------- | ----------------------------- |
| **Python 3.10+** | Language                      |
| **PyTorch 2.0+** | Neural networks, GPU training |
| **Gymnasium**    | Environment interfaces        |
| **NumPy**        | Numerical operations          |
| **Matplotlib**   | Training curves, reward plots |

---

<div style="display: flex; gap: 10px; justify-content: center; margin-top: 40px;">
  <a href="https://github.com/mahamat9/RL-DQN-PPO" style="padding: 10px 24px; background: #2c3e50; color: white; text-decoration: none; border-radius: 6px; font-size: 14px; font-weight: 600;">View on GitHub</a>
</div>

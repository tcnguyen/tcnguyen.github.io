---
layout: page
title: Notes for "Reinforcement Learning: An Introduction"
---
## Chapter 2: n-Armed Bandit Problem

- Choose repeatedly (eg 1000 times) among n different options, or actions.
- Receive a numerical reward chosen from a stationary probability distribution that depends on the action
- Objective: maximize the expected total reward.

#### Analyse ####

- We don't know the expected reward for each action ==> estimate by trying (exploration)
- At each time step there is an action with max expected reward ==> `greedy action`
- `Exploiting` is to select a greedy action
- `Exploring` is to select a non-greedy action, which is important to improve the estimations of non-greedy action's value. This improve the total reward in the long run.

**Whether it is better to explore or exploit depends in a complex way on the precise values of the estimates, uncertainties, and the number of remaining steps**

#### $\epsilon$-greedy ####
Suppose at step $t$, action $a$ is selected $K_a$ times, the estimated value for $a$ is:

$$
Q_t(a) = \frac{R_1 + ... + R_{K_a}}{K_a}
$$

 Choose the best action following the estimation almost all the time, but every once in a while, say with small probability ε, select randomly from amongst all the actions with equal probability independently of the action-value estimates.

**Advantage:** In the limit as the number of plays increases, every action will be sampled an infinite number of times, guaranteeing that $K_a → ∞$ for all $a$, and thus ensuring that all the $Q_t(a)$ converge to the true expected reward $q_∗(a)$

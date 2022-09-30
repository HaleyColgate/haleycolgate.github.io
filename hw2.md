---
layout: default
title: Defining the Problem
---

Consider an agent interacting with a Markov Decision Process (MDP) with state space $$S$$, action space $$A$$, rewards $$R$$, and transition probabilities $$P$$.  At each time point $$t$$, the agent observes a state $$s_t\in S$$, chooses an action $$a_t\in A$$ based on their policy $$\pi_t$$, transitions to a new state $$s_{t+1}$$ via transition probabilities unknown to them, and receives a reward $$r_t(s_{t}, a_{t})$$ from the environment.  The agent is given a goal like "reach 40 points in less than 20 moves." 

I want to know if we can dynamically modify the MDP to remove biases in the agent's policy.  Biases in executive function decrease flexibility in problem solving by restricting the space of possible policies and/or increasing the time it takes to arrive at some policies, so if we can decrease an agent's biases we could potentially train them to solve problems more quickly and more accurately.

We define 

$$Y_t=\begin{cases} 1 && \text{the optimal choice was right} \\ -1 && \text{the optimal choice was left} \end{cases}$$ 

and 

$$X_t=\begin{cases} 0 && \text{the agent's choice was optimal}\\
1 && \text{the agent's choice was not optimal.}\end{cases}$$  

Note that optimality could be defined in terms of the underlying MDP or in terms of the experience of the agent.  Now let $$Z(S_1,R_1,S_2,R_2,...,R_{t-1},S_t) = (X_t,Y_t)$$.  We are then interested in evaluating the average causal excursion effect $$\mathbb{E}[Z(S_1,R_1,S_2,R_2,...,r_{t-1},s_t) - Z(S_1,R_1,S_2,R_2,...,r'_{t-1},s'_t)]$$ for $$r_t,r'_t\in R$$ and $$s_t,s'_t\in S$$.

![Directed Acyclic Graph](/assets/images/dag.png)

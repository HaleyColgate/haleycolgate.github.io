---
layout: default
title: Defining the Problem
---

Consider an agent interacting with a Markov Decision Process (MDP) with state space $$S$$, action space $$A$$, rewards $$R$$, and transition probabilities $$P$$.  At each time point $$t$$, the agent observes a state $$s_t\in S$$, chooses an action $$a_t\in A$$ based on their policy $$\pi_t$$, transitions to a new state $$s_{t+1}$$ given by transition probability $$p(s_{t+1}\|s_t,a_t)$$ unknown to them, and receives a reward $$r_t(s_{t}, a_{t})$$ from the environment.  The agent is given a goal like "reach 40 points in less than 20 moves." Let $$M_T$$ be the $$T$$-th MDP an agent interacts with.

I want to know if we can give the agent a sequence of MDP problems to reduce biases in the agent's approach to the problems.  Biases in executive function decrease flexibility in problem solving by restricting the space of possible policies and/or increasing the time it takes to arrive at some policies, so if we can decrease an agent's biases we could potentially train them to solve problems more quickly and more accurately.

We define $$Y_T$$ to be the number of optimal choices the agent makes while interacting with $$M_T$$.

Note that optimality could be defined in terms of the underlying MDP or in terms of the experience of the agent, so $$Y_T$$ can depend on both the MDP and the agent.  We are then interested in evaluating the conditional average causal excursion effect $$\mathbb{E}[Y_T(M_1, M_2, \dots, m_T) - Y_T(M_1, M_2, \dots, m_T')]$$.

![Directed Acyclic Graph](/assets/images/mdp_diagram.png)

Note that $$Y_T$$ depends on the entire history of the MDPs and prior actions.

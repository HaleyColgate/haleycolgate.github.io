---
layout: default
title: Estimation 
--- 
# The Data
In the Seqer dataset, if we consider just the first MDP played by each participant the MDPs all have 3 states, the transitions were deterministic, and the actions were binary.  Rewards depended only on the next state.  Two states each had reward probability 0.2, while the other state had reward probability 0.8.  The high-reward state and one low-reward state had the same optimal action (both left or both right), while the other state transitioned the same way independent of action.  I will call a state definite if it has an optimal action and indefinite if transition is independent of action.  Note that an optimal action in a definite state leads to a transition to the other definite state.  This leads to twelve different MDPs, because there are 3 options for high reward state, 2 options for optimal action, and 2 options for the indefinite state.  Each participant completed 20 actions, and at each time step saw a diagram of which state they were in.

Let $$a_{ji}$$ be the $$i$$-th action chosen by participant $$j$$, and $$s_{ji}$$ be the $$i$$-th state shown to participant $$j$$. 
I defined an optimality function $$f(a_{ji},s_{ji})=1_{a_{ji}\text{ was optimal for state }s_{ji}} + .5\cdot{1_{s_{ji} \text{ is indefinite}}}$$ and took $$Y_j = \frac{1}{20} \sum_{i=1}^{20} f(a_{ji}, s_{ji})$$.  This gives a value between 0 and 1, where higher values mean more optimal choices were made.

# Outcome Regression
If we encode the MDP type using a one-hot encoding (for MDP type $$k$$ we form a vector with 1 in the $$k$$-th position and 0 everywhere else), we can then perform outcome regression.  I used an ordinary least squares linear model from the Python package `statsmodels`.  In the end I implemented 7 models and then used the Akaike Information Criterion (AIC) to assess model fit.  Note that AIC values are relative, with lower being better, and lower by >2 is significantly better.

We have between 7 and 15 trials of each MDP type.  As a result, I tried grouping the data based on the three characteristics identified previously to overcome some of the instability that occurs with such small sample sizes.

## Model 1
Treating each of the 12 MDP types separately results in an AIC of -108.3.  The summary data is below.  


![Summary statistics for Model 1](/assets/images/reg1.png)

## Model 2
If we assume that participants recognized that the orientation of the diagram was irrelevant so we can reorder the states to remove the stratification by placement of the high reward state, we can group the MDP types into 4 groups of 3.  Fitting a linear regression in the same manner as before results in an AIC of -117.3.  This is a significant improvement over the prior model.  The summary data is below.


![Summary statistics for Model 2](/assets/images/reg2.png)

## Model 3
If we assume participants are not affected by which action is optimal, we get six pairs of MDPs.  This results in an AIC of -114.1.  Therefore Model 2 is significantly better. The summary data is below.

![Summary statistics for Model 3](/assets/images/reg4.png)

## Model 4
If we assume that the position of the indefinite state is irrelevant, we again arrive at six pairs of MDPs.  This gives an AIC of -112.8, so again Model 2 is significantly better. The summary data is below.

![Summary statistics for Model 4](/assets/images/reg7.png)

## Model 5
Further reducing the categories by assuming that only the optimal action is relevant gives an AIC of -118.5, which is not a significant improvement over Model 2. However, neither is Model 2 significantly better than Model 5. The summary data is below.

![Summary statistics for Model 5](/assets/images/reg3.png)

## Model 6
If we instead pay attention only to the location of the high reward state, we find an AIC of -116.7.  This is similar to Models 2 and 5. The summary data is below.

![Summary statistics for Model 6](/assets/images/reg6.png)

## Model 7
Finally, if we consider just the location of the indefinite state relative to the high-reward state, we get an AIC of -121.3. This is significantly better than any other model. The summary data is below.

![Summary statistics for Model 7](/assets/images/reg5.png)

# Results
Since we have a randomized control trial, and only look at the first MDP, identification is a straight forward application of the usual assumptions.  With the linear regression model, it amounts to taking the difference of the coefficients of the group $$m_j$$ belongs to and the group $$m_j'$$ belongs to.  Using Model 7 since it had the lowest AIC by far, we therefore arrive at $$\mathbb{E}[Y(M_1)-Y(M_2)] \approx \mathbb{E}[Y|M_1] - \mathbb{E}[Y|M_2] = 0.5074-0.4633 = 0.0441$$ where $$M_1$$ is any of the MDPs where the indefinite state is counterclockwise from the high reward state and $$M_2$$ is any of the MDPs where the indefinite state is clockwise from the high reward state.  This implies people make more optimal choices when the indefinite state is counterclockwise from the high reward state.

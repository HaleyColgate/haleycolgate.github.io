---
layout: default
title: Estimation 
--- 
# The Data
In the Seqer dataset, if we consider just the first MDP played by each participant the MDPs all have 3 states, the transitions were deterministic, and the actions were binary.  Rewards depended only on the next state.  Two states each had reward probability 0.2, while the other state had reward probability 0.8.  The high-reward state and one low-reward state had the same optimal action (both left or both right), while the other state transitioned the same way independent of action.  I will call a state definite if it has an optimal action and indefinite if transition is independent of action.  Note that an optimal action in a definite state leads to a transition to the other definite state.  This leads to twelve different MDPs, because there are 3 options for high reward state, 2 options for optimal action, and 2 options for the indefinite state.  Each participant completed 20 actions, and at each time step saw a diagram of which state they were in.

Let $$a_{ji}$$ be the $$i$$-th action chosen by participant $$j$$, and $$s_{ji}$$ be the $$i$$-th state shown to participant $$j$$. 
I defined an optimality function $$f(a_{ji},s_{ji})=1_{a_{ji}\text{ was optimal for state }s_{ji}} + .5\cdot{1_{s_{ji} \text{ is indefinite}}}$$ and took $$Y_j - \frac{1}{10} \sum_{i=1}^{20} f(a_{ji}, s_{ji})$$.  This gives a value between 0 and 1, where higher values mean more optimal choices were made.  Note that I am only considering the last 10 choices, assuming that the first 10 will be mostly random as people explore the problem.

# Outcome Regression
If we encode the MDP type using a one-hot encoding (for MDP type $$k$$ we form a vector with 1 in the $$k$$-th position and 0 everywhere else), we can then perform outcome regression.  I used an ordinary least squares linear model from the Python package `statsmodels`.  In the end I implemented 7 models and then used the Akaike Information Criterion (AIC) to assess model fit.  Note that AIC values are relative, with lower being better, and lower by >2 is significantly better.

We have between 7 and 15 trials of each MDP type.  As a result, I tried grouping the data based on the three characteristics identified previously to overcome some of the instability that occurs with such small sample sizes.

## Model 0
Treating each of the 12 MDP types separately results in an AIC of -30.67.  The summary data is below.  

![Model 0](/assets/images/reg0.png)


## Model 1
If we assume that participants recognized that the orientation of the diagram was irrelevant so we can reorder the states to remove the stratification by placement of the high reward state, we can group the MDP types into 4 groups of 3.  Fitting a linear regression in the same manner as before results in an AIC of -34.33.  This is significantly better than the prior model.  The summary data is below.

![Model 1](/assets/images/reg1.png)


## Model 2
If we assume participants are not affected by which action is optimal, we get six pairs of MDPs.  This results in an AIC of -32.50.  Therefore Model 1 is better but not significantly. The summary data is below.

![Model 3](/assets/images/reg3.png)

## Model 3
If we assume that the position of the indefinite state is irrelevant, we again arrive at six pairs of MDPs.  This gives an AIC of -35.66, so this is significantly better than Model 0 and Model 2, but on par with Model 1. The summary data is below.

![Model 6](/assets/images/reg6.png)

## Model 4
Further reducing the categories by assuming that only the optimal action is relevant gives an AIC of -35.03, which is again better than Model 2, but comparable with Model 1 and Model 3. The summary data is below.

![Model 2](/assets/images/reg2.png)

## Model 5
If we instead pay attention only to the location of the high reward state, we find an AIC of -34.44.  This is comparable to the previous models. The summary data is below.

![Model 5](/assets/images/reg5.png)

## Model 6
Finally, if we consider just the location of the indefinite state relative to the high-reward state, we get an AIC of -38.02. This is the lowest AIC, and is a significant improvement. The summary data is below.

![Model 4](/assets/images/reg4.png)

# Results
Since we have a randomized control trial, and only look at the first MDP, identification is a straight forward application of the usual assumptions.  With the linear regression model, it amounts to taking the difference of the coefficients of the group $$m_j$$ belongs to and the group $$m_j'$$ belongs to.  Using Model 6 since it was the best, we arrive at $$0.5372-0.5979=-0.0607$$ where for MDPs in Group 1 the indefinite state is counterclockwise from the high-reward state and for MDPs in Group 2 the indefinite state is clockwise from the high-reward state.

This seems to suggest that performance is better when the indefinite state is clockwise from the high-reward state, and this characterizes the difference between MDPs.

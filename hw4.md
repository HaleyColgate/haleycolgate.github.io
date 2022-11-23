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
Treating each of the 12 MDP types separately results in an AIC of -71.85.  The summary data is below.  

![Model 0](/assets/images/reg0.png)


## Model 1
If we assume that participants recognized that the orientation of the diagram was irrelevant so we can reorder the states to remove the stratification by placement of the high reward state, we can group the MDP types into 4 groups of 3.  Fitting a linear regression in the same manner as before results in an AIC of -67.30.  This is significantly worse than the prior model.  The summary data is below.

![Model 1](/assets/images/reg1.png)


## Model 2
If we assume participants are not affected by which action is optimal, we get six pairs of MDPs.  This results in an AIC of -77.72.  Therefore Model 2 is significantly better. The summary data is below.

![Model 3](/assets/images/reg3.png)

## Model 3
If we assume that the position of the indefinite state is irrelevant, we again arrive at six pairs of MDPs.  This gives an AIC of -66.24, so again Model 2 is significantly better. The summary data is below.

![Model 6](/assets/images/reg6.png)

## Model 4
Further reducing the categories by assuming that only the optimal action is relevant gives an AIC of -69.52, which is again worse than Model 2. The summary data is below.

![Model 2](/assets/images/reg2.png)

## Model 5
If we instead pay attention only to the location of the high reward state, we find an AIC of -67.25.  This is again worse than Model 2. The summary data is below.

![Model 5](/assets/images/reg5.png)

## Model 6
Finally, if we consider just the location of the indefinite state relative to the high-reward state, we get an AIC of -70.51. The summary data is below.

![Model 4](/assets/images/reg4.png)

# Results
Since we have a randomized control trial, and only look at the first MDP, identification is a straight forward application of the usual assumptions.  With the linear regression model, it amounts to taking the difference of the coefficients of the group $$m_j$$ belongs to and the group $$m_j'$$ belongs to.  Using Model 3 since it was the best model, we arrive at 

|--------|-----|-----------|------------|-------------|------------|------------|-------------|
|        |     |           |            |    $$m_j'$$ |            |            |             |
|--------|-----|-----------|------------|-------------|------------|------------|-------------|
|        |     |   g1      |    g2      |    g3       |   g4       |   g5       |   g6|
|--------|-----|-----------:|------------:|-------------:|------------:|------------:|-------------:|
|        |g1   |   +0.0000 |    +0.1074 |    +0.1068  |   -0.0528  |   -0.0058  |   -0.0462|
|        |g2   |   -0.1074 |    +0.0000 |    -0.0006  |   -0.1602  |   -0.1132  |   -0.1536|
|$$m_j$$ |g3   |   -0.1068 |    +0.0006 |    +0.0000  |   -0.1596  |   -0.1126  |   -0.1530|
|        |g4   |   +0.0528 |    +0.1602 |    +0.1596  |   +0.0000  |   +0.0470  |   +0.0066|
|        |g5   |   +0.0058 |    +0.1132 |    +0.1126  |   -0.0470  |   +0.0000  |   -0.0404|
|        |g6   |   +0.0462 |    +0.1536 |    +0.1530  |   -0.0066  |   +0.0404  |   +0.0000|
|--------|-----|-----------|------------|-------------|------------|------------|-------------|

where the groups have the following characteristics:

|-----|-----------------------|-----------------------------------------|----|
|        |high   |        indefinite |    coefficient    |
|-----|:-----------------------:|-----------------------------------------|----|
|g1      |0                    |       counter-clockwise| 0.5250 |
|g2      |2                     |      clockwise        | 0.4176 |
|g3     | 1                      |     counter-clockwise| 0.4182 |
|g4    |  0                       |    clockwise        | 0.5778 |       
|g5   |   2                        |   counter-clockwise| 0.5308 |
|g6  |    1                         |  clockwise        | 0.5712 |
|-----|-----------------------|-----------------------------------------|


It was suprising to me that ignoring the optimal action led to a better model, because I thought it would be the dominant factor and mediate the effect of the relative location of the indefinite state.  There does seem to be 3 groupings in the current model, but unifying factors in the groupings are not apparent to me at this time.

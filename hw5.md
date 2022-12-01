---
layout: default
title: Sensitivity Analysis
--- 

One strength of this experimental design is that because the MDPs were randomly designed, we do not have to worry about unmeasured confounding.  However, we still have the SUTVA and consistency assumptions that are untestable.

The major assumptions I made were in the groupings I considered for the models.  Since the data is categorical, Model 0 is fully saturated so I have a baseline model that does not assume linearity in any of the parameters.  However, the six other models I evaluated assume that MDPs within each group can be treated interchangeably.  They are also not the only possible groupings, but the number of possible groupings makes testing them all impractical.

The other major assumption I made was that people finished the exploratory part of their problem solving by the last 10 steps.  One way of seeing if that is true, is to repeat the same analysis using the data from when half of the participants played their first MDP over again.  This is the resulting analysis:

## Model 0
Treating each of the 12 MDP types separately results in an AIC of 24.96.  The summary data is below.  

![Model 0](/assets/images/reg0_2.png)


## Model 1
If we assume that participants recognized that the orientation of the diagram was irrelevant so we can reorder the states to remove the stratification by placement of the high reward state, we can group the MDP types into 4 groups of 3.  Fitting a linear regression in the same manner as before results in an AIC of 12.52.  This is significantly better than the prior model.  The summary data is below.

![Model 1](/assets/images/reg1_2.png)


## Model 2
If we assume participants are not affected by which action is optimal, we get six pairs of MDPs.  This results in an AIC of 15.01.  The summary data is below.

![Model 3](/assets/images/reg3_2.png)

## Model 3
If we assume that the position of the indefinite state is irrelevant, we again arrive at six pairs of MDPs.  This gives an AIC of 16.44. The summary data is below.

![Model 6](/assets/images/reg6_2.png)

## Model 4
Further reducing the categories by assuming that only the optimal action is relevant gives an AIC of 9.223, which is significantly better than Model 1. The summary data is below.

![Model 2](/assets/images/reg2_2.png)

## Model 5
If we instead pay attention only to the location of the high reward state, we find an AIC of 11.19.  This is slightly worse than the previous model. The summary data is below.

![Model 5](/assets/images/reg5_2.png)

## Model 6
Finally, if we consider just the location of the indefinite state relative to the high-reward state, we get an AIC of 9.222. This is the lowest AIC, but is not a significant improvement. The summary data is below.

![Model 4](/assets/images/reg4_2.png)

# Results
This complicates the interpretation of the prior results.  Here Model 4 and Model 6 are a little better than Model 5, but neither is clearly better than the other.  It seems that grouping based on any of the three characteristics individually leads to a better model than combining them.

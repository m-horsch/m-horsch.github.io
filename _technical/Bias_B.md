---
layout: post
title:  "Bias #2: Cycle Checking"
date:   2026-07-03
categories:  Technical
usemathjax: true
---
<style>
table
{
    max-width: 300px;
    margin-left:auto; 
    margin-right:auto;  
}
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>

### Abstract
This is the second of four articles investigating bias in my process for Toroidal generation.  I've written [an article about the possible sources of bias](Bias_Intro), which sets the context for this article.

To generate Toroidal puzzles for game-play or experiment, I use a program that applies a number of legal moves chosen randomly.  The first variant puts no constraint on the way moves are chosen.  The second variant, which uses cycle checking, prohibits the random walk from returning to any state that it had previously been in, since the walk started.  This article compares the datasets created using these two variants and shows that both techniques produce 3x3 Toroidal samples that match the 3x3 Toroidal population distribution, provided that sufficiently many examples are generated, and that sufficiently many steps are taken.  I presume that this will extend to 4x4 Toroidals as well, though this remains to be demonstrated.


### Variations on a Sampling Procedure
My sampling procedure starts with a known initial configuration, and applies a sequence of randomly chosen legal moves, to shuffle the tiles around.  The original version of the Toroidal generating procedure put no constraint on the random moves, other than that they had to be legal row and column moves.  A slightly modified version of the process used a technique to prevent cycles in the random walk.  Until the experiments discussed in this article, I did not know which of the two versions should be preferred, and whether either of them produced biased samples.

Preventing cycles is easy.  Just keep a record of all the configurations accepted by the randomization procedure already, and check if a random move would produce a configuration that has already been recorded.  If so, discard the random move, and try another.  If not, accept the move and the record new configuration.  It's possible that this method of preventing cycles might leave the sampler with no valid move to another state.  However this would be obvious as the program would get stuck in an infinite loop.  This has never occurred in any of my many applications of this technique.


### Experiment
To compare cycle checking (CC) to a simpler randomization without cycle checking (NC), I created 8 datasets, each with $$K = 1000$$; each dataset has $$N=5000$$ 3x3 Toroidal problems.  These values of $$K$$ and $$N$$ are the same values used in the [previous article.](Bias_A) 

In 4 of these datasets, I used cycle checking.  These are labelled "CCP" in the plot below.  In the other 4 datasets, labelled "NCP," there was no cycle checking.  Both methods, CCP and NCP used the slight random perturbation of $$K$$ described in the [previous article.](Bias_A)  Since these are all 3x3 Toroidals, I can simply apply the [DIST](/solver/DIST) distance measure to each example, and output the true distance to the goal.  

### Results
From the results, I generated a histogram, shown below.

![Histogram](/TImages/BiasHistogram_CycleCheckStudy.png)

The histogram shows the proportion of the datasets plotted against their optimal solution lengths.  The true proportions, as found in the entire population, are shown in the plot as short dashed lines.  These were reported [here.](/technical/Distribution)

All 8 of the datasets are consistent with the theoretical values of the population.  More importantly, the two methods (NCP and CCP) are consistent with each other, having no obvious significant deviations.   There are random deviations in the heights of the different datasets, but this is well within what should be expected from random samples. 

As in the previous article,  I performed a Z-test, using the averages of each of the 8 datasets, as well as the known values from the entire population.  Briefly, this test checks if the sample averages are significantly different from the known, true average, and gives a p-value as a result.  The higher the p-value, the less likely that there is a difference.  The statistical values are presented below:

| Dataset | z-Value  | p-Value |
|---------|----------|---------|
|  CCP1     | 0.2346   |0.8145| 
| **CCP2**  | **2.2792** |**0.0227**|
|  CCP3     | 0.9450   |0.3447| 
|  CCP4     | 0.9450   |0.3447| 
|         |          |         |
|  NCP1   |  -0.0253  | 0.9798 | 
|  NCP2   |   1.8114  | 0.0701 | 
|  NCP3   |   1.2049  | 0.2282 | 
|  NCP4   |   0.3039  | 0.7612 | 
     
Because the "original" sampler used cycle checking, the datasets labelled CCP here are the same datasets labeled more simply "P" in the [previous article](Bias_A).  The z-Value and p-Value for datasets CCP3 and CCP4 are coincidentally equal to 4 digits; I checked that the input data and output data are different!

All but one of the p-values are greater than 0.05, which is a common threshold for significance.  So we can be fairly sure that the samples generated by both methods represent the population sufficiently well.  

### Conclusions
For the given values $$K=1000$$ and $$N=5000$$, there is no significant difference between cycle checking, and no cycle checking, when the slight random perturbation of $$K$$ is used.  Cycle checking is more expensive, so the sampler that does no checking can be used to save a bit of time.  

**Looking forward.**
In the [next article](Bias_C), I'm going to address the magnitude of $$K$$, the number of random actions applied to the initial configuration.  In this article, and the previous, I chose a value for $$K$$ which I hoped would be "large enough" not to cause any bias.  I'll explain why I believed that to be true, and what can be done to be more scientific about the choice of $$K$$.  

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_B).

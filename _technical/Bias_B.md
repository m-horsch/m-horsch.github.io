---
layout: post
title:  "Addressing Bias in Toroidal Generation: Cycle Checking"
date:   2026-05-02
categories:  Technical
usemathjax: true
---
<style>
table
{
    max-width: 0px;
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

# Draft!!

### Abstract
To generate Toroidal puzzles for game-play or experiment, I use a program that applies a number of legal moves chosen randomly.  The process can be described as a random walk through [the state space](/solver/StateSpace), starting at a specific location in the space.  I have two variants of the process.  The first variant puts no constraint on the way moves are chosen.  The second variant, which uses cycle checking, prohibits the random walk from returning to any state that it had previously been in, since the walk started.  This article compares the datasets created using these two variants and shows that both techniques produce 3x3 Toroidal samples that match the 3x3 Toroidal population distribution, provided that sufficiently many examples are generated, and that sufficiently many steps are taken.  I presume that this will extend to 4x4 Toroidals as well, though this remains to be demonstrated.

### Background
When I create Toroidal puzzles for the Toroidal game, or when I explore strategies to solve Toroidal puzzles, I have to generate a randomized configuration for these puzzles using a computer program.  In statistical language, creating a randomized configuration is called sampling, and the randomized configuration is called a sample.  I have described how I create randomized configurations in previous articles, but I will summarize briefly here.  Basically, the sampling program chooses random legal moves, and applies these moves to a given goal configuration, resulting in random shuffling of rows and columns.  

There are several choices that must be made to generate a sample.
  1. The magnitude of $$K$$, the number of random actions applied to the initial configuration.
  2. The parity (even or odd) of $$K$$.  **Note: This has already been addressed in previous articles.**
  4. The sampling program that generates a sample, given $$K$$.

When I am generating a dataset of samples, I also have to choose $$N$$, which is the number of samples in the dataset.

These choices are possibly mutually dependent, and if I want to discover and eliminate possible biases, I have to proceed carefully. No matter which of these I choose to tackle first, I have to make assumptions about the others. My plan is as follows.
  3. Verify that all sampling programs are unbiased, using the large value for $$K$$.
  4. Find a practical value for $$K$$ that is large enough to generate unbiased samples, but not so large that we waste time.
  5. Find a practical value for $$N$$ that balances confidence in future experiments, with efficiency. 

Note: I have already addressed the following steps:
  1. First, choose $$K$$ and $$N$$ sufficiently large, larger than what I might expect as  minimum values needed to assure that sampling is consistent with the population.
  2. The known parity bias can be addressed by using [slight random perturbations of $$K$$](Bias_A) in the sampling process.

### Variations on a Sampling Procedure
My sampling procedure starts with a known initial configuration, and applies a sequence of randomly chosen legal moves, to shuffle the tiles around.  At the end of the sequence, the resulting configuration is used as a starting configuration for the solver, and the known initial configuration is used as the goal configuration for the solver.  

The original version of the Toroidal generating procedure put no constraint on the random moves, other than that they had to be legal row and column moves.  At some point, I became worried that the resulting configurations could be too easy, as nothing prevented the random sequence from shuffling the same row back and forth a few times.  To avoid such useless shuffling, I implemented a technique called cycle checking.  

To understand what a cycle is, first consider the random moves applied to a Toroidal.  Each move changes the configuration a little.  A sequence of moves creates a corresponding sequence of configurations.  A sequence would have a cycle if any configuration were repeated.  This could happen with a trivial undoing of a move, e.g., `L1 R1`, or any number of longer sequences.  

I felt that cycles were a problem, because I thought that they would introduce a bias towards optimal solutions that were shorter than expected in the population.  More recently, I began to wonder if adding the cycle checking would create Toroidal configurations that were biased towards optimal solutions that were longer than expected in the population.  I had never checked either way.  This article checks.

Preventing cycles is easy.  Just keep a record of all the configurations accepted by the randomization procedure already, and check if a random move would produce a configuration that has already been recorded.  If so, discard the random move, and try another.  If not, accept the move and the record new configuration.  


### Experiment
To compare cycle checking (CC) to a simpler randomization without cycle checking (NC), I created 8 datasets, each with $$K = 1000$$; each dataset has $$N=5000$$ 3x3 Toroidal problems.  In 4 of these datasets, I used cycle checking.  These are labelled "CCP" in the plot below.  In the other 4 datasets, labelled "NCP," there was no cycle checking.  Both methods, CCP and NCP used the slight random perturbation of $$K$$ described in the [previous article.](Bias_A)   Since these are all 3x3 Toroidals, I can simply apply the [DIST](/solver/DIST) distance measure to each example, and output the true distance to the goal.  

### Results
From the results, I generated a histogram, shown below.

![Histogram](/TImages/BiasHistogram_CC.png)

The histogram shows the proportion of the datasets plotted against their optimal solution lengths.  The true proportions, as found in the entire population, are shown in the plot as short dashed lines.  These were reported [here.](/technical/Distribution)

All 8 of the datasets are consistent with the theoretical values in the table.  More importantly, the two methods (NCP and CCP) are consistent with each other, having no obvious or significant deviations.   There are random deviations in the heights of the different datasets, but this is well within what should be expected from random samples. 

As for the previous article,  I performed a Z-test, using the averages of each of the 8 datasets, as well as the known values from the entire population.  Briefly, this test checks if the sample averages are significantly different from the known, true average, and gives a p-value as a result.  The higher the p-value, the less likely that there is a difference.  The statistical values are presented below:

| Dataset | z-Value  | p-Value |
|---------|----------|---------|
|  CCP1   | -0.1119  | 0.9109  | 
|  CCP2   |  0.5292  | 0.5967  |  
|  CCP3   | -1.5847  | 0.1130  |  
|  CCP4   |  0.8064  | 0.4200  | 
|         |          |         |
|  NCP1   |  0.1826  | 0.8551  | 
|  NCP2   |  0.5638  | 0.5729  | 
|  NCP3   |  1.3262  | 0.1848  | 
|  NCP4   |  1.4128  | 0.1577  | 
     
Because the "original" sampler used cycle checking, the datasets labelled CCP here are the same datasets labeled more simply "P" in the [previous article](Bias_A).  

All the p-values are greater than 0.05, which is a common threshold for significance.  So we can be fairly sure that the samples generated by both methods represent the population sufficiently well.

### Conclusions
For the given values $$K=1000$$ amd $$N=5000$$, there is no significant difference between cycle checking, and no cycle checking, when the slight random perturbation of $$K$$ is used.  Cycle checking is more expensive, so the sampler that does not checking can be used to save a bit of time.  

**Looking forward.**
In the [next article](Bias_C), I'm going to address the magnitude of $$K$$, the number of random actions applied to the initial configuration.  In this article, and the previous, I chose a value for $$K$$ which I hoped would be "large enough" not to cause any bias.  I'll explain why I believed that to be true, and what can be done to be more scientific about the choice of $$K$$.  

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_B).

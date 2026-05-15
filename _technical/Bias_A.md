---
layout: post
title:  "Addressing Bias in Toroidal Generation: Parity Correlation"
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
I have recently become aware of a small number of possible biases in the procedures I use to generate initial Toroidal configurations. This article is focussed on one of these, namely an unexpected correlation between the parity (even or odd) of the length $$K$$ of the initialization sequence, and the parity (even or odd) of the length of a solution, in the context of generating 4x4 Toroidals.  A simple fix is to use a slight random perturbation applied to the specified $$K$$, so that any dataset generated would contain samples with a value close to $$K$$ but randomly even or odd, with equal probability.  The article demonstrates visually and statistically that the technique produces 3x3 Toroidal samples that match the 3x3 Toroidal population distribution, provided that sufficiently many examples are generated, and that $$K$$ is sufficiently large.  I presume that this will extend to 4x4 Toroidals as well, though this remains to be demonstrated.

### Background
When I create Toroidal puzzles for the Toroidal game, or when I explore strategies to solve Toroidal puzzles, I have to generate a randomized configuration for these puzzles using a computer program.  In statistical language, creating a randomized configuration is called sampling, and the randomized configuration is called a sample.  I have described how I create randomized configurations in previous articles, but I will summarize briefly here.  Basically, the sampling program chooses random legal moves, and applies these moves to a given goal configuration, resulting in random shuffling of rows and columns.  

There are several choices that must be made to generate a sample.
  1. The magnitude of $$K$$, the number of random actions applied to the initial configuration.
  2. The parity (even or odd) of $$K$$.  
  4. The sampling program that generates a sample, given $$K$$.

When I am generating a dataset of samples, I also have to choose $$N$$, which is the number of samples in the dataset.

These choices are possibly mutually dependent, and if I want to discover and eliminate possible biases, I have to proceed carefully. No matter which of these I choose to tackle first, I have to make assumptions about the others. My plan is as follows.
  1. First, choose $$K$$ and $$N$$ sufficiently large, larger than what I might expect as  minimum values needed to assure that sampling is consistent with the population.
  2. Eliminate the known parity bias, using the large value for $$K$$.
  3. Verify that all sampling programs are unbiased, using the large value for $$K$$.
  4. Find a practical value for $$K$$ that is large enough to generate unbiased samples, but not so large that we waste time.
  5. Find a practical value for $$N$$ that balances confidence in future experiments, with efficiency.
  
### The Parity Bias
Not too long ago, I noticed a correlation using my sampling methods: for 4x4 Toroidals, if $$K$$ is even, any solution found has even length.  Likewise, if $$K$$ is odd, the solution has odd length.  The correlation occurs for fairly large values of $$K$$.  
It occurs in all my informal, preliminary explorations of solving 4x4 Toroidals. 
It doesn't seem to matter if the solution is optimal or not. This parity correlation does not appear in 3x3 Toroidals.  The vast majority of my work so far has been on 3x3 Toroidals, so I didn't notice this correlation until I declared my [3x3 work completed](/solver/3x3Done). Solving 4x4 Toroidals is significantly harder than 3x3, so my data is more limited, but I have seen no exceptions.   

This is clearly a bias in sampling.  It's important to avoid any kind of bias in an empirical study, because it casts doubt on the generality of any conclusions.  I don't think that any of my solvers explicitly rely on knowing the parity of $$K$$, but it's possible that a solver that has only been tested on data where $$K$$ is even might perform differently on data where $$K$$ is odd.  And once a bias like this is become apparent, we have to address it.
  

### The Work-Around
I'm not totally convinced that this correlation is a property of 4x4 Toroidals, but I am leaning in that direction.   I have not tried to prove it as a formal property of Toroidals.  But I take seriously the possibility that it still could be a flaw in my randomization procedures. 

If it is baked into 4x4 Toroidals, I won't be able to prevent the correlation.  However, I can prevent the solvers from knowing whether I used even or odd $$K$$, which is the next best thing.

I want to keep control of the values I use for $$K$$, the number of random legal moves to apply when generating a Toroidal configuration.  So $$K$$ cannot be totally random.  Instead, given a nominal value for $$K$$, my procedure randomly chooses to use $$K-1$$, $$K$$, or $$K+1$$ random legal moves.  I have it set up so that:
  1.  50% of the time the procedure uses $$K$$ without any perturbation.
  2.  25% of the time, $$K-1$$ is used.
  3.  25% of the time, $$K+1$$ is used.

As a result, half of the time, the number of random moves is even, and half the time it is odd.  

### Experiment
To study the effect of this slight perturbation of the given value for $$K$$, I have to compare the generation procedure with and without perturbation of $$K$$.

Prior to this experiment, I informally assessed different values for $$K$$ and $$N$$, and settled on $$K=1000$$ and $$N=5000$$.  Currently, I believe these are large enough to avoid biases that could be related to small values of these parameters.  If $$K$$ is too small, the samples may be too close to the goal configuration, and therefore could induce a bias.  Likewise, if $$N$$ is too small, bad luck could result in a dataset that only differs from the population by chance; the larger $$N$$ is, the less likely that is to happen.  Currently, I have no reason to be concerned about $$K$$ or $$N$$ being too large, except for the extra time needed when using them.

So I created 8 datasets, each with $$K = 1000$$; each dataset has $$N=5000$$ 3x3 Toroidal problems.  In 4 of these datasets, I used the old method, without perturbing $$K$$.  These are labelled "U" in the results (i.e., Unperturbed).  In the other 4 datasets, the value of $$K$$ was modified as stated above. These are labelled "P" in the results (i.e. Perturbed).  Since these are all 3x3 Toroidals, I can simply apply the [DIST](/solver/DIST) distance measure to each example, and output the true distance to the goal.  

### Results
From the experimental results, I generated a histogram of distance values reported by DIST, shown below.

![Histogram](/TImages/BiasHistogram_PU.png)

The histogram shows the proportion of the datasets plotted against distance.  There are random deviations in the heights of the different datasets, but this is well within what should be expected from random samples.  The true proportions, as found in the entire population, are shown in the plot as short dashed lines.  These were reported [here.](/technical/Distribution).

All 8 of the datasets are consistent with the known proportions.  More importantly, the two methods (U and P) are consistent with each other, having no obvious or significant deviations.

Another way to look at the data is to look at the average solution length for the 8 samples.  Here's a plot:

![Error Plot](/TImages/BiasError_PU.png)

The plot shows a running average in the relative error.  The horizontal axis shows how many of the samples were used in calculating an average.  The vertical axis shows the relative error a a running average, relative to the true average distance.  The closer the data get to the horizontal line showing zero relative error, the better.  As we can see, when the number of samples is quite small, the averages are not very close to the true average.  As we use more and more samples, the averages settle down, but never quite get to zero.  This is expected.  

To give a bit of statistical formality to the visual story, I performed a Z-test, using the averages of each of the 8 datasets, as well as the known values from the entire population.  Briefly, this test checks if the sample averages are significantly different from the known, true average, and gives a p-value as a result.  It's unfortunate that I chose to use the label "P" to name my datasets, since a lower case "p" is used in the statistical "p-value."  The higher the p-value, the less likely that there is a difference.  The statistical values are presented below:

| Dataset | z-Value  | p-Value |
|---------|----------|---------|
|  P1     | -0.1119  | 0.9109  | 
|  P2     |  0.5292  | 0.5967  |  
|  P3     | -1.5847  | 0.1130  |  
|  P4     |  0.8064  | 0.4200  | 
|         |          |         |
|  U1     |  1.9153  | 0.0555  | 
|  U2     | -0.7877  | 0.4309  | 
|  U3     |  0.3039  | 0.7612  | 
|  U4     |  0.1306  | 0.8961  | 
     

All the p-values are greater than 0.05, which is a common threshold for significance.  So we can be fairly sure that the samples generated by both methods represent the population sufficiently well.  

Informally, I will report that I have rerun this test multiple times, correcting for minor technical issues unrelated to the results.  Each time I run the test, I get the same results: the p-values indicate that the sampling methods are unbiased.

### Conclusions
For large $$K$$ and large $$N$$, slightly perturbing the given value $$K$$ does not create 3x3 Toroidals that differ significantly from what's expected theoretically, nor does it produce samples that are significantly different from the kinds of samples generated without such perturbations.  The perturbation hides the actual number of random moves used to create a sample, and therefore no solver can exploit the parity of $$K$$.  Future work will have to demonstrate that slightly perturbing $$K$$ will allow my sampling programs to generate 4x4 data without any bias.

**Looking forward.**
In the next article, I'll address another possible bias of the generation procedure, namely the issue of [cycle checking](bias_B).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_A).

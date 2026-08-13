---
layout: post
title:  "Bias #1: Parity"
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
This is the first of four articles investigating bias in my process for Toroidal generation.  I've written [an article about the possible sources of bias](Bias_Intro), which sets the context for this article.

I have recently become aware of an unexpected correlation between the parity (even or odd) of the length $$K$$ of the initialization sequence, and the parity (even or odd) of the length of a solution, in the context of generating 4x4 Toroidals.  A simple fix is to use a slight random perturbation applied to the specified $$K$$, so that any dataset generated would contain samples with a value close to $$K$$ but randomly even or odd, with equal probability.  The article demonstrates visually and statistically that the technique produces 3x3 Toroidal samples that match the 3x3 Toroidal population distribution, provided that sufficiently many examples are generated, and that $$K$$ is sufficiently large.  I presume that this will extend to 4x4 Toroidals as well, though this remains to be demonstrated.

### The Parity Correlation Bias
There is a parity correlation in my sampling methods: for 4x4 Toroidals, if $$K$$ is even, any solution found has even length.  Likewise, if $$K$$ is odd, the solution has odd length.  The correlation occurs for fairly large values of $$K$$. It occurs in all my informal, preliminary explorations of solving 4x4 Toroidals. It doesn't seem to matter if the solution is optimal or not. 

This correlation may be a property of 4x4 Toroidals, but I am not entirely sure as of this writing.  I suspect that the correlation will be present for Toroidals of even dimension, e.g., 6x6, etc.  However, I take seriously the possibility that it still could be a flaw in my randomization procedures.  

This parity correlation does not appear in 3x3 Toroidals.  The vast majority of my work so far has been on 3x3 Toroidals, so I didn't notice this correlation until I declared my [3x3 work completed](/solver/3x3Done). Solving 4x4 Toroidals is significantly harder than 3x3, so my data is more limited, but I have seen no exceptions.   

I don't think that any of my solvers explicitly rely on knowing the parity of $$K$$, but it's possible that a solver that has only been tested on data where $$K$$ is even might perform differently on data where $$K$$ is odd.  And once a bias like this is become apparent, we have to address it.
  

### The Work-Around
If this correlation is a mathematical property of 4x4 Toroidals, I won't be able to prevent the correlation.  However, I can prevent the solvers from knowing whether I used even or odd $$K$$, which is the next best thing.

I want to keep control of the values I use for $$K$$, the number of random legal moves to apply when generating a Toroidal configuration.  So $$K$$ cannot be totally random.  Instead, given a nominal value for $$K$$, my procedure randomly chooses to use $$K-1$$, $$K$$, or $$K+1$$ random legal moves.  I have it set up so that:
  1.  50% of the time the procedure uses $$K$$ without any perturbation.
  2.  25% of the time, $$K-1$$ is used.
  3.  25% of the time, $$K+1$$ is used.

As a result, half of the time, the number of random moves is even, and half the time it is odd.  

### Experiment
To study the effect of this slight perturbation of the given value for $$K$$, I have to compare the generation procedure with and without perturbation of $$K$$.

Prior to this experiment, I informally assessed different values for $$K$$ and $$N$$, and settled on $$K=1000$$ and $$N=5000$$.  Currently, I believe these are large enough to avoid biases that could be related to small values of these parameters for 3x3 Toroidals.

So I created 8 datasets, each with $$K = 1000$$; each dataset has $$N=5000$$ 3x3 Toroidal problems.  In 4 of these datasets, I used the old method, without perturbing $$K$$.  These are labelled "U" in the results (i.e., Unperturbed).  In the other 4 datasets, the value of $$K$$ was modified as stated above. These are labelled "P" in the results (i.e. Perturbed).  Since these are all 3x3 Toroidals, I can simply apply the [DIST](/solver/DIST) distance measure to each example, and output the true distance to the goal.  

### Results
From the experimental results, I generated a histogram of distance values reported by DIST, shown below.

![Histogram](/TImages/BiasHistogram_ParityStudy.png)

The histogram shows the proportion of the datasets plotted against distance.  There are random deviations in the heights of the different datasets, but this is well within what should be expected from random samples.  The true proportions, as found in the entire population, are shown in the plot as short dashed lines.  These were reported [here.](/technical/Distribution)

All 8 of the datasets are consistent with the known proportions.  More importantly, the two methods (U and P) are consistent with each other, having no obvious or significant deviations.

Another way to look at the data is to look at the average solution length for the 8 samples.  Here's a plot:

![Error Plot](/TImages/BiasError_ParityStudy.png)

The plot shows a running average in the relative error.  The horizontal axis shows how many of the samples were used in calculating an average.  The vertical axis shows the relative error of each running average, relative to the true average distance.  The closer the data get to the horizontal line showing zero relative error, the better.  As we can see, when the number of samples is quite small, the averages are not very close to the true average.  As we use more and more samples, the averages settle down, but never quite get to zero.  This is expected.  

To give a bit of statistical formality to the visual story, I performed a Z-test, using the averages of each of the 8 datasets, as well as the known values from the entire population.  Briefly, this test checks if the sample averages are significantly different from the known, true average, and gives a p-value as a result.  It's unfortunate that I chose to use the label "P" to name my datasets, since a lower case "p" is used in the statistical "p-value."  The higher the p-value, the less likely that there is a difference.  The statistical values are presented below:

| Dataset | z-Value  | p-Value |
|---------|----------|---------|
|  P1     | 0.2346   |0.8145| 
| **P2**  | **2.2792** |**0.0227**|
|  P3     | 0.9450   |0.3447| 
|  P4     | 0.9450   |0.3447| 
|         |          |         |
|  U1     | -0.2159  |0.8291| 
|  U2     | -1.2902  |0.1970| 
|  U3     | 0.1133   |0.9098| 
|  U4     | -0.8743  |0.3819| 
     

All but one of the p-values are greater than 0.05, which is a common threshold for significance.  So we can be fairly sure that the samples generated by both methods represent the population sufficiently well.  

Informally, I will report that I have rerun this test multiple times, correcting for minor technical issues unrelated to the results.  Each time I run the test, I get a different set of samples, but the p-values consistently indicate that the sampling methods are unbiased.  The z-Value and p-Value for datasets P3 and P4 are coincidentally equal to 4 digits; I checked that the input data and output data are different!

### Conclusions
For large $$K$$ and large $$N$$, slightly perturbing the given value $$K$$ does not create 3x3 Toroidals that differ significantly from what's expected theoretically, nor does it produce samples that are significantly different from the kinds of samples generated without such perturbations.  The perturbation hides the actual number of random moves used to create a sample, and therefore no solver can exploit the parity of $$K$$.  Future work will have to demonstrate that slightly perturbing $$K$$ will allow my sampling programs to generate 4x4 data without any bias.

**Looking forward.**
In the next article, I'll address another possible bias of the generation procedure, namely the issue of [cycle checking](bias_B).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_A).

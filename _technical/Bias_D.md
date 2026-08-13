---
layout: post
title:  "Addressing Data Set Size"
date:   2026-08-03
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
This is the final article investigating bias in my process for Toroidal generation.  I've written [an article about the possible sources of bias](Bias_Intro), which sets the context for this article.  Technically, the topic in this article is not really about a bias. 

To generate Toroidal puzzles for my experiments, I use a program that samples initial configurations from [the state space](/solver/StateSpace).  Previous articles have shown that the sampling process is unbiased.  In this article, I consider the number $$N$$ of samples needed to make rigorous comparisons between solving strategies for 3x3 Toroidals.  

A fairly standard statistical calculation predicts that $$N=250$$ should give me acceptable results.  Datasets created with $$N=250$$ match the population fairly well.  

### Background
Most of my 3x3 Toroidal work to date has been done using the same dataset of $$N=100$$ Toroidal configurations.  I chose this value without much thought.  I used the same dataset for all of my explorations, basically out of instinct.  I knew that I would be comparing different methods on the same data, and thus, even if the values I was measuring did not precisely describe the entire population of 3x3 Toroidals, I could at least draw conclusions on a relative basis.

In any empirical work involving repeated trials, there is a trade-off.  Typically, one wants the number of trials $$N$$ to be large enough to be sure that the results speak reasonably precisely about the phenomenon being studied, but not too large to be impractical, since the larger $$N$$ is, the longer the experiment takes to run. 

The important topic of sample size is covered quite thoroughly in statistics.  For any experiment involving repeated trials, we are trying to estimate a true quantity, by taking an average over the samples.  

I'm mostly interested in the time it takes for my solvers to solve a Toroidal puzzle, and if it were possible, I would use time as my criterion for choosing $$N$$.  However, since I have so many solvers, and many of them are quite slow, I can't actually do that practically.  

So instead, I will continue to use the optimal distance to the goal state, using DIST, as in the previous 3 articles.  As long as my dataset is big enough to be similar to the entire population, I am fairly confident that my timing experiments will not lead me astray.  

Very ordinary assumptions can be used to derive the following formula for $$N$$ to estimate an average over an entire population by taking an average of $$N$$ samples:

$$N = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2$$

This formula is a [standard result in statistics](https://en.wikipedia.org/wiki/Sample_size_determination).
It has 2 parameters that experimenters can choose.  The value $$E$$ is a margin of error, which basically describes a confidence interval around the estimated value; this depends on context, and the quantity we're trying to estimate.  The value $$\alpha$$ is the probability that the margin of error fails to contain the true value; typically scientists use $$\alpha = 0.05$$.  

The remaining values are not choices.  The value $$\sigma$$ is the square root of the variance $$\sigma^2$$ of the population, which we know for 3x3 Toroidals.  The last value is $$z_{\alpha/2}$$, which is called a *critical value*, and it comes from the standard normal distribution; when $$\alpha = 0.05$$, $$z_{0.025} = 1.96$$.

The only thing left to decide is $$E$$, the margin of error.  For distance to the goal, a plausible value is $$E=0.1$$, which says we would like the sample's estimate to be within $$\pm 0.1$$ of the true value.  

With these choices, we can calculate $$N$$ as follows:

$$N = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2 = \left(\frac{1.96 \cdot 0.816}{0.1}\right)^2 = 255.9$$

So, statistical arguments suggest that $$N=250$$ would be the right ballpark.  Let's see whether this is consistent with data in the  previous article.


### Empirical evidence for the effect of $$N$$
I used 6 datasets generated for the experiment from [the previous article](Bias_C).  The datasets were created using $$N=5000$$ and $$K=50$$.

![Error Plot](/TImages/BiasError_k50.png)

The plot shows a running average in the relative error.  The horizontal axis shows how many of the samples were used in calculating an average.  The vertical axis shows the relative error of each running average, relative to the true average distance.  The horizontal line through the middle of the plot represents zero relative error, compared to the known average solution length in the population.

The scale of the image is important.  The vertical axis is shown with a maximum value of 5%, which means that each dataset shows less than 5% error for almost all values of $$N$$.

The plot shows that the relative errors decrease to something less than 2% error after only a few hundred samples, slowly settle after a couple of thousand samples, but level off without actually decreasing to zero.   The population average for distance to the goal state is around 6.1, meaning that 2% error is about 0.12, which is a little larger than the value of $$E$$ I used above.  If we look at the plot around $$N=250$$, we can see that the data is well within the 2% range.  

### Verifying $$N=250$$
I created 8 datasets of 3x3 Toroidal problems, each with $$N=250$$.  These were generated using the [slight perturbation for $$K$$](Bias_A), without [cycle checking](Bias_B), and $$K=50$$ (as determined [here](Bias_C)).  

Since these are all 3x3 Toroidals, I applied the [DIST](/solver/DIST) distance measure to each example, and output the true distance to the goal.  From the experimental results, I generated a histogram of distance values reported by DIST, shown below.

![Histogram](/TImages/BiasHistogram_VerifyN.png)

The histograms show some variance from the population, but none of the datasets are significantly far from the population levels.  

Finally, as before, I performed Z-tests, using the averages of each of the 8 datasets, as well as the known values from the entire population.  Briefly, this test checks if the sample averages are significantly different from the known, true average, and gives a p-value as a result.  The higher the p-value, the less likely that there is a difference.  The statistical values are presented below:

| Dataset | Z-Value | P-Value |
|---------|---------|---------|
| V1 |  0.9513  | 0.3414 | 
| V2 |  0.1764  | 0.8599 | 
| V3 | -0.6759  | 0.4991 | 
| V4 |  0.2539  | 0.7996 | 
| V5 |  1.1838  | 0.2365 | 
| V6 |  0.7964  | 0.4258 | 
| **V7** | **-2.1482** | **0.0317** |
| V8 |  0.9513  | 0.3414 | 

Of the 8 datasets, one had a p-value smaller than the threshold of 0.05.

### Conclusions
Statistical calculations based on reasonable assumptions result in a recommendation to use $$N=250$$.  Looking at an ensemble of such datasets, a visual inspection agrees with the calculation.


**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_D).

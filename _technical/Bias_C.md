---
layout: post
title:  "Addressing Bias in Toroidal Generation: Data Set Size"
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
This article addresses a question concerning another possible bias in the procedures I use to generate initial Toroidal configurations.  The bias in question is ... The article concludes that ...

### Background
When I create Toroidal puzzles for the Toroidal game, or when I explore strategies to solve Toroidal puzzles, I have to generate an initial configuration for these puzzles using a computer program.  I have described how I do this in previous articles.  Basically, the program applies a number of random, legal moves to the goal configuration, resulting in random shuffling of rows and columns.  

In any empirical work involving repeated trials, there is a trade-off.  You want to use enough trials to be sure that your results speak reasonably precisely about the phenomenon you are studying, but not too many, since that leads to wasted time and effort.  

In particular, most of my 3x3 Toroidal work has been done using the same dataset of 100 Toroidal configurations.  I did not deeply consider whether 100 examples was enough to draw valid conclusions.  I used the same dataset for all of my explorations, so that I would be comparing different methods on the same data, and thus, even if the values I was measuring did not precisely describe the entire population of 3x3 Toroidals, I could at least get relative values.

The important topic of sample size is covered quite thoroughly in statistics.  However, we are dealing with a population that isn't described by any statistical model that I know of.  So I thought I address the question empirically.


I created 16 datasets, each with $$K = 500$$; each dataset has 5000 3x3 Toroidal problems.  These datasets were created using all combinations of the 2 previous articles: cycle checking and random perturbations of $$K$$, resulting in datasets labelled CCR, CCT, NCR, NCT.  There are 4 datasets for each combination.  I used the Look-Up Table method to solve each dataset, and plotted how the average solution length changed over each dataset.  In the plot below, I show the difference between the calculated average and the true value, 6.09889219577 which we can calculate from the true [distribution of optimal solutions](/technical/Distribution).

![Histogram](/TImages/BiasError_ALL.png)

The plot shows the relative error in the calculated average, and shows that initially, the error is significant.  But after about 2500 samples, every method results in calculated averages with error in the range +/- 0.5%.
 

### Conclusions
For 3x3 Toroidals, the calculated average solution length does not become more precise for sample sizes larger than about 2500.  At 2500 samples, the calculated average solution length is likely to be within +/- 0.5% error.  If a lower precision is acceptable, it's pretty clear that the average will fall within +/- 2% error with about 500 samples.  If we use the calculated average solution length as a proxy to indicate whether a sample is consistent with the entire population of 3x3 Toroidals, we can be fairly confident in any empirical results with about 500 hundred samples.  This is still quite  few samples, especially for slower methods, like IDS.

### Extra text from other articles
For 3x3 Toroidals, choosing $$K=500$$ (with or without random perturbation of $$K$$) is sufficient to generate random samples whose distribution matches the population.  To be specific, the distribution of shortest solution lengths matches.  I am unaware of any aspect of 3x3 Toroidal that might matter.  The value $$K=500$$ is two orders of magnitude larger than the maximum shortest solution length for 3x3, which is 8, and the average shortest solution length, which is roughly 6.099. 

Since I have only made a preliminary effort to solve 4x4 Toroidals, I don't really know whether the generator has some hidden bias in store for me to discover.  I do not have a good estimate for the maximum shortest solution length for 4x4 Toroidals, though from preliminary explorations, I expect it to be in the range 14-20.

**Looking forward.**
In the next article, 

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/Bias_B).

---
layout: post
title:  "Solving larger Toroidals: 4x4 and up"
date:   2026-08-17
categories:  Solver
usemathjax: true
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
</style>

 
### Abstract
In this article, I apply A\* search to a set of 25 randomized 4x4 Toroidals, using unscaled TMD, and the scaled TMD, rounding up.  I was able to solve all 25 configurations, using the unscaled TMD with A\* search, using 1.5 seconds, on average, and the average solution length was roughly 18.9.  This is certainly not optimal, but is plausibly close.
When paired with scaled TMD, rounding up, A\* was able to solve only 2 of the 25 problems within the time limit of 2 minutes.  The two solved problems required 48.8 seconds each, on average, and had average solution length of 12.5.

### Details
I consider [3x3 Toroidals "solved,"](3x3Done) in the sense that any one of several approaches can be used to find optimal solutions in a tiny fraction of the time that a human being would need.   

However, for larger Toroidals, only one of the techniques I've discussed previously can be applied to 4x4 Toroidals, or bigger.  The [state space for 4x4 Toroidals is so large](StateSpace) that IDS, LUT, DIST are simply infeasible.  We will not even try these approaches directly.  As we will see, A\* can be applied with moderate success, but still leaves a little to be desired.

Because we can't look at the entire state space of 4x4 Toroidals, we don't know some pretty basic things about it.  We know the number of distinct configurations, but we don't know the length of the maximum shortest solution ([MSSL](/technical/maximum)).  Currently, I suspect it might be in the range 14-20, but that's based on very limited information.  We don't know the average solution length (or the variance), and we don't know the distribution of solution lengths.  There may be ways to infer these quantities using mathematics and graph theory, but as of the date of this article, all I have are guesses.

To explore the performance of any solver algorithm to 4x4 Toroidals (or larger), my strategy will be to run preliminary experiments using a dataset of problems that is much smaller than we would need if we followed [the methodology for 3x3 Toroidals](/technical/Bias_D).  These preliminary experiments will reveal clues about the values we don't know yet, and will give us a very rough means to draw conclusions about how the solvers will work.

### Experimental set-up
I created a dataset of 4x4 Toroidals containing $$N=25$$ randomly scrambled configurations.  Each problem was generated using $$K=150$$ random moves, avoiding the [parity bias](/technical/Bias_A) by performing a slight perturbation in $$K$$, [chosen without cycle checking](/technical/Bias_B).  The value $$K=150$$ was chosen based on a few assumptions about the graph.  Since this is a preliminary experiment, these values seem plausible even if they're not values that guarantee unbiased scientific results.  

I applied A\* search paired with [TMD (unscaled)](TMD), and a [scaled version of TMD](ScaledEstimates).  The scaled version of TMD divides the unscaled TMD value by 4, since we are working with 4x4 Toroidals.  It is an empirical fact that TMD (scaled) never over-estimates the true distance in 3x3 Toroidals.  I am assuming that it also never over-estimates for 4x4 Toroidals.  I currently do not have a proof of this, but it seems plausible.

### Results
The application of A\* to the dataset is summarized in the table below:

|                            | Number Solved | Average Time | MinL |   AveL  |  MaxL  |
|:---------------------------|--------------:|-------------:|-----:|--------:|-------:|
| A\* TMD                               | 25/25 |    1.54      |  14  |   18.9  |   22   |
| A\* TMD Ceil                          |  2/25 |   48.8       |  12  |   12.5  |   13   |

The unscaled version of TMD allowed A\* to find solutions to all 25 problems, and the average time was 1.54 seconds.  That's plenty fast enough, but the solutions are not optimal.  Using scaled TMD, A\* solved only 2 of the problems within the time limit of 2 minutes.  The average time shown is the average of the two solved problems; the average over all 25 problems includes some runtimes that exceed 120 seconds, and that is probably due to delays in the solver as Python tries to manage the very large list of partial solutions.

### Conclusions
The TMD (unscaled) distance estimate solves 4x4 Toroidals relatively quickly, but does not provide optimal solutions.  The scaled TMD distance estimate found 2 pretty good solutions, but could not solve any others.  

This is a familiar story in the field.  Suboptimal solutions can be relatively easy to find.  Optimal solutions can be a lot harder to find.  It's not always true for every kind of problem, but it's true for Toroidal.  The extra time is needed to prove that no better solution exists; the "proof" can take various forms, depending on the solver and the problem, but for many kinds of problems, including Toroidal, this proof is a matter of exploring partial solutions until optimality is guaranteed.

In the case of Toroidal, I think that the scale of the 4x4 Toroidal state space overwhelms A\*'s ability to explore it.  The scaled TMD distance estimate does not provide enough guidance to keep A\* focussed on one of the optimal solutions.  The unscaled TMD estimate over-estimates the distances so much that many of the obviously bad choices are buried below less obviously bad choices.  Unfortunately, some optimal choices are buried, too, which means A\* paired with unscaled TMD will find a sub-optimal solution before an optimal one has come up.


**Looking forward.**
I'm not sure yet.  I think the pursuit of better distance estimates is unlikely to bear fruit.  There is a way to combine IDS and A\* which does a little better than A\* by itself.  There is a way to scale a distance estimate to improve the quality of solution while paying marginally more time.  I have a whole suite of sub-optimal solvers on the shelf waiting to be applied.  This is where the fun begins!  I just have to choose.

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/4x4Prelim.html).
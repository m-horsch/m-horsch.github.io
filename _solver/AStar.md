---
layout: post
title:  "Prioritizing Promising Sequences"
date:   2025-12-28
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
This article describes a very well-known search strategy called A\* search, which combines distance estimation with a thorough search.  The idea is to be thorough, but to prioritize those paths that seem most plausible.  In this context, we base the idea of plausibility partially on distance estimates.  In this article I will demonstrate that A\* search is able to solve randomly scrambled 3x3 Toroidal puzzles efficiently, requiring only milliseconds on average, when we use TMD and its variants as a distance estimate.  Using the unscaled TMD, the average time is 9 milliseconds, and the average solution length is 6.7; this is fairly close to optimal, but not quite.  Using the scaled TMD estimate, rounding up, the average time is 57 milliseconds, and the average solution length is 6.1; this is optimal for this set of Toroidals.


### Apology
Some readers may have been puzzled that I had not yet gotten around to describing, and applying, A\* search.  A\* search is quite an important technique, and it is applied in many practical and exploratory settings.  I began trying to solve Toroidal puzzles before I started writing about solving them.  And I did, in fact, apply A\* search almost immediately.   

So, if you have been waiting for me to get around to this topic, I apologize for the wait.  I didn't intend my articles to be a vehicle to teach classical techniques in AI.  I wanted to thoroughly explore the Toroidal problem domain, and I wanted a coherent story through the techniques that I applied.



### Combining distance estimates with a thorough exploration
We will apply a strategy called A\* search, which is a kind of planning algorithm.  It can be considered a measured combination of exploration and guidance.  This strategy will keep all options open, but will explore the state space preferring move sequences that look most promising.  Given enough time, and enough memory, A\* search will find a solution; given good guidance, A\* search will find the a solution relatively quickly.  Under certain conditions, A\* search will find the best solution.  


### How does A\* work, informally?
The A\* algorithm is a bit complicated, and I should take the time to explain it in detail for ordinary people.  I'll do that another time.  For now, I will give a very high-level explanation, so that we can move on to the interesting results.  My description leaves out a lot of technical details.  

Basically, A\* keeps track of all the sequences it has already created in a kind of sorted list.  Initially there is only one sequence: the one with zero moves.  The algorithm takes the most promising sequence out of the list, and checks to see if it is in fact a solution; if so, the search is over.  If not, the algorithm *extends* the sequence in hand  by duplicating it and adding all the possible single moves to each copy (we'll avoid extending a sequence in useless ways, as before).  So in the case of 3x3 Toroidals, one sequence comes out, and up to 12 slightly longer sequences go back in.  This process of choosing a sequence, and extending it continues until the solution is found.

Because we are always taking the most promising sequence out, less promising moves stay in the list; hopefully, the very least promising sequences never get extended at all.  If the prioritization measure is effective, the list remains relatively short, and very most promising sequences are extended and built-up, until a solution is found.  On the other hand, a less effective prioritization measure would make a lot of sequences seem roughly equally promising, causing the list of available sequences to grow larger.  If the prioritization measure is very poor, the list can become unmanageable.

The prioritization measure for A\* search must have 2 contributions.  The first is a distance estimate, like the ones we've explored in previous articles, that estimates the (unknown) distance yet to travel to the goal.  The second is a cost measure, which in our case is the length of the sequence so far.  In total, the prioritization measure estimates the total distance from start to the goal.

When A\* is paired with a distance estimate that never over-estimates, an optimal sequence will never be buried so deep in the list of sequences that a sub-optimal solution will be found first.  In other words, the solution A\* search finds is an optimal solution, if the distance estimate never over-estimates.  I should point out that there are lots of terrible estimates that never over-estimate.  For example, a distance estimate that always says "0" never over-estimates the true distance, but is quite useless for prioritization.
 

**Running the algorithm.**  I applied the A\* algorithm to the same 100 3x3 Toroidal problems that we have been using all along.  I used the unscaled TMD estimate, and the scaled TMD estimate, rounding up.  The following table lists the most successful strategies from previous articles, and two new data line, at the bottom.

|                              | Number Solved | Average Time | MinL |   AveL  |  MaxL  |
|:---------------------------|----------------:|-------------:|-----:|--------:|-------:|
| [Enhanced IDS](IDS_B)                  | 100 |   12.6s      |   4  | **6.1** |  **8** |
| [Look-up Table](LUT)                   | 100 |    0.0001s   |   4  | **6.1** |  **8** |
| [Distance Table](DIST)                 | 100 |    0.0005s   |   4  | **6.1** |  **8** |
| [Greedy MTC](ScaledEstimates_B)        | 100 |    0.031s    |   9  | 162.4   |  727   |
| [Greedy TMD](ScaledEstimates_B)        | 100 |    0.004s    |   6  |  44.1   |  199   |
| [Greedy ceil TMD](ScaledEstimates_B)   | 100 |    0.009s    |   5  |  80.9   |  310   |
| A\* TMD                                | 100 |    0.009s    |   4  |   6.8   |    9   |
| A\* ceil TMD                           | 100 |    0.057s    |   4  | **6.1** |  **8** |


The first of the new lines uses the A\* algorithm with the [TMD](EstimatingDistances) distance estimate.  It's able to solve all 100 problems, with an average time of about 9 milliseconds, which is pretty fast.  It also gives pretty good quality solutions, with an average length of 6.8 moves, which is just slightly bigger than optimal.  The second of the new lines uses the same TMD distance estimate, but [scales the estimate by division](ScaledEstimates) to ensure that the distance is never over-estimated.  It finds optimal solutions to all 100 problems, requiring about 60 milliseconds on average.  

In terms of memory used, using unscaled TMD, the A\* algorithm looked at 653 sequences on average.  When paired with the scaled TMD estimate, the A\* algorithm looked at 3613 sequences, on average.  These numbers are quite small compared to the number of unique sequences stored by the table-based solvers ([LUT](LUT) and [DIST](DIST)).

**Looking forward.**
These are pretty good results, for 3x3 Toroidals.  In the next article, I'll review the [results for 3x3 Toroidals](3x3Done), before setting sights on larger sizes.

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/AStar.html).


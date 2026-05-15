---
layout: post
title:  "Distribution of Toroidal Distances"
date:   2025-09-10
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
</style>

In [a previous article](/technical/maximum.html), I defined the *maximum shortest solution* as the longest optimal solution over a whole class of Toroidals.  For example, we know that every 3x3 Toroidal has a solution with 8 moves or fewer.  We also know that there are 181440 different ways that the tiles of a 3x3 Toroidal can be arranged (assuming that we are considering only Toroidals with no exchangeable tiles).

The table below reveals how many 3x3 Toroidal configurations have optimal solutions with length 1, 2, etc.  This will be presented in terms of an absolute count, as well as a percentage of the number of configurations.  In statistical language, this is called a distribution.


| Length | Count | Percentage |
|--:|--:|--:|
| 0	|     1	|  0.0006% |
| 1	|    12	|  0.0066% |
| 2	|    96	|  0.05% |
| 3	|   736	|  0.41% |
| 4	|  5208	|  2.9% |
| 5	| 28674	| 15.8% |
| 6	| 89497	| 49.3% |
| 7	| 54741	| 30.2% |
| 8	|  2475	|  1.4% |


Almost half of all configurations have an optimal solution of length 6.  Another 30% have an optimal solution length of 7 steps.

I also calculated the distribution for 2x2 Toroidals, presented below:

| Length | Count | Percentage |
|--:|--:|--:|
| 0	|  1	|  4.2%|
| 1	|  4	| 16.3%|
| 2	| 10	| 41.7%|
| 3	|  8	| 33.3%|
| 4	|  1	|  4.2%|

This information will come in handy for at least a couple of purposes.  First, there are ways to estimate the number of moves needed to solve a Toroidal puzzle, and knowing the exact answer will help us understand the quality of the estimate.  Second, it might be useful to know whether the script I use to generate puzzles is generating puzzles without any bias.  I had suspected that my script does produce puzzles that are biased towards longer solutions, but since the number of puzzles with short solutions is relatively small, it might be hard to detect.  

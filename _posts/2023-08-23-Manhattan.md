---
layout: post
title:  "Better prioritization"
date:   2023-08-23
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

Previously, I wrote about 
[using heuristics to prioritize promising sequences]({% post_url 2023-08-16-Toroidal-AStar %}). The approach was able to solve 3x3 Toroidals in a few tens of milliseconds.  The posting ended with a claim that the prioritization scheme was unable to solve 4x4 Toroidals given a full 60 seconds to try.  I later applied A\* MTC to 4x4 Toroidals, giving the algorithm 5 minutes to try each example.  It was able to solve a few of these, but only those whose solutions were at depth 14.  I halted this experiment before it tried all 100 examples, because 5 minutes per failure is 8 hours of failure.

There are two compounding factors that change when we proceed from 3x3 to 4x4.  First, the number of possible single moves increases from 12 in the 3x3 case, to 16 in the 4x4 case.  The Enhanced IDS can prevent a lot of useless sequences, so the number of possibly useful moves can be reduced from 16 to something like 12 (rough guess) in the 4x4 case.  Secondly, because there are more tiles to put into place, the maximum number of moves needed to solve a Toroidal increases from 8 (known fact for 3x3) to something like 20 (rough guess for 4x4).  

The result creates a formidable challenge.  If we're using Simple IDS, the number of sequences for 3x3 Toroidals is $$\small 12^{8}$$, or roughly 430 million, which is a number in the range of human experience; it's larger than the population of the USA, but smaller than the total population of Europe (2023 data).  However, the number of sequences for 4x4 Toroidals is beyond human experience: $$\small 16^{20}$$, or roughly 1.2 million million million million sequences.   Roughly, the population of 151 million million Earths.  Even if we use Enhanced IDS, preventing useless sequences, the number of sequences to explore is on the order of $$\small 12^{20}$$ which is about the population of 500 thousand million Earths.  IDS is totally infeasible for 4x4 Toroidals, and larger.

|              |    3x3 Sequences  | 3x3 Time | 4x4 Sequences      | 4x4 Estimated Time |
|:-------------|------------------:|---------:|-------------------:|--------:|
| Simple IDS   | $$\small 12^{8}$$ |  *322*   | $$\small 16^{20}$$ |    -    |
| Enhanced IDS | $$\small 8^{8}$$  |  12.6s   | $$\small 12^{20}$$ |    -    |
| A\*   MTC    | $$\small 8^{8}$$  |  0.02s   | $$\small 12^{20}$$ | *6.3h*  |

The table also gives average times from previous experiments 
([Simple IDS]({% post_url 2023-08-02-Toroidal-IDS_A %}), 
[Enhanced IDS]({% post_url 2023-08-09-Toroidal-IDS_B %}), 
[AStar MTC]({% post_url 2023-08-16-Toroidal-AStar %})).  The times presented in *italics* are estimates.  In the case of Simple IDS on 3x3 Toroidals, I calculated an estimate of the time needed to explore to depth 8, given the successful solutions to shorter depths.  The estimate of 6.3 hours for A\*   MTC on 4x4 Toroidals is a ball-park figure only, based on the limited success described in the opening paragraph above.

A simple change in the prioritization scheme can significantly boost A\* performance. In my previous posting, I used the MTC heuristic, which counted misplaced tiles. However, this approach has a weakness, namely that many configurations end up with the same priority, leading to the use of a "first come, first served" tie-breaking rule. This wasn't a big issue for 3x3 Toroidals due to the relatively limited number of configurations. However, for larger ones like 4x4 Toroidals, which have many more configurations with identical priorities using MTC, the problem becomes more pronounced.

It's more effective if we count the number of positions each misplaced tile has to move to be in the right place, ignoring all the other tiles.  We'll call this the *Total Manhattan Distance*, or TMD. 
The part about ignoring the other tiles is important, because it allows us to pretend that the tiles are all independent, making the calculations simpler.  Consider the configuration below.  

![A 3x3 Toroidal with one row moved.](/TImages/Stars3x3_CAB.png)

Only three tiles are out of place, so the misplaced tiles count is 3.  But you may recognize this as being one of the diagrams from [the posting on swing]({% post_url 2023-07-26-Toroidal-SwingingIt %}).  We can confidently say that the solved position is exactly 4 moves away.  

To calculate the TMD, we'll calculate the distance of the three tiles, A, B, C.  Remember that we're not using the normal Toroidal rules for this calculation.  We're pretending that each tile can move independently, without affecting any other tile.

* Tile A could slide into place with 1 move.
* Tile B could slide into place with 1 move.  
* Tile C could slide across, and down, so 2 moves.  

So, the TMD in this configuration is 4. By coincidence, this is exactly the number of moves required by a swing in this configuration to position the three tiles correctly.
Generally, TMD will not correspond directly to the exact number of moves required to solve a Toroidal.  Hopefully, though, it will be close, and a little more informative than counting misplaced tiles.

The TMD measure has a much wider range than MTC.  On a 3x3 Toroidal, all the tiles can be out of place by at Manhattan distance of at most 2, giving a maximum TMD value of 18.  In contrast, the  MTC has a maximum value of 9.  In practice, TMD makes highly scrambled Toroidals look worse than they actually are, but getting a few tiles into place really decreases the TMD value, so moves that put tiles into place look much more promising.  

**Running the algorithm.**  
I ran the A\* algorithm with the TMD heuristic on the same set of 3x3 Toroidals that we've been using in the previous postings.
I also applied A\*  TMD on a set of 4x4 Toroidals.  These have 16 unique tiles, and each Toroidal was scrambled using 500 random moves, so they are similar in construction to the 3x3 Toroidals we've been using.  The results are summarized in the following table:

|              |      3x3  |    4x4 |
|:-------------|----------:|-------:|
| Simple IDS   |   *322*   |    -   |
| Enhanced IDS |   12.6s   |    -   |
| A\*  MTC     |    0.02s  | *6.3h* |
| A\*  TMD     |    0.006s |  2.4s  |

Each row in the table is a different algorithm, applied to the two sets of Toroidals.  The new data is on the final row.  The data values are average times, in seconds. A dash is used when the solvers could not solve any problems in 120 seconds.  The times in *italics* are estimates based on the solvers' performance when solutions were found.

We can see that the TMD heuristic improves the 3x3 times, and shows that 4x4 Toroidals are solvable in about 2.4 seconds on average.  Getting 4x4 Toroidals down to a couple of seconds is really pretty good, I think, and getting 3x3 Toroidals down to a few milliseconds is also quite impressive, given where we started.  

We have to keep in mind that the solutions found by IDS are always the shortest possible solution, but when we apply A\*, the heuristics I've proposed are not guaranteed to lead to the shortest solution.  In the case of 3x3, A\* is able to find the shortest solution quite often, but not always.  I have every reason to expect that A\* is finding similarly high-quality solutions for 4x4 Toroidals, but since IDS is infeasible, it's hard to do more than guess here. 

**Keep reading.**  The TMD uses *Manhattan distance,* which measures distances in terms of the number of rows and columns in a Toroidal: if two tiles are separated by 2 rows and 3 columns, the Manhattan distance is 2+3=5.  It's named after the kinds of paths one has to take when travelling on roads that are laid out in a grid, like in a big city like Manhattan.  The *Chebyshev distance* is related to the Manhattan distance, but instead of adding the row distance and the column distance together, we take only the larger of the two values.  For example, if two tiles are separated by 2 rows and 3 columns, the Chebyshev distance is 3.

We can use this distance in a heuristic that calculates the total Chebysev distance (TCD) over all tiles that are out of place.  I applied this heuristic to the 3x3 and 4x4 Toroidals, with the results summarized below:


|              |      3x3  |    4x4 |
|:-------------|----------:|-------:|
| Simple IDS   |     -     |    -   |
| Enhanced IDS |   12.6s   |    -   |
| A\*  MTC     |    0.02s  |    -   |
| A\*  TMD     |    0.006s |   2.4s |
| A\*  TCD     |    0.02s  |  22.2s |

The TCD heuristic requires more time than TMD for 3x3 and 4x4 Toroidals, but is still feasible for these problems.  The advantage to TCD is that it finds shorter solutions, on average.  In the table below, I give the average length of solution found by the 5 different algorithms:

|              |    3x3  |   4x4 |
|:-------------|--------:|------:|
| Simple IDS   |    -    |    -  |
| Enhanced IDS |   6.1   |    -  |
| A\*  MTC     |   6.3   |   -   |
| A\*  TMD     |   6.8   |  19.3 |
| A\*  TCD     |   6.3   |  16.4 |

From these two tables, we can see that TCD uses roughly the same amount of time as MTC on 3x3, and produces solutions of roughly the same length.  But TCD is much more effective on 4x4 Toroidals than MTC.

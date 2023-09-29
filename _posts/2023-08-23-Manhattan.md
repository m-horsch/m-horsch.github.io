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

**First we take Manhattan.** 
A change in the prioritization scheme can significantly boost A\* performance. In my previous posting, I used the MTC heuristic, which counted misplaced tiles. 
It's more effective if try to estimate how far tiles have to move to be in the right place, ignoring all the other tiles.
We'll measure distance using two ways to measure distance.   

* First, *Manhattan distance,* which measures distances in terms of the number of rows and columns in a Toroidal: if two tiles are separated by 2 rows and 3 columns, the Manhattan distance is 2+3=5.  It's named after the kinds of paths one has to take when travelling on roads that are laid out in a grid, like in a big city like Manhattan.

* Second, *Chebyshev distance,* which is related to Manhattan distance, but instead of adding the row distance and the column distance together, it takes only the larger of the two values.  For example, if two tiles are separated by 2 rows and 3 columns, the Chebyshev distance is 3. 

The part about ignoring the other tiles is important, because it allows us to pretend that the tiles all move independently, making the calculations simpler.  It also means that the distances we calculate are not the actual distances tiles will move in a solution; it's an estimate, only.  The purpose is to use an easily calculated distance to estimate the number of moves needed to solve the Toroidal from the current configuration. 

Consider the 3x3 Toroidal below. 

![A 3x3 Toroidal 4 moves away from solved.](/TImages/Stars3x3_CAB.png)

You may recognize this as being one of the diagrams from [the posting on swing]({% post_url 2023-07-26-Toroidal-SwingingIt %}).  We can confidently say that the solved position is exactly 4 moves away.  

To demonstrate, we'll calculate the Manhattan distances of the three tiles, A, B, C.  Remember that we're not using the normal Toroidal rules for this calculation.  We're pretending that each tile can move independently, without affecting any other tile.

* Tile A could slide into place with 1 move.
* Tile B could slide into place with 1 move.  
* Tile C could slide across, and down, so 2 moves.  

The following table shows Manhattan and Chebyshev distances for the three misplaced tiles:

| Tile  | Manhattan | Chebyshev |
|------:|:---------:|:---------:|
|   A   |      1    |     1     |
|   B   |      1    |     1     |
|   C   |      2    |     1     |
| Total |      4    |     3     |


For our new prioritization scheme, we'll look at all tiles that are out of place, and sum the distances.  The Total Manhattan Distance (TMD) for the configuration above is 4, and the Total Chebyshev Distance (TCD) is 3.  

Coincidentally, the TMD in this example is exactly the number of moves required by a swing in this configuration to position the three tiles correctly.
Generally, an estimate like TMD or TCD will not correspond directly to the exact number of moves required to solve a Toroidal.  Hopefully, though, it will be close, and a little more informative than counting misplaced tiles.

**Running the algorithm.**
I ran the A\* algorithm with the two new heuristics on the same set of 3x3 Toroidals that we've been using in the previous postings.
I also applied both of them on a set of 4x4 Toroidals.  These have 16 unique tiles, and each Toroidal was scrambled using 500 random moves, so they are similar in construction to the 3x3 Toroidals we've been using.  The results are summarized in the following table:

|              | 3x3 Time  | 3x3 Length | 4x4 Time | 4x4 Length |
|:-------------|----------:|-----------:|---------:|-----------:|
| Simple IDS   |   *322s*  |     -      |     -    |     -      |
| Enhanced IDS |   12.6s   |    6.1     |     -    |     -      | 
| A\*  MTC     |    0.02s  |    6.3     |  *6.3h*  |     -      | 
| A\*  TMD     |    0.006s |    6.8     |   2.4s   |    19.3    |
| A\*  TCD     |    0.02s  |    6.3     |  22.2s   |    16.4    |


Each row in the table is a different algorithm, applied to the two sets of Toroidals.  The first three rows are summaries from previous postings.
The columns **3x3 Time** and **4x4 Time** report the average time required to solve the Toroidals, with *italicized* values being estimates based on partial information.  In particular, I estimated that A\*  MTC would require about 6 hours to solve a 4x4 Toroidal, based on the limited success I mentioned in the opening paragraph above.
There are two columns labelled  **3x3 Length** and **4x4 Length**, which report the average length of the solutions found by the algorithms.  We know that IDS always finds the shortest possible solutions.  

We can see that the TMD heuristic improves the average time for 3x3 Toroidals, and shows that 4x4 Toroidals are solvable in about 2.4 seconds on average.  Using A\*  TCD is about as fast as A\*  MTC for 3x3 Toroidals, but is able to solve 4x4 Toroidals as well, in a plausible amount of time.  

When we look at the solution lengths for 3x3 Toroidals, we can see that all the algorithms based on A\* find solutions that are slightly longer than the shortest possible, found by IDS.  I reviewed the solutions found by these algorithms, and all three A\* based algorithms find the shortest solution some of the time, and when they don't, they find a solution that's usually one move longer.   While A\*  TMD is the fastest, it is less likely to find the shortest possible.  A\*  TCD is comparable to A\*  MTC in terms of time and solution length for 3x3 Toroidals, but is able to solve 4x4 Toroidals, whereas A\*  MTC requires much more time for 4x4.

**Summary.** We can find pretty good solutions to 3x3 Toroidals in just a few tens of milliseconds.  And we've got a method to solve 4x4 toroidals reasonably quickly.  We should mention that we don't know the shortest possible solutions to the 4x4 Toroidals used in this experiment.  I think the TCD average of 16.4 is pretty plausible, and probably pretty close to the average of shortest solutions.  On the other hand, it costs an extra 20 seconds on average to find a good solution.  If you're in a hurry, a TMD finds longer solutions very much quicker.

As I always do, I will end with good news and bad news.  The good news is that we can find solutions to 3x3 and 4x4 Toroidals!  The bad news is that none of these methods can solve 5x5 Toroidals.  If we are to reach past 3x3 we have to dig a little deeper into what makes Toroidal puzzles tricky to solve.  
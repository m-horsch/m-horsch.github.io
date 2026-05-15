---
layout: post
title:  "Estimating distances: Misplaced Tile Count"
date:   2025-11-27
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

### Abstract
This article describes a very simple calculation called Misplaced Tile Count (MTC), which we can use to estimate the distance between any Toroidal state and the goal state.  The main reason to try this estimate is that it is simple.  The confusion matrix shows that the MTC estimates are not very accurate, and this is also indicated by a relatively large RMSE.  When used to guide a greedy search, MTC causes the search to get stuck in infinite cycles.  However, if cycles are prevented during search, MTC can find solutions relatively quickly, but the quality of solution is substantially below optimal.

### Details
In the previous article, I considered the idea of [estimating distance information](/solver/EstimatingDistances.html) by devising some formula that can be calculated quickly, to help solve Toroidals of any size.  

In my study of Toroidal, I have explored a small number of distance estimates.  In this article, I will use the very simplest one I could think of.  It's really not a very good estimate, but that's okay.  We'll look at it carefully, and that will help us when we look at more sophisticated estimates.

![Misplaced tile count example](/TImages/Stars3x3_Misplaced.png)
**Misplaced Tile Count (MTC).** This distance estimate counts the number of misplaced tiles.  The simple intuition is that the more misplaced tiles, the greater the distance to the goal state.  In the image on the left, I have labelled the 9 tiles with a 0 or a 1, depending on whether the tile is misplaced.  There are 5 misplaced tiles in total.

In general, a 3x3 Toroidal grid can have up to 9 misplaced tiles, although it is impossible for there to be only 1 misplaced tile.  And, as I said in a previous article, 3x3 Toroidal puzzles that I create will never have exactly 2 tiles out of place (see the [third tutorial on solving Toroidal puzzles](/tutorial/1Swap.html)).

**Data collection**
I wrote a Python program to calculate the MTC for 3x3 Toroidal puzzles.  This program makes use of the [distance table look-up](/solver/DIST.html)) from a previous article. This file contains all 181440 Toroidal states of the 3x3 puzzle, along with the true distance from the given state to the goal state.  Using the states stored in this file. The Python program applies MTC to each of the 181440 states, and all the estimated values are stored and counted.  The program outputs tables of data which are described next.


**Confusion Matrix: MTC for 3x3 Toroidal**
The table below gives the confusion matrix for MTC applied to every possible 3x3 Toroidal configuration.  Most of the data is located in the bottom right corner of the table, with quite a lot of data away from the diagonal of exact distances.  This suggests to me that when a Toroidal state is relatively far from the goal state, the MTC really has no idea how far away it is.  It also shows that the MTC is likely to over-estimate the true distance quite often.  From this data, the RMSE value is roughly 2.2.

|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|
|       |   *0* |   *1* |   *2* |   *3* |   *4* |   *5* |   *6* |   *7* |   *8* |   *9* |
| **0** |     1 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **1** |     0 |     0 |     0 |    12 |     0 |     0 |     0 |     0 |     0 |     0 |
| **2** |     0 |     0 |     0 |     0 |     0 |    72 |    24 |     0 |     0 |     0 |
| **3** |     0 |     0 |     0 |    72 |     0 |    72 |     0 |   576 |     0 |    16 |
| **4** |     0 |     0 |     0 |    72 |    72 |   792 |   288 |  1368 |  1944 |   672 |
| **5** |     0 |     0 |     0 |    12 |    54 |   792 |  3456 |  6264 |  9648 |  8448 |
| **6** |     0 |     0 |     0 |     0 |   189 |  1224 |  4956 | 15876 | 34920 | 32332 |
| **7** |     0 |     0 |     0 |     0 |    63 |    72 |  2052 |  9252 | 19638 | 23664 |
| **8** |     0 |     0 |     0 |     0 |     0 |     0 |   144 |   144 |   567 |  1620 |

Another feature to notice is that the columns labelled *1* and *2* have no counts in them.  What that means is that there are no 3x3 Toroidal states that have MTC equal to 1 or 2.  This is a property of Toroidal puzzles: it's impossible to have only one tile out of place, and by design, 3x3 Toroidal puzzles will never have exactly 2 tiles out of place (see the [final tutorial on solving Toroidal puzzles](/tutorial/1Swap.html)).  Also, if the true distance is **1**, MTC comes up with an estimate of *3* a total of 12 times; that is, every time the true distance should be **1**, MTC predicts *3*.  


**The effectiveness of MTC as a guide for search**
I used the MTC estimate as a replacement for the true distance, using a greedy search. 
In other words, at every node, the greedy search calculated MTC for every possible next state, and selected the state that had the lowest MTC value.  All other states are discarded, and the search continues from the selected state. It's important to remember that this greedy search strategy is based on the [distance-table method](/solver/DIST.html), and as a result, there is no protection against trying useless sequences, which I mentioned in a [previous article](/solver/IDS_B.html).

I applied this search strategy to the same 100 3x3 problems as before.
The program was not able to solve any of these 100 problems; search times out before finding a solution.  I suspected that MTC leads the greedy search through a cycle of states (e.g., from A to B to C and then back to A), and once started on this cycle, greedy search will just keep cycling.  This cannot happen with exact distances, of course, but it is not surprising that MTC causes greedy search to cycle. The MTC estimate does not provide sufficient guidance to be the only source of information to solve Toroidal puzzles.  

I modified the greedy search to prevent cycles.  More precisely, I modified the search strategy so that if the distance estimate suggests moving to a state it had visited once before, the suggestion is discarded, and the next best state is tried.  With this modification, greedy search with MTC is able to solve all 100 problems.  The table below summarizes the timing data.

|                | 3x3 Number Solved | 3x3 Average Time   |
|:---------------|------------------:|-------------------:|
| [Simple IDS](/solver/IDS_A.html)    |  62 |    *322s*   |
| [Enhanced IDS](/solver/IDS_B.html)  | 100 |   12.6s     |
| [Look-up Table](/solver/LUT.html)   | 100 |    0.0001s  |
| [Distance Table](/solver/DIST.html) | 100 |    0.0005s  |
| MTC (no cycles)                     | 100 |    0.089s   |

As shown in the table, the average time to find a solution using MTC (no cycles) is less than one-tenth of a second.  That's not at all a bad result.

Unfortunately, the solution quality is rather poor, which is not shown in the table.  Whereas all the other methods in the table obtain an optimal solution for each solved Toroidal, the solutions obtained by MTC (no cycles) average 256 moves; the best solution was 16 moves, and the longest was 942 moves.  In contrast, on average, the optimal solution length for 3x3 Toroidals is 6.1, with 8 being the maximum.  


**Looking forward.**
In the next article, I'll describe a different  distance estimate, called [Total Manhattan Distance](/solver/TMD).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/MTC.html).

---
layout: post
title:  "Estimating distances: Total Manhattan Distance"
date:   2025-11-28
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
This article describes a calculation called Total Manhattan Distance (TMD), which we can use to estimate the distance between any Toroidal state and the goal state.  The confusion matrix shows that the TMD estimates are not very accurate, and over-estimates the true distance by a lot.  The RMSE for TMD is substantially larger than for MTC.  When used to guide a greedy search, TMD causes the search to get stuck in infinite cycles in all but 2 of the 100 example Toroidals given.  However, if cycles are prevented during search, TMD works about as well as MTC.  TMD can find solutions with about the same average time as MTC, with slightly shorter solution length.  The quality of solution found my TMD is still substantially below optimal.

### Details
In the previous article, I considered a very simple calculation called Misplaced Tile Count (MTC) to estimate the true distance between any given state and the goal state.
It was simple, but the estimate was not very accurate.  But now that we've seen a simple calculation, we can move on towards more interesting ways to estimate these distances.  
The MTC only counts which tiles need to move, but doesn't consider how far they have to go.  In this article, I'll describe the Total Manhattan Distance (TMD), which attempts to account for distances.

![Total Manhattan Distance example](/TImages/Stars3x3_Manhattan.png)
**Total Manhattan Distance.**  (TMD)  This estimate calculates how far away each misplaced tile is from its goal position, assuming that we only move along the rows and columns. 
For example, if two grid locations are separated by 2 rows and 3 columns, the Manhattan distance is 5.
To get the Total Manhattan Distance, we calculate the Manhattan distance for each out-of-place tile.  Consider the grid shown on the right.  There are 4 tiles in their correct position, so they count as zero distance away.  There are 3 tiles that are exactly 1 move away from their correct position.  And there are 2 tiles that have to move 2 positions.  The Manhattan distance is a bit tricky because the edge of the grid is not a hard boundary.  For example, a tile on the bottom row is only one move away from the top row. 


**Confusion Matrix: TMD for 3x3 Toroidal**
The table below gives the confusion matrix for TMD applied to every possible 3x3 Toroidal configuration (using the true distance data from [distance table look-up](/solver/DIST.html) as in the previous article).  As shown below, TMD has a range of estimated distances, going from *0* to *18*.  This range is quite a bit larger than the range of true distances, which for 3x3 Toroidals, is **0** to **8**.   So TMD over-estimates a lot.  The RMSE for TMD is about 6.2, which is substantially higher than MTC.

|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|
|       |   *0* |   *1* |   *2* |   *3* |   *4* |   *5* |   *6* |   *7* |   *8* |   *9* |        *10* | *11* |  *12* |  *13* |  *14* |  *15* |  *16* |  *17* |  *18* |
| **0** |     1 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **1** |     0 |     0 |     0 |    12 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **2** |     0 |     0 |     0 |     0 |     0 |     0 |    96 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **3** |     0 |     0 |     0 |     0 |     0 |    72 |     0 |   144 |    72 |   448 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **4** |     0 |     0 |     0 |     0 |    72 |     0 |   288 |   288 |   864 |   432 |  1512 |   864 |   888 |     0 |     0 |     0 |     0 |     0 |     0 |
| **5** |     0 |     0 |     0 |     0 |    36 |    72 |    12 |  1296 |  1170 |  3696 |  5112 |  5040 |  6072 |  4608 |  1080 |   480 |     0 |     0 |     0 |
| **6** |     0 |     0 |     0 |     0 |    45 |     0 |   216 |   900 |  2628 |  4320 | 12024 | 17136 | 15672 | 18072 | 11232 |  4704 |  2160 |   288 |   100 |
| **7** |     0 |     0 |     0 |     0 |    18 |     0 |    72 |   324 |  1395 |  1728 |  2844 |  7668 |  8784 |  9144 | 11628 |  6048 |  4284 |   648 |   156 |
| **8** |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |    54 |     0 |   108 |    72 |    81 |     0 |  1080 |   288 |   432 |   360 |     0 |




**The effectiveness of TMD as a guide for search**
As in the previous article, I used the TMD estimate as a replacement for the true distance, using a greedy search. 
In other words, at every node, the greedy search calculated TMD for every possible next state, and selected the state that had the lowest TMD value.  All other states are discarded, and the search continues from the selected state.  I applied this search strategy to the same 100 3x3 problems as before.
The simple greedy search strategy was only able to solve 2 of these 100 problems; the remaining 98 were terminated because they began cycling through states without making any progress.  This is better, by 2 problems, than MTC from before, but still, not very useful.

Using the version of the program that prevents the strategy from repeating a cycle, greedy search using the TMD estimate was able to solve all 100 problems.  The data is below.  The table is similar to the ones we've seen before, but I've added three columns.  It shows data about solution quality, in terms of the minimum solution length (MinL), the average solution length (AveL), and the maximum solution length (MaxL).  The numbers in **bold** are optimal values; numbers in *italics* are estimated values based on partial data.

|                          | Number Solved | Average Time | MinL |  AveL | MaxL |
|:-------------------------|--------------:|-------------:|-----:|------:|-----:|
| [Simple IDS](/solver/IDS_A.html)     |  62 | *322s*     |   4  |   5.4   |   6   |
| [Enhanced IDS](/solver/IDS_B.html)   | 100 |   12.6s    |   4  | **6.1** | **8** |
| [Look-up Table](/solver/LUT.html)    | 100 |    0.0001s |   4  | **6.1** | **8** |
| [Distance Table](/solver/DIST.html)  | 100 |    0.0005s |   4  | **6.1** | **8** |
| [MTC (no cycles)](/solver/MTC.html)  | 100 |    0.089s  |  16  | 256.0   | 942   |
| TMD (no cycles)                      | 100 |    0.082s  |   6  | 246.2   | 738   |

The TMD results are comparable to MTC: the average time to find a solution using TMD (no cycles) is less than one-tenth of a second.   It appears that TMD is slightly better than MTC in terms of solution length, using about the same average time.


**Looking forward.**
The distance estimates MTC and TMD are fairly easy to calculate, but can't really guide our current search strategy towards high quality solutions quickly.  Before I take a somewhat drastic step into exploring a different search strategy, I will make some changes to the way these two distance estimates are used.  The key idea is that each move that we make on a 3x3 Toroidal puzzle moves 3 tiles.  In other words, distances might be over-estimated by as much as a factor of three.  Better solutions might be found if estimates like TMD are divided by 3 (for 3x3 Toroidals, specifically).

In the next article, I'll explore [distance estimate scaling](/solver/ScaledEstimates)  and its implications.  

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/TMD.html).

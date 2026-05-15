---
layout: post
title:  "Scaling distance estimates: Total Manhattan Distance"
date:   2025-12-21
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
This article describes a modification of the Total Manhattan Distance (TMD) estimate,
based on the obvious fact that each move on a 3x3 Toroidal displaces 3 tiles.  To account for this, we explore 3 different ways to scale the TMD by dividing by 3.  First, by using simple division.  Second by rounding values down to the nearest whole number.  Finally, by rounding up.  The quality of these three variants is explored, using confusion matrices, RMSE values, and their effectiveness guiding greedy search (where cycles are prevented).  All three estimates have improved RMSE, and the confusion matrices show no over-estimation at all.  The two versions that round the scaled value to a whole number demonstrate improved search results, with rounding up being slightly better.

### Details
In the [previous article](/solver/TMD), I described the Total Manhattan Distance (TMD), which I used to estimate the true distance between any given state and the goal state. 
The range of TMD was quite broad, from 0-18, compared to the range of true distances, which is 0-8. 
And as I noted in the previous article, each move displaces 3 tiles. So the TMD could result in estimates that are 3 times too high. 
This alone suggests that we might make some progress by taking the TMD estimate, and dividing by 3. This is a form of scaling.

**Scaling Distances.**  
There are three ways we could account for the the fact that each move displaces several tiles.  
1. **Simple Division**: Divide the distance estimate by 3, with no rounding.
2. **Rounding Down**: Divide the distance estimate by 3, but if the result has a fractional part, round down to the nearest whole number.
3. **Rounding Up**: Divide the distance estimate by 3, but if the result has a fractional part, round up to the nearest whole number.  

The following table gives examples of each:

|:---:|:---|:---:|:---:|
| Distance estimate |  Simple Division |  Rounding Down | Rounding Up |
| 0 |     0 | 0 | 0 |
| 1 | 0.333 | 0 | 1 |
| 2 | 0.667 | 0 | 1 |
| 3 | 1.000 | 1 | 1 |
| 4 | 1.333 | 1 | 2 |
| 5 | 1.667 | 1 | 2 |
| 6 | 2.000 | 2 | 2 |
| 7 | 2.333 | 2 | 3 |

**Confusion Matrix: Scaling TMD using Simple Division**
I won't present the confusion matrix for scaling using simple division. The 
table is almost an identical copy of the confusion matrix from the [previous article](/solver/TMD.html), except the values at the top of each column is divided by 3.
So the range of estimated values is from *0* to *6*.  The RMSE for this data is 2.6, which is smaller than the RMSE for the unscaled TMD by about a factor of 3 as well.


**Confusion Matrix: Scaling TMD using Rounding Down**
The table below gives the confusion matrix for TMD with truncated scaling.  As shown below, TMD has a range of estimated distances, going from *0* to *6*.  This range is a bit smaller than the range of true distances, which for 3x3 Toroidals, is **0** to **8**.   The RMSE for TMD with truncated scaling is about 2.6, which is about one third the RMSE of unscaled TMD. 

| ----: | --: | ------: | ------: | ------: | ------: | -----: | ----: |
|       | *0* |    *1*  |    *2*  |    *3*  |    *4*  |    *5* |   *6* |
| **0** |  1  |     0   |      0  |      0  |      0  |      0 |     0 |
| **1** |  0  |    12   |      0  |      0  |      0  |      0 |     0 |
| **2** |  0  |     0   |     96  |      0  |      0  |      0 |     0 |
| **3** |  0  |    72   |    216  |    448  |      0  |      0 |     0 |
| **4** |  0  |    72   |   1440  |   2808  |    888  |      0 |     0 |
| **5** |  0  |   108   |   2478  |  13848  |  11760  |    480 |     0 |
| **6** |  0  |    45   |   3744  |  33480  |  44976  |   7152 |   100 |
| **7** |  0  |    18   |   1791  |  12240  |  29556  |  10980 |   156 |
| **8** |  0  |     0   |     54  |    180  |   1161  |   1080 |     0 |

One important observation about this table is that when the division is rounded down, the result never over-estimates the true distance.  Look at all those zeroes above the diagonal.  Also, there is exactly one state where the estimated distance is zero, and that's the goal state.  It's a little strange that for states where the true distance is **8**, the estimated distances range from *2* to *5*, but no higher.  

It's worth taking just a bit more time examining this table.  Consider the row where the true distance is **1**.  There is only one value on this row that is not a zero.  This means that when the true distance is **1**, the estimate says *1*, and never gets it wrong.  The same is true for the row where there true distance is **2**: the estimate always returns *2*.  However, look at the row for **3**.  When the true distance is **3**, the estimate could report any value from *1* to *3*.  For rows **5** and **6**, the estimate could range from *1* to *6*.  

**Confusion Matrix: Scaling TMD using Rounding Up**
The table below gives the confusion matrix for TMD when division is rounded up.  As shown below, TMD has a range of estimated distances, going from *0* to *6*.  This range is a bit smaller than the range of true distances, which for 3x3 Toroidals, is **0** to **8**.   The RMSE for TMD using Rounding Up is about 2.0, which is smaller than for TMD with truncated scaling, and simple scaling.

|------:|------:|------:|------:|------:|------:|------:|------:|
|       |   *0* |   *1* |   *2* |   *3* |   *4* |   *5* |   *6* |
| **0** |     1 |     0 |     0 |     0 |     0 |     0 |     0 |
| **1** |     0 |    12 |     0 |     0 |     0 |     0 |     0 |
| **2** |     0 |     0 |    96 |     0 |     0 |     0 |     0 |
| **3** |     0 |     0 |    72 |   664 |     0 |     0 |     0 |
| **4** |     0 |     0 |   360 |  1584 |  3264 |     0 |     0 |
| **5** |     0 |     0 |   120 |  6162 | 16224 |  6168 |     0 |
| **6** |     0 |     0 |   261 |  7848 | 44832 | 34008 |  2548 |
| **7** |     0 |     0 |    90 |  3447 | 19296 | 26820 |  5088 |
| **8** |     0 |     0 |     0 |    54 |   261 |  1368 |   792 |

We can observe from this table that rounding up also results in estimates that never over-estimate the true distance.  Note that when the estimate results in a value of *0* or *1*, the estimate is never wrong.  This is slightly better than what we get when we round down (see above).  For states where the true distance is **8**, the estimated distances range from *3* to *6*, which is a little higher than what we get when we round down.

Looking at this table a little more closely, we can see that each row is slightly more precise than for rounding down (above).  On the rows where the exact value is **0**, **1**, or **2**, the estimate is exactly right, and there are no states where the estimate gets it wrong.  For rows **3** and **4**, the range of estimates is fairly narrow, and the estimate that rounds up gets most of these states right.

**The effectiveness of scaling TMD as a guide for search**
As in the previous article, I used the three scaling variations for TMD to estimate the true distance, using a greedy search.  This time, I only applied the version of greedy search that prevents cycles.  The results are included in the table below, which also 
shows the results from previous experiments, for context.

|                          | Number Solved | Average Time | MinL |   AveL  |  MaxL |
|:---------------------------|--------------:|-----------:|-----:|--------:|------:|
| [Simple IDS](/solver/IDS_A.html)     |  62 | *322s*     |   4  |   5.4   |   6   |
| [Enhanced IDS](/solver/IDS_B.html)   | 100 |   12.6s    |   4  | **6.1** | **8** |
| [Look-up Table](/solver/LUT.html)    | 100 |    0.0001s |   4  | **6.1** | **8** |
| [Distance Table](/solver/DIST.html)  | 100 |    0.0005s |   4  | **6.1** | **8** |
| [MTC (no cycles)](/solver/MTC.html)  | 100 |    0.089s  |  16  | 256.0   | 942   |
| [TMD (no cycles)](/solver/TMD.html)  | 100 |    0.082s  |   6  | 246.2   | 738   |
| TMD Simple Division (no cycles)      | 100 |    0.084s  |   6  | 246.2   | 738   |
| TMD Rounding Down (no cycles)        | 100 |    0.014s  |  10  |  97.5   | 293   |
| TMD Rounding Up (no cycles)          | 100 |    0.016s  |   5  |  92.2   | 358   |

We can observe that the results from TMD with Simple Division are identical to the original version of TMD (with a slight variation in the average time, due to random effects).  This is no surprise, since simple division doesn't affect relative orderings.
However, when division is Rounded Down to the closest whole number, the results are in general, much better, compared to the original TMD results.  The average time is reduced, the average solution length is much smaller.  Even the maximum solution length is smaller.
Likewise, when we use division and rounding up, the results are similar.  

This data suggests that rounding up seems slightly better than rounding down, and the confusion matrix seems to support this finding.  I'm not sure data is conclusive.  

What intrigues me most about this experiment is that greedy search can find solutions very quickly, provided that cycles are prevented.  

**Looking forward.**
We might be able to make some progress trying to come up with more accurate distance heuristics.  I might indulge in that exploration in the future.  Because we started with
a search strategy that uses exact distances, cycles are not the only problem.  The greedy strategy used here does nothing to avoid useless sequences, which I used in previous strategies.  

In the [next article](/solver/ScaledEstimates_B.html), I'll apply the technique to avoid useless sequences, and we'll see improved results!

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/ScaledEstimates.html).

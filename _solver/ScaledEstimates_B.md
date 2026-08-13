---
layout: post
title:  "Preventing useless sequences for greedy search"
date:   2025-12-26
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

Revised: 2026-08-12

### Abstract
This article describes a modification of the greedy search strategy used in the previous articles, so that the algorithm chooses between possible moves that explicitly avoid useless sequences ([as described here](/solver/IDS_B.html)).  We show that this technique reduces sequence length, and also solution time, for all the distance estimating functions we have considered (MTC, TMD, scaled TMD).  The data show that this variation of greedy search quickly finds solutions that are still too far from optimal to be considered competitive with the optimal solution quality.


### Details
In previous articles, I described how we can use distance estimating functions to guide greedy search towards a solution.  Specifically, to get any results at all, I had to modify the original greedy strategy to prevent cycles.  It was surprising to me that this simple change enabled greedy search to solve 3x3 Toroidals as quickly as it did.  

I examined a few of the greedy solutions, and I found that there were lots of very useless sequences of moves.  These useless sequences did not create any cycles, of course, because I had already modified the algorithm to avoid that.  However, many long sequences of moves could be reduced to much more concise number of steps.  For example, here's an example sequence I pulled from one of the solutions:

```
L1 D0 D0 D1 D0 D0 D1 D0 D0 R2
```

This sequence has an effect on a 3x3 Toroidal, and the sequence does not create a cycle of states.  However, the long sequence of `D` moves can be reduced to a single move:

```
L1 U1 R2
```

To understand the simplification, we need to remember that moves `D0` and `D1` have no effect on each other.  Since `D0` is repeated 6 times, it returns the first column to the same state as it was right after the initial `L1`.  In other words, the result of 6 `D0` is to have no effect at all.  There are 2 `D1` moves, which is equivalent to `U1`.

The software I wrote to prevent useless sequences for IDS would have prevented this rather long sequence.  So I added it to the cycle-preventing greedy search strategy, and tried all the distance estimates I've used so far.  

There is a lot of data to look at, so I've broken it down into tables of related methods.  

### MTC


|                            | Number Solved | Average Time | MinL |   AveL  |  MaxL |
|:---------------------------|------------:|-------------:|-----:|--------:|------:|
| [MTC (no cycles)](MTC)     | 250  |    0.046    |   2  | 202.0   | 915   |
|  MTC (no cycles, prevent)  | 250 |     0.022    |   2  | 171.2   | 713  |

Using MTC and preventing useless sequences improves the results somewhat, though the solutions are still quite far from optimal.


### TMD 

|                            | Number Solved | Average Time | MinL |   AveL  |  MaxL |
|:---------------------------|------------:|-------------:|-----:|--------:|------:|
| [TMD (no cycles)](TMD)     | 250  |    0.053    |   2  | 237.8   |  771   |
| TMD (no cycles, prevent)   | 250  |    0.003    |   2  |  48.8   |  267   |

Preventing useless sequences improves the results significantly for TMD (unscaled).  

### TMD Rounding down

|                          | Number Solved | Average Time | MinL |   AveL  |  MaxL |
|:-----------------------------|------------:|-------------:|-----:|--------:|------:|
| [TMD Floor (no cycles)](ScaledEstimates)   | 250 |  0.015  |  2  | 124.5   | 401   |
| TMD Floor (no cycles, prevent)     | 250 |  0.016  |  2  | 134.6   | 1031   |

For TMD scaled using rounding down, the results are actually worse when useless sequences are prevented!  

### TMD Rounding up

|                            | Number Solved | Average Time | MinL |   AveL  |  MaxL |
|:-----------------------------|------------:|-------------:|-----:|--------:|------:|
| [TMD Ceil (no cycles)](ScaledEstimates)     | 250 |  0.008  |   2  |  84.5   | 428  |
| TMD Ceil (no cycles, prevent)               | 250 |  0.007  |   2  |  92.5   | 351  |

For TMD scaled using rounding up, preventing useless sequences has little effect on the average solution time, or the average length of solution, but perhaps a little better on the maximum solution length.

Finally, here's a table showing how the new results compare to methods that can produce optimal solutions.

|                              | Number Solved | Average Time | MinL |   AveL  |  MaxL  |
|:---------------------------|------------------:|-----------:|-----:|--------:|-------:|
| [Enhanced IDS](IDS_B)          | 250 |    5.736    |   2  | **6.1** |  **8** |
| [Look-up Table](LUT)           | 250 |    0.00002  |   2  | **6.1** |  **8** |
| [Distance Table](DIST)         | 250 |    0.0002   |   2  | **6.1** |  **8** |
|  MTC (no cycles, prevent)      | 250 |     0.022   |   2  | 171.2   |  713   |
| TMD (no cycles, prevent)       | 250 |     0.003   |   2  |  48.8   |  267  |
| TMD Floor (no cycles, prevent) | 250 |     0.016   |   2  | 134.6   | 1031  |
| TMD Ceil (no cycles, prevent)  | 250 |     0.007   |   2  |  92.5   |  351   |

As shown, combining greedy search, cycle prevention, and useless sequence prevention, with ordinary unscaled TMD produces the best results.  But even in this configuration, the average solution length is quite far from optimal.

**Looking forward.**
We might be able to make some progress trying to come up with more accurate distance estimates.  I have spent many hours mulling over different ways to calculate distances for Toroidal, and it doesn't seem to have brought any success.  But I may return to this topic one day.

In the next article, I'll describe [A-star search](/solver/AStar).  

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/ScaledEstimates_B.html).

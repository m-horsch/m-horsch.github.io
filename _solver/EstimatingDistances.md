---
layout: post
title:  "Towards solving Toroidal using a distance estimate"
date:   2025-02-22
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
This article describes an approach to solving Toroidals by estimating the true distance between a given state and the goal state.  This approach opens up a large number of options for different ways to calculate a distance estimate.  In this article, I provide some background information to help evaluate the utility of different distance estimates.  

### Details
In the previous article, I showed how to [search with distance information](/solver/DIST.html) by constructing a distance table in which each possible state is stored with the exact (shortest) distance to the goal state.  For Toroidals larger than 3x3, the approach seems totally infeasible, at least for the kinds of computers I have access to.  There are far too many possible states to calculate or store the distance table.  It may be possible to use a few tricks to reduce the size of Toroidal distance tables, but I am not too optimistic that these tricks will help us solve 4x4 Toroidals, or larger.  I'll leave that kind of exploration to future work.

It is worth considering whether the distances might be calculated somehow, without exploring every possible state and building a table from the collected data.  Distances could be calculated when they are needed, without being stored in a table.  As long as the formula does not require too much time to use, it would be feasible for Toroidals larger than 3x3.

I don't currently know any formula to calculate exact distances in Toroidal.  I don't believe such a formula exists, but that's just an educated guess. 

While a calculation of exact distances is (probably) out of reach, we can certainly propose some formulae to *estimate* the distances. Armed with such a formula, we could consider using the estimated distances instead of the true distances.  If the estimates are good enough, we may be able to use them effectively to solve Toroidals using the greedy algorithm.  But if not, we may still be able to use them, in a different way, which will be discussed in a later article.

Before we get to the distance estimates, let's look at the criteria that we'll be using to assess their utility.


**Error categories.**
The difference between an estimated distance and the true distance is called the error in the estimate; or simply, the error. There are different kinds of errors:
* Zero error, which means that the formula produced the correct distance. 
* Positive error, which means that the formula over-estimated the true distance.
* Negative error, which means that the formula under-estimated the true distance.

Any method to estimate a distance might demonstrate all three kinds of errors, for different states.  It might over-estimate the distance for some states, and under-estimate the distance for other states; there may be some states where the estimate has zero error. Naturally, we'd prefer errors to be relatively small; the closer to zero, the better! 

Calculating the an error assumes that we have the correct distances in advance.  For 3x3 Toroidals (and smaller), this assumption is true, but for larger Toroidals, we do not have the correct distances.  We might still be able to reason about what kinds of errors an estimation formula might make.  For example, we might be able to infer from the formula that it will make a mix of error categories, whereas we might be able to infer that a different formula might only under-estimate the true distances.  This is still useful information, even if we can't calculate exact errors.

**Data presentation.**
There are a number of ways we could evaluate distance estimates.   We will focus on solving 3x3 Toroidals only, so that we can make use of the exact distances.  We will try to generalize this specific case to larger Toroidals, but we will of course require caution.  

One of ways to evaluate the viability of a Toroidal distance estimate is to use what's called a *confusion matrix.*  It's basically a table that compares true values to estimated values. The small table below is not real data, but it will help describe how a confusion matrix works.

|--------:|------:|------:|------:|
|         |   *0* |   *1* |   *2* |
|   **0** |     1 |     0 |     7 |
|   **1** |     2 |     4 |     0 |
|   **2** |     0 |     0 |     9 |

The first column in each row shows a true distance; these are numbers in **bold**.  The first row shows an estimated distance value; these are *slanted* numbers.  To keep this example brief, the true and estimated distances are 0, 1, and 2.  The data in the table are counts; specifically, we are counting Toroidal states.  For example, there is a count of 1 state where the true distance and the estimated distance agreed on the value 0.  Also, there were 7 states for which the true distance was **0**, but the estimated distance was *2*.

In a confusion matrix, the numbers along the diagonal (from top left to bottom right) show how often the estimates were exactly right.  In the example, there were 1+4+9=14 states where the distance estimate had zero error. Numbers above this diagonal show how often the distances were over-estimated; numbers below this diagonal show under-estimates.  Just looking at this table, the zero error counts are the majority, and only relatively small counts for positive or negative errors.

Keep in mind that this small example is fiction, made up for the purposes of explaining what a confusion matrix is.  For 3x3 Toroidals, the range of true distances will be 0-8.  The range of estimates will depend on the formula used to estimate the distance.  So a confusion matrix for 3x3 Toroidals will be quite a bit larger than this example.

Another way to evaluate distance estimates is by summarizing the errors in a numeric value, such as a *root mean squared error* (RMSE).  This is what you get if you calculate the error values (positive and negative) and then square them, take the average, and then take the square root of the average.  It's typically used for numeric estimates in science and engineering.  For example, the RMSE for the above table is 1.14.  This is relatively  low.

**Application in Greedy Search**
The final way to evaluate the distance estimates is to apply them as I suggested above.  If we replace the distance table lookup (DIST) with the formula to estimate the distance, we can find out whether the formula can be used to solve 3x3 Toroidals, how much time is used, and how good the solutions are, in terms of length.  
  
**Exact Distances.**
I thought it might be useful to present the best possible estimate, namely the true distance. I can do this because I've already calculated the exact distances using the [Distance Table method](/solver/DIST.html) described previously.  Remember that this is for presentation purposes; there's no way to calculate this data efficiently; I obtained this data by enumerating all possible states, which is too expensive to calculate at every step.  Still, we can use this as a hypothetical gold standard.  The confusion matrix is below.

|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|
|       |   *0* |   *1* |   *2* |   *3* |   *4* |   *5* |   *6* |   *7* |   *8* |
| **0** |     1 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **1** |     0 |    12 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |
| **2** |     0 |     0 |    96 |     0 |     0 |     0 |     0 |     0 |     0 |
| **3** |     0 |     0 |     0 |   736 |     0 |     0 |     0 |     0 |     0 |
| **4** |     0 |     0 |     0 |     0 |  5208 |     0 |     0 |     0 |     0 |
| **5** |     0 |     0 |     0 |     0 |     0 | 28674 |     0 |     0 |     0 |
| **6** |     0 |     0 |     0 |     0 |     0 |     0 | 89497 |     0 |     0 |
| **7** |     0 |     0 |     0 |     0 |     0 |     0 |     0 | 54741 |     0 |
| **8** |     0 |     0 |     0 |     0 |     0 |     0 |     0 |     0 |  2475 |

Notice that the positive counts are exactly on the diagonal.  In other words, there are no errors at all.  As we come up with formulae to estimate 3x3 Toroidal distances, the closer we get to this table, the better. Because there are no errors, the RMSE is 0.0.  We've already used the true distances to solve 3x3 Toroidals [here](/solver/DIST.html), but I'll copy the search results below.

|                | 3x3 Number Solved | 3x3 Average Time  |
|:---------------|-----------:|-----------:|
| [Simple IDS](/solver/IDS_A.html)    | 62 |    *322s*   |
| [Enhanced IDS](/solver/IDS_B.html)  | 100 |   12.6s    |
| [Look-up Table](/solver/LUT.html)   | 100 |   0.0001s  |
| [Distance Table](/solver/DIST.html) | 100 |    0.0005s  |




**Looking forward.**
In the next article, we'll see how to estimate distance using the [misplaced tile count (MTC)](/solver/MTC).

**Data Provenance.**
Detailed information about the data summarized in this article (specifically, the data for "Exact Distances") can be found [here](/dataprovenance/DIST.html).

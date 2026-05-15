---
layout: post
title:  "Maximum Shortest Solution length"
date:   2025-09-09
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
</style>

Any Toroidal puzzle has an optimal solution, namely the shortest sequence of moves that solves it.  The *maximum shortest solution* is the longest optimal solution over a whole class of Toroidals.  For example, the class of 3x3 Toroidals, with no exchangeable tiles has solution lengths at most 8 moves long.  In terms of a [state space graph](/solver/StateSpace.html), this distance is the farthest away any state can be from any other state, in terms of the number of transitions.

I will use the abbreviation "MSSL" to refer to the length of the maximum shortest solution.  This concept is called "God's number" when applied to (normal 3x3x3) Rubik's cubes.  See [God's algorithm](https://en.wikipedia.org/wiki/God%27s_algorithm).  

It's important to provide the context of the class of Toroidal when considering the MSSL.  Toroidals with higher dimensions will tend to have longer solutions.  Toroidals with more exchangeable tiles will tend to have shorter solutions than no exchangeable tiles, and even with fewer.  

In the table below, I enumerate the known maximum shortest solution length that I have discovered, for Toroidals that have no exchangeable tiles.  

| Rows | Columns | Number of States | MSSL | ASSL | 
|-----:|--------:|-----------------:|-----:|-----:|
|   2  |    2    |        4!        |   4  | 2.17 | 
|   3  |    3    |        9!        |   8  | 6.10 |

This data was established by enumerating all possible move sequences in an iterative deepening process.  See [this page on iterative deepening](/solver/IDS_A.html), and [this page on mapping the Toroidal state space](/solver/LUT.html).  I doubt that I can use this method to determine the MSSL for larger Toroidals.

Edit: I added a column labelled "ASSL," which is an abbreviation for *Average Shortest Solution Length,*  and the intent is to determine the average over all possible initial configurations for Toroidals of the same order.  For 2x2 and 3x3 Toroidals, the ASSL can be calculated directly.  For larger Toroidals, the ASSL can only be estimated, as the number of possible configurations is too large to enumerate.

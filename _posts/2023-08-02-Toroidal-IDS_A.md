---
layout: post
title:  "Iterative Deepening Search"
date:   2023-08-02 
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
**The simplest possible computer algorithm for Toroidal.**
One of the most basic computer algorithms we can try to use to solve Toroidal is called
Iterative Deepening Search (IDS).  IDS is a general purpose algorithm that can be applied to many different kinds of search problems.  For our purposes, IDS is a way to look through all the possible move sequences by trying all the sequences with 1 move only, then all the sequences with 2 moves, then all the sequences with 3 moves, etc.  Since we know that every Toroidal is solvable, we will find a solution eventually, and it will be the shortest possible solution.

I have a simplistic implementation of IDS which does nothing fancy.  I called it simplistic because it tries all the possible combinations of moves, even the ones that are obviously not worth trying, such as `L1 R1 L1 R1` which does nothing useful.  We're giving it a try because it's simple, but we shouldn't be too hopeful.  It's basically a place to start, and to appreciate the difficulty of the problem.

**Running the program.**
I created 100 random 3x3 Toroidal problems.  These Toroidal problems have 9 unique tiles (that is, there are no exchangeable tiles to make the problem easier), and each Toroidal was scrambled using 500 random moves.  I applied my simplistic version of IDS to these 100 problems.  The solver was given a time limit of 120 seconds, which is plenty of time for a human to solve a 3x3 Toroidal.  

The implementation of IDS was able to solve 62 out of 100 3x3 Toroidals.  The other 38 were unsolved.  Of these 62 solved problems, there were solutions of length 4, 5, and 6.  The remaining 38 Toroidals have solutions of length 7 and higher, which this version of the solver was unable to find within the 2 minute time limit.  The results are summarized below:

| Solution length | Number | Average time (seconds)|
|:-:|--:|--:|
| 4 | 7 | 0.47 |
| 5 | 17 | 4.20 |
| 6 | 38 | 38.90 |

The trend in the average time required to solve Toroidal is pretty obvious.  Solutions of length 5 take about 10 times longer to find on average than solutions of length 4.  Likewise, solutions of length 6 also take about 10 times longer that solutions of length 5.  We could be more precise than "about 10 times" but we're just getting started, so there's no need to nail down this factor precisely.

If we assume that this trend continues to solutions of length 7 and 8, we can predict that to find a solution of length 7, we’ll need about 10 times longer than we needed for solutions of length 6, or about 400 seconds (just over 6 minutes).  To find solutions of length 8, about 4000 seconds (just over 1 hour).  

**Simplistic IDS is not good enough.**
These estimates are not good news for the simplistic IDS implementation.  A human should be able to solve any 3x3 Toroidal in a couple of minutes.  Clearly, the simplistic strategy of checking every sequence up to length 8 considers too many useless sequences.  

**What's the problem?**
Since this version of the solver looks at all the possible sequences, the time needed by the simplistic IDS program to solve a Toroidal depends on how many move sequences there are.  

Let’s consider a 3x3 Toroidal first, to work through the numbers.  There are 12 possible moves you can make at any point: Up and Down on every column, and Left and Right on every row.  Therefore there are 12 sequences of length 1.  Each one of these can be extended by 12 possible moves, so the total number of sequences of length 2 is 
$$\small 12 \times 12 = 144$$.
In general, there are $$\small 12^k$$ sequences of length $\small k$.  The number of sequences gets 12 times bigger every time $\small k$ increases by 1.  

It turns out that for 3x3 Toroidals, the minimal solution has 8 moves or fewer.  I’ll have to explain why this is true in another posting.  So we only have to be able to look at sequences of length 8 to find a solution to any 3x3 Toroidal.  Using the calculations from earlier, we know that there are $\small 12^8 = 429981696$ sequences of length 8.  About 430 million.  A lot of these 430 million are going to be totally useless sequences.  It would be better to trim those before letting IDS take a long run at solutions of length 8, or even solutions to 4x4 or bigger.

In a future posting, I will show that IDS can do significantly better if it does not include all the useless sequences.  I'll post the results first, and then later discuss how I managed to get rid of the useless sequences.  

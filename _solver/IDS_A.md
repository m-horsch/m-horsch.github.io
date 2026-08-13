---
layout: post
title:  "Iterative Deepening Search"
date:   2023-09-06 
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

Revised: 2026-08-06

### Abstract
A simple implementation of Iterative Deepening Search (IDS) is applied to 3x3 Toroidals.  This application of IDS is called Simple IDS because it tries all possible sequences of moves.  We investigate the effectiveness of Simple IDS by applying it to a set of 250 randomly scrambled 3x3 Toroidals. 
The algorithm was given a time limit of 2 minutes, and was only able to solve 199 of these examples.  Ignoring the unsolved examples, the average time used by Simple IDS to solve these 199 examples was about 47 seconds.

### Details
**The simplest possible computer algorithm for Toroidal.**
One of the most basic computer algorithms we can try to use to solve Toroidal is called
Iterative Deepening Search (IDS).  IDS is a general purpose algorithm that can be applied to many different kinds of search problems.  For our purposes, IDS is a way to look through all the possible move sequences by trying all the sequences with 1 move only, then all the sequences with 2 moves, then all the sequences with 3 moves, etc.  Since we know that every Toroidal is solvable, we will find a solution eventually, and it will be the shortest possible solution.

I have an implementation of IDS which does nothing fancy.  I called it simple because it tries all the possible combinations of moves, even the ones that are obviously not worth trying, such as `L1 R1 L1 R1` which does nothing useful.  We're giving it a try because it's simple, but we shouldn't be too hopeful.  It's basically a place to start, and to appreciate the difficulty of the problem.

**Running the program.**
I created 250 random 3x3 Toroidal problems. 
These Toroidal problems have 9 unique tiles (that is, there are no exchangeable tiles to make the problem easier), and each Toroidal was scrambled using 50 random moves.  I applied my Simple version of IDS to these 250 problems.  The solver was given a time limit of 120 seconds, which is plenty of time for a human to solve a 3x3 Toroidal.  

Note: While working on this Simple IDS solver, I began to suspect that every 3x3 Toroidal could be solved in 8 moves or less.  In later experiments, I was able to confirm this suspicion.  [A future article will report on this.](/technical/maximum)

The implementation of IDS was able to solve 199 out of 250 3x3 Toroidals.  The average time to solve these problems is 47.1 seconds
Of these 199 solved problems, there were solutions of lengths 2-7.  The remaining 51 Toroidals have solutions of length 7 and higher, which this version of the solver was unable to find within the 2 minute time limit.  The results are summarized below:

| Solution length | Number | Average time (seconds)|
|:-:|--:|--:|
| 2 |  1 |  0.002   |
| 3 |  0 |  N/A     |
| 4 |  8 |  0.210   |
| 5 | 52 |  1.930   |
| 6 | 98 | 23.437   |
| 7 | 40 | 81.109   |
| 8 |  0 | N/A      |


**Simple IDS is not good enough.**
These estimates are not good news for the Simple IDS implementation.  A human should be able to solve any 3x3 Toroidal in a couple of minutes.  The Simple strategy of checking every sequence up to length 8 considers too many useless sequences.  

**Looking forward.**
In the next article, I'll describe a technique for [preventing useless sequences](/solver/IDS_B.html).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/IDS_A.html).
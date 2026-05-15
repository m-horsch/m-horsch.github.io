---
layout: post
title:  "3x3 Toroidals considered solved!"
date:   2026-02-11
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


### The story so far
I started my accounts with [a strategy (IDS) to try all possible sequences of moves](IDS_A).  In IDS, the move sequences are explored without any guidance, and all options are explored, until an optimal solution is found, eventually.  Because there are so many possible move sequences to explore, I employed a technique to [prevent useless sequences](IDS_B), which significantly increased the speed of the IDS solver, but the results for 3x3 Toroidals were still rather unimpressive compared to competent human solvers.  

I described [look-up tables](/solver/LUT), and [distance tables](DIST), which eliminate the need to explore possible solutions entirely.  These techniques are all guidance, and no exploration. In a sense, these two approaches don't solve the Toroidal problem, but automate the output of already-known solutions to every possible 3x3 Toroidal.  These techniques are extremely fast for 3x3 Toroidals and smaller, but there is a somewhat large table that needs to be stored in memory.

I tried [greedy search using distance estimates](ScaledEstimates), and as long as cycles are prevented, solutions can be found pretty quickly, though the solutions are quite far from optimal.  Memory requirements for greedy search methods are very small.

Most recently, I described [A\* search using TMD-based distance estimates](AStar), which was able to find optimal solutions to 3x3 Toroidals very quickly.  The memory requirements are modest, compared to the table-based methods.

### Done, at least for 3x3!
The results from A\* search seem to be pretty good.  Using a scaled TMD distance estimate, A\* search is able to find optimal solutions to any 3x3 Toroidal in less than one-tenth of a second on average, using very reasonable amounts of memory.  

While it is possible that a better distance estimate might be waiting to be discovered, I don't think we need it to solve 3x3 Toroidals.  Since introducing this problem in my courses, I have spent considerable time mulling over different ways of looking at Toroidal puzzles (sitting at my desk, or walking trails, or as an alternative to counting sheep when I am sleepless).  This topic will remain on the back-burner for the time being.


### Looking forward.
I've already mentioned that some of the techniques that I covered so far cannot be feasibly applied to Toroidals larger than  3x3.  These include methods based on Iterative Deepening Search (both [Simple](IDA_A) and [Enhanced](IDS_B)), as well as table-based methods ([LUT](LUT) and [DIST](DIST)).  A 4x4 Toroidal has far too many possible states to try looking through them all, much less store a table with all of them.  So I will say no more about them.

In the next article, I'll report on using greedy search, as well as A\* Search on these [larger Toroidals](FourByFour). 

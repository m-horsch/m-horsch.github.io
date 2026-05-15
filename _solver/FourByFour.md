---
layout: post
title:  "Solving larger Toroidals: 4x4 and up"
date:   2026-03-11
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

### Draft
 * structure, basic intro
 
### Abstract
In this article, I apply A\* search to a set of randomized 4x4 Toroidals, using unscaled TMD, and the scaled TMD, rounding up.  I was able to solve 100 randomized 4x4 Toroidals, paired with unscaled TMD with A\* search, using roughly 2.7 seconds, on average, and the solution had length roughly 19.5.  This is certainly not optimal, but is plausibly close.
When paired with scaled TMD, rounding up, A\* was able to solve only 7 of the 100 problems within the time limit of 2 minutes.  The seven solved problems required 12.9 seconds each, on average.  The seven solved problems had solutions of length 12, and these solutions are optimal.

### Details
I consider 3x3 Toroidals "finished," in the sense that any one of several approaches can be used to solve them in a tiny fraction of the time that a human being would need.  It would be a software engineering exercise to add a "Solve" button to my Toroidal page, or even to give hints to the player.  

However, for larger Toroidals, only one of the techniques I've discussed previously can be applied to 4x4 Toroidals, or bigger.  The [state space for 4x4 Toroidals is so large](StateSpace) that IDS, LUT, DIST are simply infeasible.  As we will see, A\* can be applied with moderate success, but still leaves a little to be desired.


**Looking forward.**
In the next article, I'll describe a technique that tries to find a practical compromise of optimality requiring a reasonable amount of time.

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/ScaledEstimates_B.html).
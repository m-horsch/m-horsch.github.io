---
layout: post
title:  "Explorations using LUT and DIST for larger Toroidals"
date:   2023-11-01
---

### Outline
  * LUT and DIST work quickly for 3x3 Toroidals, but complete tables for larger Toroidals are infeasible.
  * It's feasible to store partial tables, say 4-5 moves deep.  Starting from the goal position, store all possible states up to some step limit.
  * Use any heuristic method M to search for a solution, and in the loop that extends a move sequence, check whether the current state is in the table, meaning a path has been found.
  * This path may not be optimal, depending on our choice of M.  
  * Variation: Suppose we choose a heuristic method M that is not optimal.  Once M discovers a path from a start state S1 to that state S2 in the table, make the problem a little simpler by using M again to search from S1 again, but making S2 the goal state. There are 3 possible outcomes.
    1.  M finds the same path again.  
    2.  M finds a better path, because it is looking more specifically for S2.
    3.  M finds a worse path, because M is not optimal.
  * Variation.  Suppose M is not optimal.  We could employ a heuristic method M2 that is known to provide better quality solutions, to search from S1 to S2.    
  * Variation.  M2 could reuse the partial table, so that in the search from S1 to S2, we check whether the current state is in the table.  To make this work best, we find the permutation p of state labels in S2 so that p(S2) = the goal state.  Apply this permutation to S1 as well.  Now we can reuse the table to find a path from p(S1) to p(S2).  This path can be combined with the path stored in the table from S2 to the (original) goal state.

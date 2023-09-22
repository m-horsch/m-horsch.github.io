---
layout: post
title:  "Prioritizing Promising Sequences"
date:   2023-08-16
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

Previously, I wrote about [the strategy to try all possible sequences of moves]({% post_url 2023-08-02-Toroidal-IDS_A %}).  The take-away from that posting was that there are too many possible move sequences to explore.  I discussed [the importance of preventing useless sequences]({% post_url 2023-08-09-Toroidal-IDS_B %}), which enabled a significant increase in the speed of the IDS solver.  There are still too many sequences, even when we prevent the really useless ones.  I ended with the idea that we should prioritize promising sequences.  Let's make that idea clearer, and show how well it works.

**Priorities.**  The IDS solvers worked through all possible move sequences (the second version ignored the useless ones), looking first at sequences of length 1, then length 2, etc, stopping only when a solution was found.  This can be interpreted as a weak kind of prioritization, where the shorter sequences are given higher priority than longer sequences.  I call it weak, because it doesn't care very much about the effect of the sequences.  All sequences of the same length are considered equal.

We will construct promising sequences by actually looking at the effect of the moves. 
A move that leaves the Toroidal scrambled is not very promising.  A move that puts a few tiles into place seems more promising.  We'll build up promising sequences by adding promising moves to already-promising sequences.  The hope is that an exploration of a few promising sequences will solve Toroidal much faster than exploring all possible sequences.  And we'll see at the end of this posting that this can be done, at least for 3x3 Toroidals.

The easiest way to measure the effect of a sequence of moves is to *count the number of tiles that are not in the correct position*.  When the Toroidal is highly scrambled (see below, left), almost all of the tiles will be in the wrong position, so the count will be relatively high.  When the Toroidal is partially solved (see below, middle), only a few tiles will be out of position, and the count will be relatively low.  And when the Toroidal is solved (below, right), zero tiles are out of position. 

![Three configurations, illustrating the concept of misplaced tiles as a measure of priority.](/TImages/TLC_Overview.png){:width="60%"}

**Coming down from the mountains.**  This prioritization describes something like a mountain range.  Every configuration of tiles is a location in the Toroidal Mountain range, and every move (i.e., every push of a row or column) takes you to a new location in the range.  Furthermore, the number of misplaced tiles in the configuration is the altitude of your location.  If the configuration is very scrambled, the location is a high peak in the mountains.  The configurations where some tiles are in the right place are lower than the peaks, and the one place where all the tiles are in the right place is the lowest location in the Toroidal Mountain range.  

Looking at it this way, a solution is a path through this mountain range, from some initial location, down to the lowest point, which is the solved Toroidal.  Why can't we just write a program to simulate a hiker that just walks downhill? 
Like real world mountain ranges, the Toroidal Mountain range has peaks, hills, ridges, flat meadows, and valleys.  If you're on a peak, every direction is downhill, but some of these downhill steps do not get you closer to your goal.  For some places in the mountain range, there is no way to move downhill.  You could step onto a large flat meadow where every step is at the same level, or in a basin where every move takes you uphill again.  Real mountain ranges are like this, and the Toroidal range is like this as well.

![A landscape painted by Stable Diffusion, using text chosen from the above paragraph.](/TImages/ToroidalLandscape.png){:width="40%"}

Even if we gave the hiker a map of the Toroidal Mountain range, it wouldn't get them very far because the Toroidal Mountains are not exactly like real mountain ranges.  They are more like a complex game of snakes and ladders; snakes move you downhill, and ladders take you uphill.  Furthermore, in the Toroidal Mountains, when you take a step, you don't know whether you're going to step on a snake or a ladder.  In the Toroidal Mountains, one step can take you very far away from the other steps you could have taken.  A map for a real world place gives you some clues about how to get to where you want to go.  A map of the Toroidal Mountains is as much a puzzle as the Toroidal puzzle it represents!  

**Many hikers get the job done.**  So while we can't solve Toroidal by simulating a solitary hiker trying to get home, we can use the landscape to guide our explorations by simulating many hikers all trying different paths.  We'll select one of the hikers, namely the one whose position seems most promising at the moment, and advance that hiker in a promising direction.  

To be a bit more precise, the hikers are not advanced, they are copied, or cloned.  The hiker at the most promising location gets copied, and a copy is placed at each location one move away. We will not place a copy for moves that are useless, nor for moves that return to a previously visited location.  

This strategy advances a number of promising hikers forward, leaving the hikers at unpromising locations where they are.  If any of the hikers at primising locations moves into an unpromising location, we let them stay there while we advance other hikers.  The notion of being in a promising location is relative: we compare the locations of all the hikers, and advance the most promising one.  The most promising one can change at every turn.  Hikers in locations that seemed unpromising may eventually be the best, which could happen if all the other hikers get stuck in even worse locations.

**Penalize long sequences.**  There's one more technical detail to add, which is a penalty on long move sequences.  Each hiker knows the length of the path they've traced, and each move adds one to the penalty.  It's entirely plausible that many, if not all, of the hikers will eventually find themselves wandering around in a large meadow, or a bowl, in the Toroidal Mountains.  The penalty on long sequences will not prevent hikers from wandering in the meadows, but it will give a chance to other hikers whose altitude is higher, but whose route is shorter.

**Running the algorithm.**  I applied this heuristic strategy to the same 100 3x3 Toroidal problems that we used before.  I will summarize the results with the following table:

| Solution length | Plain IDS | Enhanced IDS | A\*  |
|:-:|--:|--:|--:|
| 4 |     0.47  |   0.101 |   0.001 |
| 5 |     4.20  |   0.631 |   0.004 |
| 6 |    38.90  |   3.710 |   0.012 |
| 7 |   *400*   |  20.900 |  0.023  |
| 8 |   *4000*  | 126.700 |  0.066  |

The data in the columns labelled **Plain IDS** and **Enhanced IDS** are average times to find a solution of the given length, copied from the previous postings (italics represent estimated times).  The new column labelled <strong>A\*</strong> is also an average time in seconds, and shows that A\* can solve 3x3 Toroidals in tiny fractions of a second.  On average, this heuristic strategy is more than 500 times faster than the **Enhanced IDS** strategy.  

The combination of multiple hikers, a prioritization scheme, and a penalty on length, is pretty effective for 3x3 Toroidals.  Surely this problem can be considered "solved." 

**Not so fast.**  I applied this heuristic technique to 4x4 Toroidals.  I mentioned in the previous posting that IDS couldn't solve any of them in a time limit of 60 seconds, even if all the useless sequences are prevented.  Well, A\* cannot solve any of them either.  At least, not yet.  The problem is that 4x4 is not just a little bigger than 3x3.  It's a lot bigger.  The first thing we'll try is a better heuristic, which will focus even more on the promising arrangements.  Surprisingly, it will help a little.  

**Almost as good.**  While IDS took a long time, it is guaranteed to find the shortest possible sequence of moves to solve a Toroidal.  And it is possible for A\* to have such a guarantee, but counting the number of misplaced tiles cannot provide such a guarantee.  The reason is somewhat technical, but in simple terms, counting misplaced tiles sometimes makes a configuration look worse than it actually is.  It's a small problem that we could fix.  As it stands, most of the solutions found by our prioritization scheme are optimal, but a few are one or 2 moves longer than what IDS would find.  For some purposes, a pretty good solution very quickly might be more valuable than the best solution some time (possibly a long time) later.  We'll explore this a bit more as well.


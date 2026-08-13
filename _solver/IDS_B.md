---
layout: post
title:  "Preventing useless sequences"
date:   2023-09-20
usemathjax: true
---
<style>
blockquote 
{
    color: #111;
    letter-spacing: 0px;
    font-size: 16px;
}
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
</style>

Revised: 2026-08-06

### Abstract
By preventing many (but not necessarily all) useless sequences of moves, an Enhanced version of Iterative Deepening Search is shown to be more effective than Simple IDS.
Enhanced IDS is applied to a set of 250 randomly scrambled 3x3 Toroidals.  The algorithm was given a time limit of 2 minutes, and was able to solve all 250 examples.  The average time used by Enhanced IDS to solve these examples was about 5.736 seconds.

### Details
**Some sequences are not worth looking at.**
In [a previous article](/solver/IDS_A.html), I described a simple version of Iterative Deepening Search (IDS), which explored all possible move sequences, starting with length 1, then length 2, etc.  This is a very simple algorithm to build, and it is guaranteed not to miss a solution, provided you give it enough time.

The problem with this simple approach is that it looks at far too many sequences, most especially, sequences that are totally pointless, like `L1 R1 L1 R1`; this sequence pushes row 1 back and forth, making no progress at all.  It also considers `L1 L2` to be a different sequence than `L2 L1` even though the two have identical effects on a Toroidal. 
Because  Simple IDS spends time looking at all the useless sequences, it can take a long time to find solutions to relatively simple Toroidal puzzles.

**Preventing useless sequences.**
Briefly, avoiding useless sequences can be accomplished by a simple principle. 
> If two different sequences of moves have the same effect, and one is shorter than the other, prevent trying the longer one.
> If two sequences have the same effect, and they are the same length, choose one of them, and prevent the other.  

Admittedly, my description is a bit vague, and we can only apply it in a limited way.  I will describe it in more detail in a separate article.  I didn't want this one to be too long.  But I will provide examples to give the basic idea.
1. Useless sequences like `L1 R1` are prevented, because it's equivalent to a sequence with zero moves.
2. In a 3x3 Toroidal, the sequence `L1 L1` is equivalent to the single action `R1`.  So we will allow `R1` to be tried, because it's shorter, but `L1 L1` will be prevented.
3. In a 4x4 Toroidal,  `L1 L1` and `R1 R1` have the same effect; we arbitrarily allow the first sequence, and prevent the second.  
4. The sequences `L1 L2` and `L2 L1` have the same effect, so we will arbitrarily allow the first, and prevent the second.

With just this one principle, we can eliminate a vast quantity of useless moves.  This principle can be encoded in the part of the program that proposes new moves to add to a sequence.  I was able to encode this feature in such a way that the potentially useful moves were calculated only once, at the start of the program, as opposed to having them be recalculated every time the solver considers a move.  This boosts the speed of the solver even more.   

**Running the program.**
I applied my Enhanced version of IDS to the same set of 250 random 3x3 Toroidal problems as in the previous article.  The solver was given a time limit of 120 seconds, just as in the previous article.

It was able to solve 250 out of 250 3x3 Toroidals.  The average time required was 5.736 seconds.  The results can be broken down by solution length, as follows:

| Solution length | Number | [Simple IDS](/solver/IDS_A.html) | Enhanced IDS | 
|:-:|--:|--:|--:|
| 2 |  1 |     0.002  |  0.001 |
| 3 |  0 |     N/A    |  N/A   |
| 4 |  8 |     0.210  |  0.047 |
| 5 | 52 |     1.930  |  0.288 |
| 6 | 98 |    23.437  |  2.377 |
| 7 | 83 |    *81.109*  | 10.060 |
| 8 |  9 |    N/A     | 38.965 |

The column **Simple IDS** shows the times  (in seconds) needed by the Simple IDS implementation from before.  Note that for problems with Solution length 7, Simple IDS only solved 40 of the 83 problems in the dataset; so the average time is italicized for emphasis.

The column labelled **Enhanced IDS** shows the time (in seconds) needed when useless sequences are avoided, as described above.  Remember that these are averages, and that individual Toroidal puzzles can take longer or shorter than these average times.


**Enhanced IDS is still pretty slow.**
Avoiding useless sequences speeds up IDS significantly.  Going forward, we will continue to use this principle of avoiding useless sequences, whenever it makes sense.  

When we avoid useless sequences, on average, IDS will be very fast compared to a human.  For problems whose solution length is 8 moves, the average of 38.9 seconds is still very competitive compared to a human solver.  However, we can't be too satisfied with these results, because this method is unlikely to be able to solve Toroidals larger than 3x3.  We'll need a substantially faster method to tackle such problems.

**Looking forward.**
In the next article, we'll explore the structure of the puzzle by describing the [Toroidal state space](/solver/StateSpace.html).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/IDS_B.html).
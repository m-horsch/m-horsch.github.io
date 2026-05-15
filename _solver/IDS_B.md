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

### Abstract
By preventing many (but not necessarily all) useless sequences of moves, an Enhanced version of Iterative Deepening Search is shown to be more effective than Simple IDS.
Enhanced IDS is applied to a set of 100 randomly scrambled 3x3 Toroidals.  The algorithm was given a time limit of 2 minutes, and was able to solve 99 of these examples.  The average time used by Enhanced IDS to solve these examples was about 12.6 seconds.

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
I applied my Enhanced version of IDS to the same set of 100 random 3x3 Toroidal problems as in the previous article.  The solver was given a time limit of 120 seconds, just as in the previous article.

It was able to solve 99 out of 100 3x3 Toroidals.  I forced the solver to find a solution to the last remaining example, which had a solution of length 8, and took 226s.  Since this was the only one, I included it in my statistical calculations. The results are summarized below:

| Solution length | Number | [Simple IDS](/solver/IDS_A.html) | Enhanced IDS | 
|:-:|--:|--:|--:|
| 4 |   7 |       0.47 |   0.101 |
| 5 |  17 |       4.20 |   0.631 |
| 6 |  38 |      38.90 |   3.710 |
| 7 |  35 |    *350*   |  20.900 |
| 8 |   3 |   *3150*   | 126.700 |

The column **Simple IDS** shows the times  (in seconds) needed by the Simple IDS implementation from before.  The column labelled **Enhanced IDS** shows the time (in seconds) needed when useless sequences are avoided, as described above.  Remember that these are averages, and that individual Toroidal puzzles can take longer or shorter than these average times.

Italicized values in the table are crude estimates.  In the previous article, I remarked that the solution averages for Simple IDS increase by about a factor of 9.  From this pattern, I guessed that Simple IDS would require about 350 seconds on average to find solutions of length 7, and 3150 seconds to find solutions of length 8.    

In the solution times for Enhanced IDS, we can see an increase of about a factor of 6.  This represents a substantial improvement, because this factor compounds with the depth of the shortest solution.  The deeper Enhanced IDS has to look, the better it will seem, compared to Simple IDS.  
 

**Enhanced IDS is still pretty slow.**
Avoiding useless sequences speeds up IDS significantly.  Going forward, we will continue to use this principle of avoiding useless sequences, whenever it makes sense.  

Even if we avoid useless sequences, IDS still takes a rather long time to solve the trickiest 3x3 Toroidals.  It will take even longer to solve Toroidals larger than 3x3.  

I have encountered computational problems where the best thing to do is let IDS work until it finds a solution; for example, automated mathematical reasoning applications.  In such applications, the target state is vague and generic, and it's hard to aim for it.  You'll have to take my word for it that it's true, since this is not the time or place for a digression on automated mathematical reasoning.  

However, the goal for Toroidal is not vague or generic; we know exactly how the tiles should be arranged when we're done.  It's entirely reasonable to take advantage of this, and try to choose moves that seem to get us closer to goal.  That's the direction for the next few articles.

**Looking forward.**
In the next article, we'll explore the structure of the puzzle by describing the [Toroidal state space](/solver/StateSpace.html).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/IDS_B.html).
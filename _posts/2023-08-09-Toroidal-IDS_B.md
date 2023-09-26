---
layout: post
title:  "Preventing useless sequences really helps"
date:   2023-08-09
categories:  Solver
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

**Some sequences are not worth looking at.**
In [a previous posting]({% post_url 2023-08-02-Toroidal-IDS_A %}), I described a simple version of Iterative Deepening Search (IDS), which explored all possible move sequences, starting with length 1, then length 2, etc.  This is a very simple algorithm to build, and it is guaranteed not to miss a solution, provided you give it enough time.

The problem with this simple approach is that it looks at far too many sequences, most especially, sequences that are totally pointless, like `L1 R1 L1 R1`; this sequence pushes row 1 back and forth, making no progress at all.  It also considers `L1 L2` to be a different sequence than `L2 L1` even though the two have identical effects on a Toroidal.  Even if `L1 L2` is useful, it is inefficient to look at it again as `L2 L1.`  Because  Simple IDS spends time looking at all the useless sequences, it can take a long time to find solutions to relatively simple Toroidal puzzles.

In this posting I will describe, somewhat vaguely, a more sophisticated version of IDS, which avoids the great majority of the useless moves that Simple IDS explored.  Then I will demonstrate the value of this technique, by showing how much faster it is.

**Preventing useless sequences.**
Briefly, avoiding useless sequences can be accomplished by a simple principle. 
> If two different sequences of moves have the same effect, and one is shorter than the other, prevent trying the longer one.
> If two sequences have the same effect, and they are the same length, choose one of them, and prevent the other.  

Admittedly, my description is a bit vague.  I will describe the theory behind it in a separate posting.  I didn't want this one to be too long.  But I will provide examples to give the basic idea.
1. Useless sequences like `L1 R1` are prevented, because it's equivalent to a sequence with zero moves.
2. In a 3x3 Toroidal, the sequence `L1 L1` is equivalent to the single action `R1`.  So we will allow `R1` to be tried, and `L1 L1` will be prevented.
3. In a 4x4 Toroidal,  `L1 L1` and `R1 R1` have the same effect; we arbitrarily allow the first sequence, and prevent the second.  

With just this one principle, we can eliminate the vast majority of useless moves.  This principle can be encoded in the part of the program that proposes new moves to add to a sequence.  It is far faster to generate only the good moves than it is to generate all moves, and then delete the ones you don't want.  

**Running the program.**
To demonstrate the value of this principle, I used the same set of 100 random 3x3 Toroidal problems as in the previous posting.  I applied my enhanced version of IDS to these 100 problems.  The solver was given a time limit of 120 seconds, just as in the previous posting.

The implementation of IDS was able to solve 99 out of 100 3x3 Toroidals.   I forced the solver to find a solution to the last remaining example, which had a solution of length 8, and took 226s.  Since this was the only one, I included it in my statistical calculations. The results are summarized below:

| Solution length | Number | Simple IDS | Enhanced IDS | Speed-up Factor |
|:-:|--:|--:|--:|--:|
| 4 |   7 |     0.47 |   0.101 |   3.96 |
| 5 |  17 |     4.20 |   0.631 |   6.47 |
| 6 |  38 |    38.90 |   3.710 |   10.03 |
| 7 |  35 |   *400*   |  20.900 |  *19.00*   |
| 8 |   3 |   *4000*  | 126.700 |  *32.00*   |

The column **Simple IDS** shows the times  (in seconds) needed by the Simple IDS implementation from before.  The column labelled **Enhanced IDS** shows the time (in seconds) needed when useless sequences are avoided, as described above.  The column labelled **Speed-up Factor** shows how many times faster Enhanced IDS is as compared to Simple IDS; for example, Enhanced IDS is 3.96 times faster than Simple IDS at finding solutions of length 4.  

Italicized values in the table are the result of educated guesses and calculations based on those guesses.
In a previous posting, we saw that Simple IDS was unable to find any solution of length 7 or 8 within the 2 minute time limit.  I guessed that Simple IDS would require about 400 seconds on average to find solutions of length 7, and 4000 seconds to find solutions of length 8.  If we accept these guesses as plausible, the estimated Factor for solutions of length 7 is about 19, and for 8 is around 32.  

**Faster IDS is not good enough.**
Avoiding useless sequences speeds up IDS significantly.  Going forward, we will continue to use this principle of avoiding useless sequences.

However, we've gone about as far as we can go with IDS.  Even if we avoid useless sequences, IDS still takes a rather long time to solve the trickiest 3x3 Toroidals.  Furthermore, it doesn’t work very well on 4x4 Toroidals or higher.  The enhanced IDS program was unable to solve any of 100 4x4 Toroidals within a time limit of 60 seconds.  The problem is that Enhanced IDS gives equal time to all the non-useless sequences.  It would be better if we could spend more time on sequences that seem promising, and less time on sequences that don't seem to be helpful.

To do better than IDS, we’re going to try a technique called heuristics.  In this context, a heuristic will attempt to guide a search towards a solution by looking at how promising the sequences are.  More promising sequences are tried first.  When we avoid useless sequences, and try to prioritize the promising sequences, we will conquer 3x3 Toroidals quite admirably.  However, even that won’t be enough for 5x5 Toroidals, or larger.  We’re going to need even more tricks to tackle those.


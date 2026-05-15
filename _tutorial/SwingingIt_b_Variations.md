--- 
layout: post 
title:  "Beyond TLC: Variations of the Swing" 
date:   2023-08-16
--- 
<style> 
img {15px;}
</style>

In the previous article, I described [the Swing algorithm](/tutorial/SwingingIt.html).
It's an application of the basic moves I described in [the novice TLC method.](/tutorial/NoviceMethod_b_Details.html) A Swing is a sequence of 4 moves with the following properties:
1. The row and column moves alternate. A valid Swing can start with a row move or a column move. 
2. Only one row is moved, and only one column.  No other row or column is moved.
3. The column moves alternate direction: first `D`, then `U`.  Likewise, the row moves alternate direction: first `R`, then `L`.
In this article, I will describe variations of this sequences of moves.

**Swing Variations.** 
Each of the following is a swing. 
  * `D3 R2 U3 L2`
  * `U1 L1 D1 R1` 
  * `R2 U3 L2 D3` 
  * `L1 U4 R1 D4` (for this one the Toroidal must have 4 columns
or more)

The following are not swings:
  * `D3 U3 R2 L2` (row and column moves are not alternating) 
  * `U1 L1 D2 R1` (the column moves affect different columns)


**Repeating a swing pattern.** 
In an example from the previous article, we performed a
swing on a Toroidal tiles labelled A, B, C swapped positions, in a kind of
clockwise rotation. 

![A Toroidal with tiles labelled A,B,C](/TImages/Stars3x3_ABC.png) 
&rarr;  `L2 U3 R2 D3`  &rarr;
![A Toroidal after a Swing move](/TImages/Stars3x3_CAB.png)

If you apply the same swing again, the same three
tiles will rotate clockwise again.  Finally, after performing the swing
a third time, the three tiles will be in their original position.  
This is a bit hard to visualize, so I’ve created a demonstration using a
5x5 Toroidal, which simply applies the swing `U3 L2 D3 R2` 3 times in
succession.  It uses my "Replay Toroidal" page, which I created to help
me visualize the effects of long sequences of moves.  The Replay page
looks like the Toroidal page, but you can’t move the tiles yourself. 
You can only play or step through a sequence of moves.  You can adjust
the speed of each move with the slider. The tiles that rotate are
labelled A, B, and C, and after every swing, they've swapped positions
in the clockwise direction.

[Click here to see the demonstration]({{ site.toroidalreplay_url | append:  "?mode=Swinging.json"}}){:target="_blank"}.

**Long Swings.** All of the examples presented have swings that move a
row or column exactly one step in some direction.  But as long as we're
careful, we can also move tiles farther.  For example: 
  * `D3 D3 R2 U3 U3 L2` 
  * `U1 L1 L1 D1 R1 R1` 
  * `R2 R2 U3 U3 L2 L2 D3 D3`

These are sequences where the rows and columns are not strictly
alternating.  The first example has column 3 moving down twice before a
row move.  That's okay, as long as the column moves back twice after any
row move.   My emphasis on the fact that the short swings always
alternate row and column moves was intended to focus attention on the
key concepts.  After these concepts are acquired, they can be relaxed.

And sometimes you might move more than one row or column during a swing. 
For example, here's a double swing:  `D2 D3 R2 U2 U3 L2`.  It moves two
different columns before the row move.  An expert might find it
effective to use a double swing in very special cases where two tiles
can be moved into place at the same time.

**Abbreviated swings.** If you are familiar with the TLC method that I
posted earlier, you may recall that I described the first move as
optional.  If the tile you want to bring into the TLC is in the way, the
first move is to push it out of the way.  If the tile is already out of
the way, you don't need to move it first.  This may suggest that the
swing can be reduced to a 3 move sequence, with an optional additional
move to use before the three moves.

However, the key aspect of the swing is that it preserves the position
of almost all of the tiles (except for three).  Any partial swing will
push a few more tiles out of their former position.  Now, if those tiles
are all in the gutter, and you don't care about the gutter yet, it's
okay to do a partial swing.  But if you're not using a dedicated gutter,
then you'll probably want to do a full swing, to preserve the position
of more tiles.

This is not a problem.  Any partial swing with 3 moves can be completed
with a single move. For example, suppose you used `D3 R2 U3` to put a
tile into position. To complete the swing, just add the row move `L2`.
Here are more examples: 
  * Partial swing: `D3 R2 U3`.  Completion: `L2` 
  * Partial swing: `U1 L1 D1`.  Completion: `R1` 
  * Partial swing: `R2 U3 L2`.  Completion: `D3` 
  * Partial swing: `L1 U4 R1`.  Completion: `D4`

It also happens to be true that any partial swing of 2 moves can be
completed, and there is exactly one way to complete it: 
  * Partial swing: `D3 R2`.  Completion: `U3 L2` 
  * Partial swing: `U1 L1`.  Completion: `D1 R1` 
  * Partial swing: `R2 U3`.  Completion: `L2 D3` 
  * Partial swing: `L1 U4`.  Completion: `R1 D4`

The third and fourth moves in a swing must use the same row and column
as the first two moves, and must move in the opposite direction.


## Looking forward
3. Toroidals with at least one even dimension (e.g., 4x3) can lead to a configuration that is tricky to solve.  I describe the problem, and its solution [on this page.](/tutorial/1Swap.html)
3. You can go back to the main Toroidal page, and explore [my work on computer algorithms to solve Toroidal.](/index.html)


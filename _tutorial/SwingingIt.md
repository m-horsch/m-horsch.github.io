--- 
layout: post 
title:  "Beyond TLC: Introducing the Swing" 
date:   2023-08-09
--- 
<style> 
img { float:left; margin-right:15px; }
br { clear: left; }
blockquote { color: #111; letter-spacing: 0px; font-size: 16px; }
</style>

In a previous article, I described [a simple approach called TLC that
novices can use to solve Toroidal puzzles](/tutorial/NoviceMethod_b_Details.html).
It always works, and never gets into a
dead-end, though it might result in a high number of moves. 
The method uses a dedicated *gutter,* and tiles are moved to and from the gutter.

A more advanced perspective is that you don’t
actually need a dedicated gutter; any tile you haven’t placed into
position yet can be a “gutter.”  Using this idea, you can substantially 
reduce the number of moves you need to solve Toroidal puzzles. 

In this article, I will describe the basic algorithm, and demonstrate its effect on a 3x3 Toroidal.  

## One basic algorithm to solve Toroidal: Swing
As I mentioned in other articles, Toroidal is basically a 2-dimensional Rubik’s
cube.  To solve a Rubik’s Cube, there are multiple *algorithms* that move
the cubies around.  There are beginner’s and expert’s algorithms for
Rubik's Cube, but they all have a common goal: put as many cubies into
place as possible, using some clever tricks to avoid moving most of the
cubies already in place.

It turns out that for Toroidal, apart from shifting a row or a column to move a tile, there are just two algorithms we need. One of these is used in a very specific situation; it will be fully described in a different article (pointers at the end). This article will describe the other algorithm, which can be applied far more frequently.

For some reason that I
can no longer remember, I call this basic algorithm a *swing*. The swing
has numerous variations, and we’ll touch briefly on those after seeing
one of them in detail.

Every sequence of moves described for TLC was a swing in one variation
or another.  Here’s one of them, from a previous article:

>    **Moving any tile from the gutter to the TLC using a TLC row
>    move.** 
>    Let's say there's a tile in the gutter column that we want
>    to move to the TLC.  We'll call this tile T. 
>    1.    If T is in the
>    gutter column, you’ll use a row TLC move to place it into the TLC.
>    2. Before you make the TLC row move, make sure that T is not in
>    the row you’ll be moving.  If it is, make a gutter column move to
>    get it out of the way.  Up or down doesn't matter. 
>    3.  With T out
>    of the way, temporarily move the TLC with a row move, so that the
>    place where T belongs is in the gutter. 
>    4. Shift T into place
>    with a column gutter move. 
>    5.  Immediately push the row back with
>    a TLC move, with T in place. 

The above description uses plain language, which can be a bit clumsy.
But with a few bits of terminology, it can be made clearer and more
concise.

**Terminology.**  We need to give names to the basic moves.  A row can
be pushed left (`L`) or right (`R`).  A column can be pushed up (U) or down
(D).  We also need to name the rows and columns.  We’ll count them
starting from the top left: rows `1`, `2`, `3`, etc, and columns `1`, `2`, `3`, etc.
 To completely specify a move we give the direction and a number.  So,
`L2` means “push the second row left one position”, and `U1` means “push
the first column up one position.”  If you need to move a tile two
positions, use two moves, e.g. `L2 L2`.

**Example.** We’ll revisit example we saw in a [previous article](/tutorial/NoviceMethod_b_Details.html).

![A Toroidal with the TLC almost complete](/TImages/Stars3x3_Move_T.png)
In the graphic on the left, the tiles that belong in the TLC are
highlighted, and the tiles that belong in the gutter are darkened.  The
tile T is in the gutter, but it should be in the TLC.  It's on the row
that we'll be moving, so T is actually in the way. <br/>

![Moving the tile T out of the way](/TImages/Stars3x3_Moved_T.png) We
have moved T out of the way by moving column 3 down: `D3`. <br/>

![Displacing the TLC temporarily](/TImages/Stars3x3_Displace_TLC.png)
With T out of the way, we have moved row 2 to the right: `R2`. <br/>

![Pushing T into position in the row](/TImages/Stars3x3_Position_T.png)
We've pushed T into position by moving column 3 up: `U3`. <br/>

![Repairing the TLC](/TImages/Stars3x3_Repair_TLC.png) Finally, we
pushed the middle row back into place: `L2`. <br/>

The whole sequence can be described very concisely by writing `D3 R2 U3 L2`. 
There are some important observations to make about this sequence.
1. The row and column moves alternate: column, row, column, row.  It is also valid to start with a row move, as long as the moves alternate between row and column moves.
2. Only one row is moved (here, row 2), and only one column (here, column 3).  No other row or column is moved.
3. The column moves alternate direction: first `D`, then `U`.  Likewise, the row moves alternate direction: first `R`, then `L`.

This specific combination of moves is one example of swing algorithm, and there are many variations. 

## The effect of the swing
Every swing variation has the effect of moving exactly three tiles, leaving all other tiles where they were to begin with. 
While you’re performing the 4 moves of a swing, several tiles move around, but except
for the 3 that change places, all the other tiles that move are returned to
their previous position when the swing is complete.
To visualize this effect, let's look at the swing `L2 U3 R2 D3` on a
familiar 3x3 Toroidal.  

![A Toroidal with tiles labelled A,B,C](/TImages/Stars3x3_ABC.png)
Here’s a starting configuration for the visualization.  There are three
tiles labelled A, B, C.  These are the ones that will swap places. <br/>

![A Toroidal after L2](/TImages/Stars3x3_B_AC.png) Here it is after
applying `L2`.  The unlabelled tile on row 2 has moved, along with A and
B.  This is temporary. <br/>

![A Toroidal after L2](/TImages/Stars3x3_A_B_C.png) After `U3`.  Another
unlabelled tile moves along with the column. <br/>

![A Toroidal after L2](/TImages/Stars3x3_A_CB.png) After `R2`.  Notice
that the unlabelled tile on row 2 has moved back to its original
position. <br/>

![A Toroidal after L2](/TImages/Stars3x3_CAB.png) After `D3`.  The swing
is complete.  The unlabelled tile on column 3 moved back to its original
position, and only the tiles labelled A, B, C have changed position.
<br/>

The tiles labelled A,B,C have swapped positions, in a kind of clockwise
rotation. Naturally, we want to use the swing to move tiles into their
correct position, not, as the example does, move them out of position.
But any sequence can be reversed, by reversing the order, and flipping
directions. So to reverse `L2 U3 R2 D3`, we can use the sequence `U3 L2
D3 R2`. This sequence is also a swing, and moves the three tiles
counter-clockwise.

## Summary
The swing is the essential technique, and in general, a swing is
required to solve any Toroidal.  There are specific Toroidals that do
not require the use of a swing in any of its variations, but those are
always boring puzzles.

## Looking forward
1. I describe [variations on the Swing algorithm here.](/tutorial/SwingingIt_b_Variations/html)
3. Toroidals with at least one even dimension (e.g., 4x3) can lead to a configuration that is tricky to solve.  I describe the problem, and its solution [on this page.](/tutorial/1Swap.html)
3. You can go back to the main Toroidal page, and explore [my work on computer algorithms to solve Toroidal.](/index.html)

---
layout: post
title:  "How to Solve It (2)"
date:   2023-07-26 
categories:  Playing
---
<style>
img
{
    float:left;
    margin-right:15px;
}
br
{
    clear: left;
}

blockquote 
{
    color: #111;
    letter-spacing: 0px;
    font-size: 16px;
}
</style>

In a previous posting, I described [a simple approach called TLC that novices can use to solve Toroidal puzzles]({% post_url 2023-07-19-NoviceMethod %}).  It always works, and never gets into a dead-end, though it might result in a high number of moves.   If you want to reduce your move count, you don't have to stick to the TLC too rigidly.  Once you get the hang of moving tiles around, you don’t actually need a dedicated gutter; any tile you haven’t placed into position yet is a “gutter.”  This posting will explain why this idea works.

## One basic algorithm to solve Toroidal: Swing
As I mentioned before, a Toroidal is basically a 2-dimensional Rubik’s cube.  To solve a Rubik’s Cube, there are multiple algorithms that move the cubies around.  There are beginner’s and expert’s algorithms for Rubik's Cube, but they all have a common goal.  Put as many cubies into place as possible, using some clever tricks to avoid moving most of the cubies already in place.  

It turns out that for Toroidal, we can rely on one basic algorithm, consisting of a combination of alternating moves.
For some reason that I can no longer remember, I call this basic algorithm a *swing*.
The swing has numerous variations, and we’ll touch briefly on those after seeing one of them in detail.  

Every sequence of moves described for TLC was a swing in one variation or another.  Here’s one of them, from the other posting:

>    **Moving any tile from the gutter to the TLC using a TLC row move.**
>    Let's say there's a tile in the gutter column that we want to move to the TLC.  We'll call this tile T.
>    1.	If T is in the gutter column, you’ll use a row TLC move to place it into the TLC.  
>    2.	Before you make the TLC row move, make sure that T is not in the row you’ll be moving.  If it is, make a gutter column move to get it out of the way.  Up or down doesn't matter.
>    3.	With T out of the way, temporarily move the TLC with a row move, so that the place where T belongs is in the gutter.
>    4.	Shift T into place with a column gutter move.  
>    5.	Immediately push the row back with a TLC move, with T in place. 

The above description uses plain language, which can be a bit clumsy. But with a few bits of terminology, it can be made clearer and more concise. 

**Terminology.**  We need to give names to the basic moves.  A row can be pushed left (L) or right (R).  A column can be pushed up (U) or down (D).  We also need to name the rows and columns.  We’ll count them starting from the top left: rows 1, 2, 3, etc, and columns 1, 2, 3, etc.  To completely specify a move we give the direction and a number.  So, `L2` means “push the second row left one position”, and `U1` means “push the first column up one position.”  If you need to move a tile two positions, use two moves, e.g. `L2 L2`.  

<!-- Second, to identify tile locations, we’ll use two numbers in parentheses.  The tile in row 1 and column 1 is at location (1,1).  The tile at row 2 and column 4 is (2,4).  We always write (*row*, *col*). -->

**Example.** We’ll revisit example we saw in that [previous posting]({% post_url 2023-07-19-NoviceMethod %}).  
 
![A Toroidal with the TLC almost complete](/TImages/Stars3x3_Move_T.png)
In the graphic on the left, the tiles that belong in the TLC are highlighted, and the tiles that belong in the gutter are darkened.  The tile T is in the gutter, but it should be in the TLC.  It's on the row that we'll be moving, so T is actually in the way. 
<br/>

![Moving the tile T out of the way](/TImages/Stars3x3_Moved_T.png) 
We have moved T out of the way by moving column 3 down: `D3`.  
<br/>

![Displacing the TLC temporarily](/TImages/Stars3x3_Displace_TLC.png)
With T out of the way, we have moved row 2 to the right: `R2`.
<br/>

![Pushing T into position in the row](/TImages/Stars3x3_Position_T.png)
We've pushed T into position by moving column 3 up: `U3`.
<br/>

![Repairing the TLC](/TImages/Stars3x3_Repair_TLC.png) 
Finally, we pushed the middle row back into place: `L2`.
<br/>

The whole sequence can be described very concisely by writing `D3 R2 U3 L2`.
The first and third moves in this sequence used column 3 (and no other column).  The second and fourth moves used row 2 (and no other rows).  The row and column moves alternate. This combination of moves is the swing algorithm, and there are many variations. 


**Swing Variations.**   When you need to move a tile into place, you will use a swing, almost every time.  To perform a swing, you must move one row, and one column.  In the TLC method, you may need to start with a row move, for example, when the tile you need is in the gutter column gutter.  Sometimes you'll need to start the swing with a column move, if the tile you need is in the gutter row.  A swing is a swing no matter whether you start with a row move or a column move.  To be a swing, you must move the row one way, and then the other way, and the column one way, and then the other.  The swing alternates row and column moves.

Each of the following is a swing.  
* `D3 R2 U3 L2`
* `U1 L1 D1 R1`
* `R2 U3 L2 D3`
* `L1 U4 R1 D4` (for this one the Toroidal must have 4 columns or more)

The following are not swings:
* `D3 U3 R2 L2` (row and column moves are not alternating)
* `U1 L1 D2 R1` (the column moves affect different columns)

## The effect of the swing
Every swing variation has the effect of swapping exactly three tiles.  While you’re performing a swing, multiple tiles move around, but except for the 3 that swap places, all the tiles that move are returned to their previous position when the swing is complete.

To visualize this effect, let's look at the swing `L2 U3 R2 D3` on a familiar 3x3 Toroidal.  We know it's a swing because the sequence alternates row and column moves; also, one particular row (row 2) moves back and forth, and one particular column (column 3) gives up and down.  It's the same sequence that I showed in the TLC posting.

![A Toroidal with tiles labelled A,B,C](/TImages/Stars3x3_ABC.png) 
Here’s a starting configuration for the visualization.  There are three tiles labelled A, B, C.  These are the ones that will swap places.  
<br/>

![A Toroidal after L2](/TImages/Stars3x3_B_AC.png) 
Here it is after applying `L2`.  The unlabelled tile on row 2 has moved, along with A and B.  This is temporary.
<br/>

![A Toroidal after L2](/TImages/Stars3x3_A_B_C.png) 
After `U3`.  Another unlabelled tile moves along with the column.
<br/>

![A Toroidal after L2](/TImages/Stars3x3_A_CB.png) 
After `R2`.  Notice that the unlabelled tile on row 2 has moved back to its original position.
<br/>

![A Toroidal after L2](/TImages/Stars3x3_CAB.png) 
After `D3`.  The swing is complete.  The unlabelled tile on column 3 moved back to its original position, and only the tiles labelled A, B, C have changed position.
<br/>

The tiles labelled A,B,C have swapped positions, in a kind of clockwise rotation.
Naturally, we want to use the swing to move tiles into their correct position, not, as the example does, move them out of position.
But any sequence can be reversed, by reversing the order, and flipping directions.
So to reverse `L2 U3 R2 D3`, we can use the sequence `U3 L2 D3 R2`.
This sequence is also a swing, and moves the three tiles counter-clockwise.  


**Repeating a swing pattern.**
In the above example, we performed a swing, and tiles labelled A, B, C swapped positions, in a kind of clockwise rotation.  If you apply the same swing again, the same three tiles will rotate clockwise again.  Finally, after performing the swing a third time, the three tiles will be in their original order.  And none of the other tiles move at all!

This is a bit hard to visualize, so I’ve created a demonstration using a 5x5 Toroidal, which simply applies the swing `U3 L2 D3 R2` 3 times in succession.  It uses my "Replay Toroidal" page, which I created to help me visualize the effects of long sequences of moves.  The Replay page looks like the Toroidal page, but you can’t move the tiles yourself.  You can only play or step through a sequence of moves.  You can adjust the speed of each move with the slider.
The tiles that rotate are labelled A, B, and C, and after every swing, they've swapped positions in the clockwise direction.

[Click here to see the demonstration]({{ site.toroidalreplay_url | append:  "?mode=Swinging.json"}}){:target="_blank"}.

**Long Swings.** All of the examples presented have swings that move a row or column exactly one step in any direction.  But as long as we're careful, we can also move tiles farther.  For example:
* `D3 D3 R2 U3 U3 L2`
* `U1 L1 L1 D1 R1 R1`
* `R2 R2 U3 U3 L2 L2 D3 D3`

These are sequences where the rows and columns are not strictly alternating.  The first example has column 3 moving down twice before a row move.  That's okay, as long as the column moves back twice after any row move.   My emphasis on the fact that the short swings always alternate row and column moves was intended to focus attention on the key concepts.  After these concepts are acquired, they can be relaxed.  

And sometimes you might move more than one row or column during a swing.  For example, here's a double swing:  `D2 D3 R2 U2 U3 L2`.  It moves two different columns before the row move.  An expert might find it effective to use a double swing in very special cases where two tiles can be moved into place at the same time.  

**Abbreviated swings.**
If you are familiar with the TLC method that I posted earlier, you may recall that I described the first move as optional.  If the tile you want to bring into the TLC is in the way, the first move is to push it out of the way.  If the tile is already out of the way, you don't need to move it first.  This may suggest that the swing can be reduced to a 3 move sequence, with an optional additional move to use before the three moves.

However, the key aspect of the swing is that it preserves the position of almost all of the tiles (except for three).  Any partial swing will push a few more tiles out of their former position.  Now, if those tiles are all in the gutter, and you don't care about the gutter yet, it's okay to do a partial swing.  But if you're not using a dedicated gutter, then you'll probably want to do a full swing, to preserve the position of more tiles.

This is not a problem.  Any partial swing with 3 moves can be completed with a single move.
For example, suppose you used `D3 R2 U3` to put a tile into position.
To complete the swing, just add the row move `L2`.
Here are more examples:
* Partial swing: `D3 R2 U3`.  Completion: `L2`
* Partial swing: `U1 L1 D1`.  Completion: `R1`
* Partial swing: `R2 U3 L2`.  Completion: `D3`
* Partial swing: `L1 U4 R1`.  Completion: `D4`

It also happens to be true that any partial swing of 2 moves can be completed, and there is exactly one way to complete it:
* Partial swing: `D3 R2`.  Completion: `U3 L2`
* Partial swing: `U1 L1`.  Completion: `D1 R1`
* Partial swing: `R2 U3`.  Completion: `L2 D3`
* Partial swing: `L1 U4`.  Completion: `R1 D4`

The third and fourth moves in a swing must use the same row and column as the first two moves, and must move in the opposite direction.  

## Summary
The swing is the essential technique, and in general, a swing is required to solve any Toroidal.  There are specific Toroidals that do not require the use of a swing in any of its variations, but those are always boring puzzles.  


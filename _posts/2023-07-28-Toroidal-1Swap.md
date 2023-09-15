---
layout: post
title:  "The One-Swap Finish Problem"
date:   2023-07-28 
categories:  Playing
---
<style>
blockquote 
{
    color: #111;
    letter-spacing: 0px;
    font-size: 16px;
}
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>

**Description.**  All Toroidal puzzles are solvable, by virtue of the way they are constructed.
I take a tiled image, and apply random, legal moves.
The legal moves are constrained so that a random move is never the opposite of the previous move.
For example, if the previous random move was `L2`, if the next random move chosen is `R2`, I throw it away and choose another.

However, there is a situation that can arise, when one (or both) of the dimensions is even.  I wrote this in [the introduction]({% post_url 2023-07-12-Toroidal-an-Introduction %}):

> ![Image-style puzzle with one pair of tiles swapped](/TImages/Puzzle4x41S.png)
>  **Even Dimensions.**  Puzzles that have *at least* one even dimension (e.g., 3x4) can arrive at a  configuration that feels like a dead-end.
> To be precise, in a puzzle with at least one even dimension, it is common to have the puzzle nearly completely solved, with exactly one pair of tiles out of place, as shown in the image to the left.
> The two tiles highlighted with red outline are side-by-side, and out of place; the rest of the puzzle is solved.
> If you could just lift the tiles out of the grid and swap them, they would be in the right place.
> This configuration is still solvable, but the number of moves needed to swap the tiles is unexpectedly long.
> This situation only occurs when there is at least one even dimension, and all tiles are unique.   I will say more about this in a separate posting. 


I've been calling this situation the *1-swap finish problem* (1SF), since the Toroidal could be solved by making a single swap, and leaving every other tile unmoved.  However, as I mentioned earlier, one of the basic moves, which I called *swing,* swaps three tiles.
We can apply a swing directly to these two tiles, and by doing so we can put one of these two tiles into place, but the other will form a 1SF situation with the third tile moved by swing.

Note: If you see 1SF in a 3x3 grid (or any grid where both dimensions are odd), it cannot be solved.  This situation cannot arise in Toroidal, because of the way puzzles are constructed.

**Solution.**  Let's begin by naming the tiles that need to be swapped.  We'll call them A and B, and in the 1SF situation in the example above, B is to the left of A.  To correct the 1SF, we can use the following sequence:

    R4 D1 R4 U1 R4 R4 D1 R4 U1

In words:  Start by moving tile A out of the way with `R4 D1 R4 U1`.   Then do `R4` to prepare tile B, and then `R4 D1 R4 U1` to position A beside B.  

1. [Here's a demonstration]({{ site.toroidalreplay_url | append:  "?mode=1SF.json" }}){:target="_blank"}.
2. [Here's the playable Toroidal to give it a try yourself]({{ site.toroidal_url | append:  "?mode=training/1SF.json" }}){:target="_blank"}.

I broke the longer sequence into 3 smaller sub-sequences to explain it.  There is a sequence of 4 moves that appears twice: `R4 D1 R4 U1`.  This contains a partial or incomplete swing.  I am not sure if this is an important observation.  If the swings were complete, I would be more confident in saying "swing is all you need."  On the other hand, it might be better to treat the sequence `R4 D1 R4 U1` as a distinct kind of sequence, with one axis moving back and forth and the other moving one direction only.  

**Chopping.**  Let's give a name to this new kind of sequence: a *chop*.  So named because it reminds me of chopping carrots: one row or column moves in one direction, and the other moves back and forth.  We have seen two chops so far:
1. Alternating first: `D1 R4 U1 R4`
2. Alternating second: `R4 D1 R4 U1`

**Alternate Solution.**  I discovered the above sequence using my Python Toroidal solver.  However, just by working with Toroidal puzzles, I discovered a different solution.  For the record, I will describe it as well.  

To correct the 1SF in the example above, we can use the following sequence: 

    U2 L4 U2 R4 U2 L4 U2 R4 U2

In words: start by pushing tile B up.   Then push tile A left, beneath B.  Push the column with the two tiles up, alternating left and right moves on the bottom row.
This alternate solution repeats the chop `U2 L4 U2 R4` twice, followed by a single move `U2`.

**Complications.**  My demonstration was applied to a 4x4 Toroidal, where the two swappable tiles are on the same row.  It is possible that the two swappable tiles are adjacent, on the same column.  If so, the direct application of either of the above solutions needs to be modified.  Any move sequence can be rotated, if you're careful.  The 1SF problem can arise when the tiles that should be swapped are not actually side-by-side.  You can apply the same strategies, by slightly modifying the distance the tiles have to move.  It's often simpler to apply the swing until the tiles to be swapped are adjacent.

The 1SF problem can occur in a problem with one even and one odd dimension, e.g., 3x4.  Either of the solutions can be used, but it will depend on context.  For the first sequence, the two tiles will have to be on the odd dimension axis.  You can also use the alternate sequence, but the swappable tiles must appear side-by-side on the even dimension axis.  If the swappable tiles are not on the preferred axis, neither sequence will work directly.  In this case, you can use a swing to change the tiles to be swapped.

I limit my Toroidal puzzles to 5x5, so the only even dimension you see here is 4 rows and/or 4 columns.  But in principle, the 1SF problem can arise on even dimensions higher than 4.  In that case, after the sequence of moves, you'll have to push the row a couple of positions to the right to finish up.  The 1SF problem does occur on a 2x2, but it can be solved with one move.


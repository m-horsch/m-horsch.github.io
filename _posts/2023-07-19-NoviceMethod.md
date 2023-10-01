---
layout: post
title:  "How to Solve it (1)"
date:   2023-07-19 
categories:  Playing
---
<style>
img
{
    padding: 15px;
}
</style>
## Brief Introduction 

If you’re new to Toroidal, [you can read a brief introduction]({% post_url 2023-07-12-an-Introduction %}).
In summary, the challenge is to rearrange a rectangular grid of image tiles, to produce an image of some kind.  You can slide tiles left, right, up, or down.  The whole row (or column) moves, and any tile pushed towards the edge of the grid reappears on the opposite side.  Every Toroidal is solvable.  There are never any dead ends; only detours.   Every Toroidal has multiple solutions, some longer, some shorter.  

## Beginner’s Strategy: TLC
The basic beginner's strategy is to solve the Top Left Corner (TLC) first, and then solve the last row and column.  For a 3x3 Toroidal, the TLC is a 2x2 square; in general, it's every tile except the last row and column.  It can be any corner, in fact, but TLC has a certain ring to it.

In the diagram below, we have a 3x3 Toroidal scrambled on the left, the same 3x3 Toroidal with the 2x2 TLC solved in the middle, and then the whole Toroidal solved on the right.  While the diagram has these stages side by side, it takes several moves (not shown) to progress from one to the next.

![A scrambled 3x3 Toroidal](/TImages/Stars3x3_scrambled.png) &rarr;
![The top left corner solved](/TImages/Stars3x3_TLC_done.png) &rarr;
![The completely solved Toroidal](/TImages/Stars3x3.png)

More details are given below, but the basic idea is to solve the TLC using row 3 and column 3 as “gutters” to move tiles around.  While you are positioning the TLC tiles, you don’t care about the tiles in the gutter.  You can move the gutter freely, without affecting the TLC. 

![The last row and column are the gutters](/TImages/Stars3x3_gutter.png)

Once the TLC is done, you can solve the gutter by moving only the gutter row and gutter column.  A move that *only* affects the gutter will be called a gutter move.  Any move that affects the TLC will be called a TLC move.  TLC moves displace tiles found in the TLC and in the gutter, but we will use TLC moves carefully so that they do not ruin what you've already accomplished.  

You can always move a tile from the TLC to the gutter, and from the gutter to the TLC, without ruining the work you’ve already done in the TLC.  The key is to make a TLC move, then a gutter move, then a TLC move again.  It’s always one of the following 2 possibilities:
1.	TLC row move, gutter column move, TLC row move back.
2.	TLC column move, gutter row move, TLC column back.

Let’s look at it in more detail. I will focus on TLC row moves first; that is, option 1 above.

### Moving any tile from the gutter column to the TLC using a TLC row move
Let's say there's a tile in the gutter column that we want to move to the TLC.  We'll call this tile T.
1.	Since T is in the gutter column, you’ll use a row TLC move to place it into the TLC.  
3.	Before you make the TLC row move, make sure that T is not in the row you’ll be moving.  If it is, make a gutter column move to get it out of the way.  Up or down doesn't matter.
5.	With T out of the way, temporarily move the TLC with a row move, so that the place where T belongs is in the gutter.
6.	Shift T into place with a column gutter move.  
7.	Immediately push the row back with a TLC move, with T in place.  

**Example.**
We start with the TLC is almost complete, with only the tile marked T to put into place.  In the graphic below, the tiles that belong in the TLC are highlighted, and the tiles that belong in the gutter are darkened.
 
![A Toroidal with the TLC almost complete](/TImages/Stars3x3_Move_T.png) 

The tile T would be in the correct position if it were one position to the left, on the same row, but we can't simply push it there.  So we move it out of the way first.  It's in the gutter column, which can move freely up or down.  In this case I moved it down.

![Moving the tile T out of the way](/TImages/Stars3x3_Moved_T.png) 

With T out of the way, we can make a TLC move, pushing the middle row to the right.  This puts T side-by-side with the tile it is replacing.

![Displacing the TLC temporarily](/TImages/Stars3x3_Displace_TLC.png) 

Now we push T into position by moving the gutter column up.  

![Pushing T into position in the row](/TImages/Stars3x3_Position_T.png) 

Finally, we make a TLC move, pushing the middle row back into place.  

![Repairing the TLC](/TImages/Stars3x3_Repair_TLC.png) 

Now if you look closely at the last Toroidal image here, you can see that the gutter can be solved with one move!  This doesn't always happen so nicely.

**You try it.** Click on [the link to practice this move]({{ site.toroidal_url | append: "?mode=training/TLC_practice.json"}}){:target="_blank"}.  The example is exactly the same as the one above, except I substituted a plain blue T-block for one of the image tiles.  If you make a mistake, use the Undo button, or the Reset button.  In the description above, I moved the tile T out of the way by moving it down.  Try moving it up, and see how it changes things!

### Moving a tile from the gutter row to the TLC using a TLC column move
This is exactly the same as the above description, except rows and columns are exchanged.  Again, T is the name for the tile we want to move; this time, from a gutter row to the TLC.

1.	If T is in the gutter row, you’ll use a column TLC move to place it into the TLC.  
2.	Before you make the TLC column move, make sure that T is not in the column you’ll be moving.  If it is, make a gutter row move to get it out of the way.  Left or right doesn't matter.
3.	With T out of the way, temporarily move the TLC with a column move, so that the place where T belongs is in the gutter.
4.	Shift T into place with a row gutter move.  
5.	Immediately push the column back with a TLC move, with T in place. 

From here on, I won't give both row and column descriptions, but I'll use the clumsier (and slightly confusing) phrase "row (or column)."  

**You try it.** Click on [the link to practice this move]({{ site.toroidal_url | append: "?mode=training/TLC_practiceB.json"}}){:target="_blank"}.  It's the same image, but this time the T block is on the gutter row.  And it's not in the way, so step 2 above is not needed!

### Moving any tile from the TLC to the gutter  
Occasionally you may need to move a tile to the gutter.  Your first thought should be to replace the tile you want to move with the correct tile, using one of the sequences we saw earlier.  But sometimes, that's not convenient, so getting rid of the unwanted tile is a first step.  This can be done with the same sequence of moves as we described above.  The only difference is that we don't care which tile comes into the TLC.  Again, T is the name we're giving to the tile we want to move.
1.	Push the row (or column) with T on it, so that T is in the gutter.  
2.	Push the gutter column (or row) one position.  
3.	Immediately push the row (or column) back to its original position.  

### If a tile is in the TLC, but in the wrong place:
1.	Move the tile from the TLC to the gutter.
2.	Move the tile from the gutter to the right place in the TLC.

### Solving the gutter
If the TLC is complete, all the tiles for the gutter are already in the gutter.  The only task is to move them into place.  We’ll solve the gutter row using the same moves as we used to solve the TLC, except that we will only use gutter moves.  To solve the gutter, we will not make any moves that affect the TLC.   
1. If you move a tile into the gutter row, it will be from the gutter column.          
2. Likewise, if you move a tile to the gutter column, it will be from the gutter row.  
    
You may have to move a tile from one position in the gutter row (or gutter column) to another position in the gutter row (or gutter column); do this using the gutter column (or gutter row).  Remember, to solve the gutter row and column, only move the gutter row and column.

**You try it.**  Here are two examples of the 3x3 Toroidal with scrambled gutters.  It turns out that on a 3x3, the gutters are almost always easy to solve.  
1. [Gutter practice 1]({{ site.toroidal_url | append:  "?mode=training/Gutter_practice.json" }}){:target="_blank"}.  
1. [Gutter practice 2]({{ site.toroidal_url | append:  "?mode=training/Gutter_practiceB.json" }}){:target="_blank"}.  


## Larger Toroidals

This strategy works for any size Toroidal.  For example, to solve 5x5, the TLC is the 4x4 corner of the Toroidal, and row 5 and column 5 are gutters.  At the very end, once the top 4x4 TLC is completed, you can solve the gutter, again, only by moving tiles in the gutter.  For larger Toroidals, solving the gutter takes longer.

To solve the 4x4 TLC, you can work hierarchically, or row-by-row.  These are discussed next.

### Hierarchical Strategy
If you are hierarchically minded, you can solve a large Toroidal by solving the top 2x2 corner first, using all the remaining rows and columns as gutters.  Then with the 2x2 TLC done, extend it by solving the top left 3x3.  This means moving tiles to the 3rd row and column beside the 2x2 corner you already completed.  After that, work on the top left 4x4 corner.  Once the 4x4 TLC is done, you can solve the gutter.

In the sequence of graphics below, the 5x5 Toroidal is progressively solved using the hierarchical method.  The solved portion of the Toroidal is greyed out a bit to highlight the progress.

![A scrambled 5x5 Toroidal](/TImages/5x5_HScrambled.png) &rarr;
![The top left 2x2 corner solved](/TImages/5x5_TLC2_Done.png) &rarr;
![The top left 3x3 corner solved](/TImages/5x5_TLC3_Done.png) &rarr;
![The top left 4x4 corner solved](/TImages/5x5_TLC_Done.png) &rarr;
![The completely solved 5x5 Toroidal](/TImages/5x5_Solved.png)

### Row-by-row Strategy
Another useful strategy for large Toroidals is to solve row by row from the top, using the remaining rows, and the last column as a gutter.  For example, in a 5x5 Toroidal, solve the top row first (except the 5th column which will still be part of the gutter), then the second row, also excluding the 5th column.  Once the 4x4 TLC is done, you can solve the gutter.

In the sequence of graphics below, the 5x5 Toroidal is progressively solved using the row-by-row method.  The solved portion of the Toroidal is greyed out a bit to highlight the progress.

![A scrambled 5x5 Toroidal](/TImages/5x5_RScrambled.png)  &rarr;
![The top left 2x2 corner solved](/TImages/5x5_1Rows.png) &rarr;
![The top left 3x3 corner solved](/TImages/5x5_2Rows.png) &rarr;
![The top left 4x4 corner solved](/TImages/5x5_3Rows.png) &rarr;
![The top left 4x4 corner solved](/TImages/5x5_4Rows.png) &rarr;
![The completely solved 5x5 Toroidal](/TImages/5x5_Solved.png)

## From Novice to Expert 
Part of the fun of Toroidal is to seek an even shorter solution than the one you found.  The novice method uses the same essential moves that an expert would use.  The only difference between the TLC method above, and an expert's strategy is that the TLC method has a dedicated gutter, a row and column where you don't care what happens, at least until the TLC is done.  An expert uses the same moves to push the tiles from *here* to *there*, instead of from *the gutter* to *the TLC*. Once you are comfortable with the basic moves, any tile that is not already in place can be used as a gutter.

 

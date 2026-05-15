---
layout: post
title:  "The TLC Method Described with Examples"
date:   2023-07-26 
---
<style>
img
{
    padding: 15px;
}
</style>
## Brief Introduction 

In a previous article, I gave [an overview of the TLC method](/tutorial/NoviceMethod_a_Overview.html).  This article gives just a little more detail, with examples.  I also provide precise written instructions that you can apply when solving a 3x3 Toroidal.

## Recap of the Beginner’s Strategy: TLC
The basic beginner's strategy is to solve the Top Left Corner (TLC) first, and then solve the last row and column, which I called the "gutter."  The diagram below shows the TLC (solved)), and the gutters (scrambled).

![Showing the top left corner, and the gutters](/TImages/Stars3x3_gutter.png)

While you are positioning the TLC tiles, you don’t care about the tiles in the gutter.  You can move the gutter freely, without affecting the TLC. Once the TLC is done, you can solve the gutter by moving only the gutter row and gutter column. 

This strategy works because you can always move a tile from the TLC to the gutter, and from the gutter to the TLC, without ruining the work you’ve already done in the TLC.  The key is to make a TLC move, then a gutter move, then a TLC move again.  It’s always one of the following 2 possibilities:
1.	One TLC row move, one gutter column move, one TLC row move back.
2.	One TLC column move, one gutter row move, one TLC column move back.

Let’s look at it in more detail. I will focus on TLC row moves first; that is, option 1 above.

### Moving any tile from the gutter column to the TLC using a TLC row move
In the example below, there's a tile in the gutter column that we want to move to the TLC.  We'll call this tile T. The TLC is almost complete, with only the tile marked T to put into place.  The tiles that belong in the TLC are highlighted, and the tiles that belong in the gutter are darkened.
 
![A Toroidal with the TLC almost complete](/TImages/Stars3x3_Move_T.png) 

The tile T would be in the correct position if it were one position to the left, on the same row, but we can't simply push it there.  So we move it out of the way first.  It's in the gutter column, which can move freely up or down.  In this case I moved it out of the way by moving it down.

![Moving the tile T out of the way](/TImages/Stars3x3_Moved_T.png) 

With T out of the way, we can make a TLC move, pushing the middle row to the right.  This puts T side-by-side with the tile it is replacing.

![Displacing the TLC temporarily](/TImages/Stars3x3_Displace_TLC.png) 

Now we push T into position by moving the gutter column up.  

![Pushing T into position in the row](/TImages/Stars3x3_Position_T.png) 

Finally, we make a TLC move, pushing the middle row back into place.  

![Repairing the TLC](/TImages/Stars3x3_Repair_TLC.png) 


**You try it.** Click on [the link to practice this move]({{ site.toroidal_url | append: "?mode=training/TLC_practice.json"}}){:target="_blank"}.  The example is exactly the same as the one above, except I substituted a plain blue T-block for one of the image tiles.  If you make a mistake, use the Undo button, or the Reset button.  In the description above, I moved the tile T out of the way by moving it down.  Try moving it up, and see how it changes things!

The example above can be summarized by a sequence of steps, as follows:

1.	Since T is in the gutter column, use a  TLC row move to place it into the TLC.  
3.	Before you make the TLC row move, make sure that T is not in the row you’ll be moving.  If it is, make a gutter column move to get it out of the way.  Up or down doesn't matter.
5.	With T out of the way, make a TLC row move, so that the place where T belongs is in the gutter.
6.	Shift T into place with a column gutter move.  
7.	Immediately push the row back with a TLC move, with T in place.  


### Moving a tile from the gutter row to the TLC using a TLC column move
This is exactly the same as the above description, except rows and columns are exchanged.

**You try it.** Click on [the link to practice this move]({{ site.toroidal_url | append: "?mode=training/TLC_practiceB.json"}}){:target="_blank"}.  This time the T block is on the gutter row.  And it's not in the way, so we can save a step!

At the risk of boring the reader, I will describe the sequence of steps.  

Remember that T is the name for the tile we want to move; this time, from a gutter row to the TLC.
1.	If T is in the gutter row, use a column TLC move to place it into the TLC.  
2.	Before you make the TLC column move, make sure that T is not in the column you’ll be moving.  If it is, make a gutter row move to get it out of the way.  Left or right doesn't matter.
3.	With T out of the way, make a TLC column move, so that the place where T belongs is in the gutter.
4.	Shift T into place with a row gutter move.  
5.	Immediately push the column back with a TLC move, with T in place. 


### Moving any tile from the TLC to the gutter  
Occasionally you may need to move a tile to the gutter.  Your first thought should be to replace the tile you want to move with the correct tile, using one of the sequences we saw earlier.  But sometimes, that's not convenient, so getting rid of the unwanted tile is a first step.  This can be done with the same sequence of moves as described above.  The only difference is that we don't care which tile comes into the TLC.  This time, T is the name we're giving to the tile we want to move.
1.	Push the row (or column) with T on it, so that T is in the gutter.  
2.	Push the gutter column (or row) one position.  
3.	Immediately push the row (or column) back to its original position.  

### If a tile is in the TLC, but in the wrong place:
1.	Move the tile from the TLC to the gutter.
2.	Move the tile from the gutter to the right place in the TLC.

### Solving the gutter
If the TLC is complete, all the tiles for the gutter are already in the gutter.  The only task is to move them into place.  We’ll solve the gutter row using only gutter moves.  To solve the gutter, we will not make any moves that affect the TLC.   
1. If you move a tile into the gutter row, it will be from the gutter column.          
2. Likewise, if you move a tile to the gutter column, it will be from the gutter row.  
    
You may have to move a tile from one position in the gutter row (or gutter column) to another position in the gutter row (or gutter column); do this using the gutter column (or gutter row).  Remember, to solve the gutter row and column, only move the gutter row and column.

**You try it.**  Here are two examples of the 3x3 Toroidal with scrambled gutters.  It turns out that on a 3x3, the gutters are almost always easy to solve.  
1. [Gutter practice 1]({{ site.toroidal_url | append:  "?mode=training/Gutter_practice.json" }}){:target="_blank"}.  
1. [Gutter practice 2]({{ site.toroidal_url | append:  "?mode=training/Gutter_practiceB.json" }}){:target="_blank"}.  

## Looking forward
1. If I've given you enough direction already, you can leave this tutorial and try your hand at some of the [other Toroidal puzzles I've created.]({{ site.toroidal_url }})
2. To learn about Toroidals that are larger than 3x3, [I have some advice for that.](/tutorial/NoviceMethod_c_Larger.html)
3. The TLC Method is a beginner's method, with a dedicated gutter.  But the moves that use the gutter as described above can be applied without a dedicated gutter.  [Learn how to do that here.](/tutorial/SwingingIt.html)
3. Toroidals with at least one even dimension (e.g., 4x3) can lead to a configuration that is tricky to solve.  I describe the problem, and its solution [on this page.](/tutorial/1Swap.html)
3. You can go back to the main Toroidal page, and explore [my work on computer algorithms to solve Toroidal.](/index.html)



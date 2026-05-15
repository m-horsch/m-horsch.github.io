---
layout: post
title:  "The TLC Method for Larger Toroidals"
date:   2023-08-02 
---
<style>
img
{
    padding: 15px;
}
</style>
## Brief Introduction 

In a previous article, I described [the TLC method using examples, and with precise instructions.](/tutorial/NoviceMethod_b_Overview_.html)  This article gives some advice about how to manage larger Toroidal puzzles.

## Larger Toroidals
The TLC strategy works for any size Toroidal.  For example, to solve 5x5, the TLC is the 4x4 corner of the Toroidal, and row 5 and column 5 are gutters.  At the very end, once the top 4x4 TLC is completed, you can solve the gutter, again, only by moving tiles in the gutter.  For larger Toroidals, solving the gutter takes longer.

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

## Looking forward
1. You can leave this tutorial and try your hand at some of the [other Toroidal puzzles I've created.]({{ site.toroidal_url }})
3. Toroidals with at least one even dimension (e.g., 4x3) can lead to a configuration that is tricky to solve.  I describe the problem, and its solution [on this page.](/tutorial/1Swap.html)
3. My description of TLC has leaned heavily on a sequence of moves that move tiles to and from the gutter.  It turns out that this sequence of moves is completely general, and there is no actual need for a gutter at all.  I describe it [on this page.](/tutorial/SwingingIt.html)




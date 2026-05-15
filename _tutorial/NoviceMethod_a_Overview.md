---
layout: post
title:  "An Overview of the TLC Method to Solve Toroidal"
date:   2023-07-19 
---
<style>
img
{
    padding: 15px;
}
</style>

## Brief Introduction 

If you've landed this page without going through my main Toroidal page, [you can access it here](/index.html). 

This page gives a very brief overview of a method that novices can learn to solve Toroidal puzzles.  I will describe the method, and give you the opportunity to try it out with some hand-crafted examples.  This description may be enough for some.  However, I will present on subsequent pages a more detailed account for those who'd like more detail.

The TLC method will always work, but it may not result in the shortest possible solution.  Once you've practiced the TLC method, you have all the tools you need to explore 3x3 Toroidals to discover a shortest solution.  However, for larger Toroidals having at least one dimension with an even size (e.g., 4x3), there is a situation that can arise that can seem like a dead-end.  The natural moves needed to solve 3x3 Toroidals won't get you past this situation.  I show how to deal with it [on this page](/tutorial/1Swap.html).

## Beginner’s Strategy: TLC
TLC is a basic beginner's strategy.  It directs you to solve the Top Left Corner (TLC) first, and then solve the last row and column.  For a 3x3 Toroidal, the TLC is a 2x2 square; in general, it's every tile except the last row and column.  It can be any corner, in fact, but TLC has a certain ring to it.  

In the diagram below, we have a 3x3 Toroidal scrambled on the left, the same 3x3 Toroidal with the 2x2 TLC solved in the middle, and then the whole Toroidal solved on the right.  While the diagram has these stages side by side, it takes several moves (not shown) to progress from one to the next.

![A scrambled 3x3 Toroidal](/TImages/Stars3x3_scrambled.png) &rarr;
![The top left corner solved](/TImages/Stars3x3_TLC_done.png) &rarr;
![The completely solved Toroidal](/TImages/Stars3x3.png)

The basic idea is to solve the TLC using row 3 and column 3 as “gutters” to move tiles around.  While you are positioning the TLC tiles, you don’t care about the tiles in the gutter.  You can move the gutter freely, without affecting the TLC. 

![The last row and column are the gutters](/TImages/Stars3x3_gutter.png)

Once the TLC is done, you can solve the gutter by moving only the gutter row and gutter column.  A move that *only* affects the gutter will be called a gutter move.  Any move that affects the TLC will be called a TLC move.  

TLC moves displace tiles found in the TLC and in the gutter, but we will use TLC moves carefully so that they do not ruin what we've already accomplished. You can always move a tile from the TLC to the gutter, and from the gutter to the TLC, without ruining the work you’ve already done in the TLC.  The key is to make a TLC move, then a gutter move, then a TLC move again.  It’s always one of the following 2 possibilities:
1.	TLC row move, gutter column move, TLC row move back.
2.	TLC column move, gutter row move, TLC column move back.

Once the TLC is complete, all the tiles for the gutter are already in the gutter.  The only task is to move them into place.  We’ll solve the gutter row using only gutter moves.  Don't make any moves that affect the TLC.   

**You try it.**
For some readers, I may have already provided enough information to get you solving 3x3 Toroidals.  So I will end the brief overview here with a couple of tutorial examples for you to try on your own.
1. An example to [move one tile from the gutter to the TLC.]({{ site.toroidal_url | append: "?mode=training/TLC_practice.json"}}){:target="_blank"}
2. An example to [solve the TLC only.]({{ site.toroidal_url | append: "?mode=training/TLC_practiceB.json"}}){:target="_blank"}
3. An example to [solve the gutter only.]({{ site.toroidal_url | append:  "?mode=training/Gutter_practice.json" }}){:target="_blank"} 

## Looking forward
1. If I've given you enough direction already, you can leave this tutorial and try your hand at some of the [other Toroidal puzzles I've created.]({{ site.toroidal_url }})
2. If you want a bit more detailed guidance to master the TLC method, you can move on to [an article that describes the TLC in more detail.](/tutorial/NoviceMethod_b_Details.html)
3. Toroidals with at least one even dimension (e.g., 4x3) can lead to a configuration that is tricky to solve.  I describe the problem, and its solution [on this page.](/tutorial/1Swap.html)
3. You can go back to the main Toroidal page, and explore [my work on computer algorithms to solve Toroidal.](/index.html)



---
layout: post
title:  "Generating Toroidal Configurations for Fun and Science"
date:   2026-05-20
categories:  Technical
usemathjax: true
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
img
{
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>

I have described how I create randomized configurations in previous articles, but I've always considered it subordinate to their main message.  In this article, I will spell out the process in detail.  There is nothing too surprising about the process, and my purpose is only to centralize the discussion in one article, so that I can refer to it as necessary.  It may be helpful to review the [article on the Toroidal State Space](/solver/StateSpace).


### Starting from the end 
By default, every Toroidal puzzle has a standard goal configuration.  For 3x3 Toroidals, the default is just the integers 0 through 8 in sequence left to right, top to bottom.  For the Toroidal game, I simply split an image into tiles, and associate each tile with the corresponding image. 

![Numbered Image Configuration](/TImages/Stairs3x3Numbered.png) 

Sometimes, the default is not appropriate, for example, in the case of block puzzles.  If a block appears more than once in the puzzle, then it is not appropriate to give identical looking blocks different integer values.  Instead, each identical block gets the same integer value, and a goal configuration can be described in terms of the location of the blocks in the goal configuration.  My script allows the description of each block's number as the goal position.

![Numbered Image Configuration](/TImages/Cross3x3Numbered.png)

### Random walks in the state space  
Once the goal configuration is stablished, either by default, or by design, a sequence of random moves are applied to the goal configuration, resulting in a scrambled puzzle.  In the language of mathematics, this is called a random walk.  When we talk about solving, we always start with the scrambled state, and work towards the goal state.  When we talk about scrambling, we start with the goal state, and randomly walk away from it, to some (hopefully unpredictable) scrambled state.  

An important aspect of the random walk is the number of steps you take.  I've been using the letter $$K$$ to represent the number of steps.  If $$K$$ is very small, a random walk can never get too far from the goal state.  For an interesting puzzle, $$K$$ has to be big enough that the path back to the goal state is not obvious.  For my work in evaluating the performance of Toroidal solvers, $$K$$ has to be big enough that the resulting scrambled state looks like it was chosen at random from the possible states.  

I have an [article about the choice of $$K$$](/technical/Bias_C).  For 3x3 Toroidals, using $$K=50$$ is sufficient.  Larger values of $$K$$ don't create more interesting configurations, or more representative examples.

I have a second [article about the parity of the value of $$K$$](/technical/Bias_A).  For 4x4 Toroidals, if $$K$$ is even, no matter how big, then any solution, no matter how long, will also be even; likewise, if $$K$$ is odd, every solution will have an odd length.  In that article, I describe how to perturb any given value of $$K$$ by a small amount to avoid creating problems with any kind of predictable feature. 

### Cycles and cycle checking
The original version of my Toroidal generating script put no constraint on the random moves, other than that they had to be legal row and column moves.  At some point, I became worried that the resulting configurations could be too easy, as nothing prevented the random sequence from shuffling the same row back and forth a few times.  To avoid such useless shuffling, I implemented a technique called cycle checking. 

A cycle in this context is any sequence of steps that results in a configuration that has been seen previously, during the same random walk.  This could happen with a trivial undoing of a move, e.g., `L1 R1`, or any number of longer sequences.  Cycle checking is pretty easy do to, but does add some time to the scrambling process.  The bigger $$K$$ is, the more time is added.

I have an [article comparing cycle-checking to no cycle-checking](/technical/Bias_B).  It turns out that cycle-checking doesn't scramble "better" than without cycle-checking, provided that $$K$$, the number of steps, is sufficiently high.  It turns out that $$K=50$$ is sufficiently high.

### Implementation  
My script to create Toroidal puzzles defaults to the following behaviours:
  1.  $$K$$ must be specified.  There is no default value.
  2.  While a nominal $$K$$ is required, by default, the script uses a value created by randomly perturbing the given value of $$K$$.  The actual number of steps is hidden from any solving process.  It is possible to over-ride this default, and use exactly $$K$$, as given.
  3.  By default, the script does no cycle checking.  It is possible to over-ride this default.
  
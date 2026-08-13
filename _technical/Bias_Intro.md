---
layout: post
title:  "Addressing Bias in Toroidal Generation"
date:   2026-05-28
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
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>


### Background
When I create Toroidal puzzles for the Toroidal game, or when I explore strategies to solve Toroidal puzzles, I have to generate a randomized configuration for these puzzles using a computer program.  In statistical language, creating a randomized configuration is called sampling, and the randomized configuration is called a sample.  A sampling process can be biased if a dataset of samples does not match the population from which they are sampled.  

I have described how I create randomized configurations in previous articles, but I will summarize briefly here.  Basically, the sampling program chooses random legal moves, and applies these moves to a given goal configuration, resulting in random shuffling of rows and columns.  

If I were only interested in creating interesting sample puzzles for people to enjoy, I wouldn't worry about the sampling process too much, unless I had good reason to think I was creating poor puzzles.  However, I am also interested in comparing algorithmic strategies for solving Toroidals, and if my sampling process is biased, then my conclusions are also biased.  If I can avoid any bias, that would be best.  If I cannot avoid a bias, it is important, at the very least, to know how the samples are biased.

There are several choices that must be made to generate a randomized Toroidal configuration.  Each choice could introduce a bias.
  1. The magnitude of $$K$$, the number of random actions applied to the initial configuration.
  2. The parity (even or odd) of $$K$$.  
  4. The sampling program that generates a sample, given $$K$$.

When I am generating a dataset of samples, I also have to choose $$N$$, which is the number of samples in the dataset.

These choices are possibly mutually dependent, and if I want to discover and eliminate possible biases, I have to proceed carefully. No matter which of these I choose to tackle first, I have to make assumptions about the others. My plan is as follows.
  1. First, choose $$K$$ and $$N$$ sufficiently large, larger than what I might expect as  minimum values needed to assure that sampling is consistent with the population.
  2. Eliminate the known parity bias, using the large value for $$K$$.
  3. Verify that all sampling programs are unbiased, using the large value for $$K$$.
  4. Find a practical value for $$K$$ that is large enough to generate unbiased samples, but not so large that we waste time.
  5. Find a practical value for $$N$$ that balances confidence in future experiments, with efficiency.

### The Magnitude of $$K$$
$$K$$ is the number of random actions applied to the initial configuration to create a scrambled configuration.  For an interesting puzzle, $$K$$ has to be big enough that the scrambled state is not too close to the initial configuration.  For my work in evaluating the performance of Toroidal solvers, $$K$$ has to be big enough that the resulting scrambled state looks like it was chosen at random from the possible states.  To avoid needless work, we want $$K$$ to be the smallest value that's also large enough to create interesting puzzles, and representative scrambled configurations.

I address this topic in [this article](Bias_C).

### The Parity of $$K$$
Not too long ago, I noticed a correlation using my sampling methods: for 4x4 Toroidals, if $$K$$ is even, any solution found has even length.  Likewise, if $$K$$ is odd, the solution has odd length.  The correlation occurs for fairly large values of $$K$$. It occurs in all my informal, preliminary explorations of solving 4x4 Toroidals. It doesn't seem to matter if the solution is optimal or not. This parity correlation does not appear in 3x3 Toroidals.  I suspect the correlation will be present on Toroidals with even dimensions, e.g. 6x6, etc.  The vast majority of my work so far has been on 3x3 Toroidals, so I didn't notice this correlation until I declared my [3x3 work completed](/solver/3x3Done). Solving 4x4 Toroidals is significantly harder than 3x3, so my data is more limited, but I have seen no exceptions.

I address this topic in [this article](Bias_A).


### Random walks 
Scrambling the Toroidal grid by making random legal moves can be seen as a random walk through the Toroidal state space.  The original version of the Toroidal generating procedure put no constraint on the random moves, other than that they had to be legal row and column moves.  At some point, I became worried that the resulting configurations might be too easy, as nothing prevented the random sequence from shuffling the same row back and forth a few times.  To avoid such useless shuffling, I implemented a technique called cycle checking, which ensures that the random walk never revisits a previous state while scrambling.  It was much later that I became worried that cycle checking might result in samples that are statistically farther away from the goal than one should expect just by looking at the state space. 

In other words, I had two related, but different methods to scramble Toroidals, but I had no evidence that either one of them was unbiased.  I address this uncertainty in [this article](Bias_B).


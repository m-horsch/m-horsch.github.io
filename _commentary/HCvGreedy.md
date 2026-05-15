---
layout: post
title:  "On the difference between Hill-climbing Search and Greedy Search"
date:   2025-03-10
usemathjax: true
---

I'm retired.  It's great.  But there's no one here but me to make sure I get it right, and sometimes I don't get it right.  So if you notice any errors in my presentations, I would be grateful to be able to correct them!

As a case in point, in a few of the solver articles I talk about greedy algorithms.  But while putting the material together, I have flip-flopped between calling them greedy or hill-climbing.  These are similar and related concepts, though there is a technical difference.
 * A **greedy algorithm** makes a choice based on local and limited information, to try to achieve a good or maybe even optimal outcome.  It's a pretty generic description, and it covers a wide variety of algorithms.  In particular, any Toroidal solver that tries to choose the next "best" move to add to a sequence of moves, is a greedy solution.
* A **hill-climbing algorithm** is more specific.  It's a way of optimizing a complete solution, by nudging the complete solution in various ways to move it towards a better location.  The key is the requirement of a complete solution. For example, we might already have a long sequence of moves that solve a Toroidal puzzle, and we might devise a hill-climbing algorithm to improve the sequence by making it shorter.  I have not yet discussed this kind of algorithm.  There are some obvious easy optimizations that could be applied, but my current opinion is that it would be hard to design a hill-climbing algorithm that does more than a few obvious optimizations.  I could be wrong.  Maybe I'll look into it one day.

So there are a number of articles where I started calling some algorithms "greedy," then changed it to "hill-climbing" and then back to "greedy."  Hopefully, I've caught them all, and managed to smooth out any descriptions.


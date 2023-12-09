---
layout: post
title:  "The Toroidal State Space"
date:   2023-08-16
categories:  [Solver]
usemathjax: true
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
blockquote 
{
    color: #111;
    letter-spacing: 0px;
    font-size: 16px;
}
</style>

To date, we've tried to solve Toroidal using [Simple IDS]({% post_url 2023-08-02-IDS_A %}),
and [Enhanced IDS]({% post_url 2023-08-09-IDS_B %}).
Neither one of these is very effective.
Let's look a little deeper into the nature of Toroidal problems, to see where the trouble is.

**The smallest Toroidals.** Let's start with the smallest size, 2x2 Toroidals. [Here's an example for you to play with.]({{ site.toroidal_url | append:  "?mode=training/2x2.json"}}){:target="_blank"} 

A 2x2 Toroidal has 4 tiles, which can be arranged in $$\small 4! = 24$$ ways.
Every 2x2 Toroidal starting configuration will be one of these, and the goal is also one of these.  With 2 rows and 2 columns, there are 8 basic possible moves that can be made.  However, because the 2x2 grid is so small, exactly half of the moves are redundant. 
For example, consider moving the top row. 
If you shift it right, you end up with the same configuration that you would have gotten if you shifted the same row left.

The figure below shows one way to visualize the configurations and the basic moves.

![A visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpace.png){:width="60%"}

The diagram shows 24 small pictures of a 2x2 Toroidal with 4 unique tiles. 
Each line in the diagram represents a basic move that can be made, and the line leads to the configuration that results.  
This diagram is sometimes called a *state space visualization*.
Each Toroidal configuration is a *state*, and each line is a *transition* between states.

For example, here is a close-up of a transition between two states, at the top-left of the circle:

![A close-up showing one of the transitions in the 2x2 Toroidal state space.](/TImages/2x2_StateSpaceTrans1.png){:width="30%"}

The line connecting these two states represents the action of moving the top row.  If you start in the state on the right, and move the top row, the configuration matches the state on the left.  The transition also goes the other direction: if you start in the state on the left, and you move the top row, the Toroidal ends up in the state on the right.  Each transition in the full diagram is bi-directional like that.

The full diagram above can  tell us a few things.  Just by tracing around the circle, you can see that there is a path from any configuration to any of the others.  In fact, there are multiple paths between any two configurations.  Some paths trace through more states, and others through fewer states.  When we talk about solution length, we're actually talking about how many transitions we have to cross in the state space.
There will always be a path between any 2 states that is shortest, though there may be multiple paths with the same shortest length.

While the diagram might look like it could be navigated like a map, it is certainly not as useful as a map.
Locations in the real world are related to other locations by distance and direction.  
A map models the locations and distances to be as consistent as possible with the real world.  Locations in the real world are also related to the underlying shape of the real world.  On small scales, we can pretend the underlying shape is flat. A straight line between two points on a small map represents the shortest distance between those two point, and the length of that line can be calculated if you know the locations of the two points.  On larger scales, the earth's curvature means the calculations are more complicated, but they can still be done.  

But the diagram of the 2x2 Toroidal state space is not like a map.  A Toroidal state is not a location, and transitions don't have any direction.  The state space shows how states are related to each other, but not how they are related to any underlying shape.
There is no *right way* to organize the states and transitions for Toroidal. 
To make the diagram above even somewhat understandable, I had to decide where to put the states.  I laid them out in a circle, because it seemed to be the most helpful visualization.  There are short lines, and long lines, but each line represents a basic move, and no basic move is longer than any other.   If I had kept every line the same length, the Toroidal states would all be in one big pile, and the diagram would not help anyone understand anything.  

To illustrate this point, I moved the states around a bit, to produce the following diagram:

![An alternative visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpaceB.png){:width="60%"}

This diagram contains every state and transition in the original diagram.  While it may seem a little less tidy that the original, it is no less valid as a visualization.  There are still long lines, and short lines, and the pattern of transitions is not consistent.
There may be a nicer way to lay out the state space, but I did not take the time to find it.  

This is the first thing that make Toroidal hard to solve: it's hard to navigate.  Any computer program that tries to solve a 2x2 Toroidal has to find its way around this state space.  That's not to say that it's totally hopeless.  If most of the tiles are in the right place, you might have an intuition about what to do next.  But if most of the tiles are out of place, it's hard to know which move you should make.  

**Larger Toroidals.**  I will not be providing visualizations of state spaces for larger Toroidal varieties. They are simply too many states, as demonstrated in the table below.

| Rows | Columns | Number of States |
|--:|--:|--:|
| 2 | 2 |  4! = 24 |
| 3 | 3 |  9! = 362880 |
| 4 | 4 | 16! = 20922789888000 |
| 5 | 5 | 25! = 15511210043330985984000000 |

The 2x2 state space visualization above was not too hard to draw.  I would say it took me about an hour to draw all the possible states, and all the transitions between them.  It took me a little longer to arrange the states so that there was a nice path all the way around the circle.  

Imagine trying to draw a similar state space diagram for 3x3 with the states in a circle. 
There are 362880 states to draw, and each state has 12 lines to other states.
Some of the lines would be drawn to nearby states, but other lines would have to be drawn to states on the other side of the circle.  If each state were drawn with tiles of about the same size and spacing as in the above diagram, the diameter of the circle would be more than 1500m.  Drawing that many states and transitions would take months, not minutes.  

There's an interesting fact about 3x3 Toroidals.  The state space is separated into two distinct sets of states, both containing exactly half of the states.  I mentioned this in a previous posting on the [1-Swap Finish Problem]({% post_url 2023-07-28-1Swap %}).  So, there would be a way to draw the state space showing these two distinct sets.  There would be transitions within each set, but no transitions from one set to the other.  

For 4x4 Toroidals, it's almost unimaginable.  There are 21 million million states.  A circular layout of all the 4x4 configurations (with the same size and spacing as above) would have a diameter 62 times the Sun's diameter.  If we had a Sun the same diameter as the diagram for 4x4 Toroidals, it would appear in the sky about the same size as a large melon held at arm's length. 

This is the second reason that Toroidal is a hard problem to solve.  The number of possible states is unimaginably huge, after 3x3.  Any algorithm that tries to explore this state space has to find a way to ignore the vast majority of states.  


---
layout: post
title:  "The Toroidal State Space"
date:   2023-08-30
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
</style>

The algorithms I am using to solve Toroidal are technically known collectively as state space search.  In a future posting, I'll discuss the idea of search, but first we have to understand what a state space is, and more importantly, what Toroidal's state space looks like.  This is the key to understanding why building a general Toroidal solver is challenging.

A *state space* is an abstract, conceptual collection of *states,* and the *transitions* between them.  A state is usually defined as the information needed to completely define a system or process.  States are related to other states by *actions*; an action that changes a state causes a transition to another state.  

In Toroidal, the state is a particular configuration of tiles; basically a snapshot of the positions of all the tiles between moves.  The transitions are the simple moves that can be made.
The Toroidal state space represents every possible configuration, and every possible move that can be made.  

A state space can be represented or visualized by
a *network* of *nodes* connected by lines called *edges*.  In general, networks can represent many kinds of complex organization structures. 
<!-- 
For example, a roadmap is a network, where cities are nodes, and roads are edges.  The Internet is a network, with modems and routers as nodes, and with wires, cables, and even radio transmitters and receivers connecting them as edges. 
In computer science, this kind of network is usually called a *graph*. 
Graphs can be used conceptually, i.e., as a way of thinking about the organization of a system, but they can also be concrete, i.e., designed and constructed to organize actual data, enabling efficient computation.  Our use of networks will be conceptual; we will not build a graph explicitly.
 -->
For Toroidal, the nodes in the network represent states in the state space.  The possible moves in the Toroidal state space are represented by lines drawn between pairs of nodes.
Let's consider the [smallest variety of Toroidal, the 2x2 variety]({{ site.toroidal_url | append:  "?mode=training/2x2.json" }}){:target="_blank"}.
With 4 unique tiles, there are $\small 4! = 24$ possible configurations, or states.
(The notation $\small 4!$ refers to the factorial function: $\small 4! = 4 \times 3 \times 2 \times 1$.)  The factorial function is used to count simple permutations and combinations.  If you have 4 things, there are $\small 4! = 24$ ways you can arrange them.

With 2 rows and 2 columns, there are 8 possible moves that can be made.  However, because the 2x2 grid is so small, exactly half of the moves are redundant. 
For example, consider moving the top row.  If you shift it right, or left, you end up with the same configuration.  So in 2x2 Toroidal, each configuration is connected to 4 other configurations.  The figure below shows one way to visualize this state space.

![A visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpace.png){:width="60%"}

The nodes in the diagram are small pictures of a 2x2 Toroidal with 4 unique tiles.  The nodes are laid out in a circle, because it seemed to be the most helpful visualization.
Each line in the diagram is an edge that represents a move that can be made, and the edge leads to the configuration that results.  I did not label the moves; each edge could have the name of the corresponding move, e.g. `L1` or `D2`, but that was just a little too much work.  

In the case of 2x2 Toroidal, you can pick any 2 nodes, and there will always be a path  from one node to the other by moving around the network following edges.
Some paths are longer, and others are shorter.  There will be a path that is shortest, though there may be multiple paths with the same shortest length.

For 3x4, 4x3, and 4x4 Toroidals, there will always be a path between any two configurations.  I suspect (but have not proved) that this property is true for all Toroidals where one dimension has even size.  On the other hand, the state space for 3x3 Toroidals 
has two equal sized halves (like islands), and there is no edge to get from one half to the other.  Again, I have not proved it, but I suspect this is true for any Toroidal where both dimensions have odd sizes.
That's why I create puzzles by applying random moves: if the state space is split into islands, applying random moves is a way of guaranteeing that we stay in the one half where we started.
That's also why the [1SF problem]({% post_url 2023-07-28-Toroidal-1Swap %}) is unsolvable for 3x3: it basically means that the initial position is in one half, and the goal position appears in the other half.  This could happen if a Toroidal were created by randomly permuting the tile locations.

I will not be providing visualizations of state spaces for larger Toroidal varieties. They are simply too many configurations, as demonstrated in the table below.

| Rows | Columns | Moves | Configurations|
|--:|--:|--:|--:|
| 2 | 2 | 4 | 4! = 24 |
| 3 | 3 | 12 | 9! = 362880 |
| 4 | 4 | 16 | 16! = 20922789888000 |
| 5 | 5 | 20 | 25! = 15511210043330985984000000 |

The size of the state space gets really big even for relatively small Toroidals.  To draw a similar state space diagram for 3x3, in a circle, every degree around this circle would have more than 1000 nodes.  If each node were drawn with tiles of about the same size and spacing as in the above diagram, the diameter of the circle would be more than 1500m.  

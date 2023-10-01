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
Let's look a little deeper into the nature of Toroidal problems.

Let's start with the smallest size, 2x2 Toroidals. [Here's an example for you to play with.]({{ site.toroidal_url | append:  "?mode=training/2x2.json"}}){:target="_blank"} 

A 2x2 Toroidal has 4 tiles, which can be arranged in $$\small 4! = 24$$ ways.
Every 2x2 Toroidal starting configuration will be one of these, and the goal is also one of these.  With 2 rows and 2 columns, there are 8 basic possible moves that can be made.  However, because the 2x2 grid is so small, exactly half of the moves are redundant. 
For example, consider moving the top row. 
If you shift it right, you end up with the same configuration that you would have gotten if you shifted the same row left.
This duplication can be removed, [as suggested by a previous posting]({% post_url 2023-08-09-IDS_B %}).

The figure below shows one way to visualize the configurations and the basic moves.

![A visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpace.png){:width="60%"}

The diagram shows 24 small pictures of a 2x2 Toroidal with 4 unique tiles. 
Each line in the diagram represents a basic move that can be made, and the line leads to the configuration that results.  
This diagram is sometimes called a *state space visualization*.
Each Toroidal configuration is a *state*, and each line is a *transition* between states.  
Any computer program that tries to solve a 2x2 Toroidal has to find its way around this state space.

It can also tell us a few things.  Just by tracing around the circle, you can see that you can always find a path from one configuration to any of the others.  You can see that there are multiple paths between any two configurations.  Some paths trace through more states, and others through fewer states.  When we talk about solution length, we're actually talking about how many transitions we have to cross in the state space.
There will always be a path between any 2 states that is shortest, though there may be multiple paths with the same shortest length.

The diagram's layout is entirely arbitrary. To make the diagram even somewhat understandable, I had to decide where to put the states.   The pictures are laid out in a circle, because it seemed to be the most helpful visualization.  But there is no *right way* to organize the states and transitions for Toroidal.  The Toroidal state space is not like a map, where the location of cities in the map is a model of their actual location in the world.  

On a map, longer lines usually represents longer distances, but that is not true here.  
There are short lines, and long lines, but each line represents a basic move, and no basic move is longer than any other.   If I had kept every line the same length, the Toroidal states would all be in one big pile, and the diagram would not help anyone understand anything.

People (and computers) are relatively good at using maps, because maps represent arrangements that are real and intuitive.  If you are *here,* and you want to get *there,* it's generally a good idea to start moving in the direction that gets you closer to *there.*  This intuitive correspondance between *here* and *there* is not true in Toroidal.  There is nothing intuitive about the way states are connected to each other.

This is the first thing that make Toroidal hard to solve.  If most of the tiles are in the right place, you might have an intuition about what to do next.  But if most of the tiles are out of place, then there's almost no hint about which move you should make.

I will not be providing visualizations of state spaces for larger Toroidal varieties. They are simply too many configurations, as demonstrated in the table below.

| Rows | Columns | States |
|--:|--:|--:|
| 2 | 2 |  4! = 24 |
| 3 | 3 |  9! = 362880 |
| 4 | 4 | 16! = 20922789888000 |
| 5 | 5 | 25! = 15511210043330985984000000 |

The 2x2 state space was not too hard to represent.  I would say it took me about an hour to draw all the possible states, and all the transitions between them.  It took me a little longer to arrange the states so that there was a nice path all the way around the circle.  

Imagine trying to draw a similar state space diagram for 3x3 with the states in a circle. 
There are 362880 states to draw, and each state has 12 lines to other states.
Some of the lines would be drawn to nearby states, but other lines would have to be drawn to states on the other side of the circle.  If each state were drawn with tiles of about the same size and spacing as in the above diagram, the diameter of the circle would be more than 1500m.  Drawing that many states and transitions would take months, not minutes.

For 4x4 Toroidals, it's almost unimaginable.  There are 21 million million states.  A circular layout of all the 4x4 configurations (with the same size and spacing as above) would have a diameter 62 times the Sun's diameter.  If we had a Sun the same size as the diagram we're imagining, it would appear in the sky about the same size as a large melon held at arm's length. 

This is the second reason that Toroidal is a hard problem to solve.  The number of possible states is unimaginably huge, after 2x2.  Any algorithm that tries to explore this state space has to find a way to ignore the vast majority of states.

Now let's count moves and move sequences.  For a 2x2 toroidal, there are 8 moves, with half of them redundant.  But for 3x3, and larger, there are no redundant moves (though there are redundant sequences).


| Rows | Columns | Basic Moves | Depth |
|--:|--:|--:|--:|
| 2 | 2 | 4 | 4 |
| 3 | 3 | 12 | 8 |
| 4 | 4 | 16 | ?20|
| 5 | 5 | 20 | ?40 |




## moved 

  It's easy to see that each configuration has 4 other configurations one move away; that's what the lines are for.  It's also pretty easy to count the number of configurations 2 moves away.  If you are patient and careful, you can check that, no matter which configuration you choose, there is exactly one other configuration 4 moves away, and none farther than 4.  



A 3x3 Toroidal also has 12 basic moves, 2 for every row and column.  Each move takes you from one of the 362880 configurations to another.  So, it's almost like we have a map of 362880 cities, connected by roads.  Each city is connected to 12 other cities.  The problem of 
solving a 3x3 Toroidal is equivalent to finding a route from any one of the cities to any other city.

But as I said in a previous posting, this Toroidal map is not like real world geographical maps.  To understand this, let's look at 2x2 Toroidals. 

In the diagram below, I have selected one of the configurations, the one at the 12 noon position, to be the goal configuration.  The top row is AB, and the bottom row is CD, so an alphabetic order on the tiles.  Since this is the goal position, it is zero moves away from the goal position, and the diagram labels it with a big 0.  Every line leading from this position is one move away from the goal position, so the diagram labels these with a big 1.  We can continue to label the states with numbers representing their actual distance from the goal position.

![The 2x2 state space labelled with the number of moves from the goal position at 12 noon.](/TImages/2x2_PathLength.png){:width="60%"}

We can 


As we saw in a previous posting, the 3x3 problem is more or less solved, by a number of methods that get pretty good solutions in a few tens of milliseconds, on average.  

| Rows | Columns | Basic Moves | Depth|
|--:|--:|--:|--:|
| 2 | 2 | 4 | 4! = 24 |
| 3 | 3 | 12 | 9! = 362880 |
| 4 | 4 | 16 | 16! = 20922789888000 |
| 5 | 5 | 20 | 25! = 15511210043330985984000000 |


A *state space* is an abstract, conceptual collection of *states,* and the *transitions* between them.  A state is usually defined as the information needed to completely define a system or process.  States are related to other states by *actions*; an action that changes a state causes a transition to another state.  It's abstract, because it can be applied to many kinds of computational problems.  If you can identify states and transitions for any given problem, you can apply this concept, and you can apply state space search algorithms to it.  

In Toroidal, the state is a particular configuration or arrangement of tiles; basically a snapshot of the positions of all the tiles.
The transitions are the simple moves that can be made.
The Toroidal state space represents every possible configuration, and every possible move that can be made.  

A state space can be represented or visualized by
a *network* of *nodes* connected by lines called *edges*.
In general, networks can represent many kinds of complex organization structures. 
<!-- 
For example, a roadmap is a network, where cities are nodes, and roads are edges.  The Internet is a network, with modems and routers as nodes, and with wires, cables, and even radio transmitters and receivers connecting them as edges. 
In computer science, this kind of network is usually called a *graph*. 
Graphs can be used conceptually, i.e., as a way of thinking about the organization of a system, but they can also be concrete, i.e., designed and constructed to organize actual data, enabling efficient computation.  Our use of networks will be conceptual; we will not build a graph explicitly.
 -->
For Toroidal, the nodes in the network represent states in the state space.  The possible moves in the Toroidal state space are represented by lines drawn between pairs of nodes.
Let's consider the [smallest variety of Toroidal, the 2x2 variety]({{ site.toroidal_url | append:  "?mode=training/2x2.json" }}){:target="_blank"}.
With 4 unique tiles, there are $\small 4! = 24$ possible configurations, or states.
(The notation $\small 4!$ refers to the factorial function: $\small 4! = 4 \times 3 \times 2 \times 1$.)  The factorial function is used to count simple permutations and combinations.  If you have 4 things, there are $\small 4! = 24$ ways you can arrange them.

In the case of 2x2 Toroidal, you can pick any 2 nodes, and there will always be a path  from one node to the other by moving around the network following edges.
Some paths are longer, and others are shorter.  

For 3x4, 4x3, and 4x4 Toroidals, there will always be a path between any two configurations.  I suspect (but have not proved) that this property is true for all Toroidals where one dimension has even size.  On the other hand, the state space for 3x3 Toroidals 
has two equal sized halves (like islands), and there is no edge to get from one half to the other.  Again, I have not proved it, but I suspect this is true for any Toroidal where both dimensions have odd sizes.
That's why I create puzzles by applying random moves: if the state space is split into islands, applying random moves is a way of guaranteeing that we stay in the one half where we started.
That's also why the [1SF problem]({% post_url 2023-07-28-1Swap %}) is unsolvable for 3x3: it basically means that the initial position is in one half, and the goal position appears in the other half.  This could happen if a Toroidal were created by randomly permuting the tile locations.


## from Manhattan

*A small increase creates a formidable challenge.* There are two compounding factors that change when we proceed from 3x3 to 4x4.  First, the number of possible single moves increases from 12 in the 3x3 case, to 16 in the 4x4 case.
The Enhanced IDS can prevent a lot of useless sequences, so the number of possibly useful moves can be reduced from 16 to something like 12 (rough guess) in the 4x4 case.
Secondly, because there are more tiles to put into place, the maximum number of moves needed to solve a Toroidal increases from 8 (known fact for 3x3) to something like 20 (rough guess for 4x4).  

These two factors make solving 4x4 Toroidals very challenging.  
If we're using Simple IDS, the number of sequences for 3x3 Toroidals is $$\small 12^{8}$$, or roughly 430 million, which is a number in the range of human experience; it's larger than the population of the USA, but smaller than the total population of Europe (2023 data).  However, the number of sequences for 4x4 Toroidals is beyond human experience: $$\small 16^{20}$$, or roughly 1.2 million million million million sequences.  That's roughly the population of 151 million million Earths.  That's still too big to think about.  If every one of the estimated 50 million habitable planets in the Milky Way had Earth's population, we'd be talking about 3 million Milky Ways.  

Even if we use Enhanced IDS, preventing useless sequences, the number of sequences to explore is on the order of $$\small 12^{20}$$ which is about the population of 500 thousand million Earths, or 10 thousand Milky Ways.  IDS is totally infeasible for 4x4 Toroidals, and larger.

|              |    3x3 Sequences  | 3x3 Time | 4x4 Sequences      | 4x4 Estimated Time |
|:-------------|------------------:|---------:|-------------------:|--------:|
| Simple IDS   | $$\small 12^{8}$$ |  *322*   | $$\small 16^{20}$$ |    -    |
| Enhanced IDS | $$\small 8^{8}$$  |  12.6s   | $$\small 12^{20}$$ |    -    |
| A\*   MTC    | $$\small 8^{8}$$  |  0.02s   | $$\small 12^{20}$$ | *6.3h*  |

The table also gives average times from previous experiments 
([Simple IDS]({% post_url 2023-08-02-IDS_A %}), 
[Enhanced IDS]({% post_url 2023-08-09-IDS_B %}), 
[A\* MTC]({% post_url 2023-08-16-AStar %})).  The times presented in *italics* are estimates.
In the case of Simple IDS on 3x3 Toroidals, I calculated an estimate of the time needed to explore to depth 8, given the successful solutions to shorter depths.
The estimate of 6.3 hours for A\*   MTC on 4x4 Toroidals is a ball-park figure only, based on the limited success described in the opening paragraph above.


However, this approach has a weakness, namely that many configurations end up with the same priority, leading to the use of a "first come, first served" tie-breaking rule. This wasn't a big issue for 3x3 Toroidals due to the relatively limited number of configurations. However, for larger ones like 4x4 Toroidals, which have many more configurations with identical priorities using MTC, the problem becomes more pronounced.

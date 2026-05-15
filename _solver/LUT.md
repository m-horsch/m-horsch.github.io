---
layout: post
title:  "Mapping the Toroidal State Space"
date:   2023-10-18
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

### Abstract
This article describes the application of a look-up table (LUT) to solve Toroidal puzzles of size 3x3 and smaller.  The table stores the best move for each configuration of tiles, assuming a specific goal state.  The tables are relatively small, and can easily fit into main memory for most modern personal computing devices.  A simple traversal algorithm using this table can solve randomly scrambled 3x3 Toroidal puzzles with an average time of about 0.1 milliseconds.  In practice, this technique is limited in application, as the size of such tables for Toroidals larger than 3x3 is prohibitively large.

### Details
In a previous article, [I presented the idea of a state space](/solver/StateSpace.html).  I said that any computer program to solve Toroidals has to navigate the state space.  This is quite a literal statement: A solution to a Toroidal is a path from the start state to the goal state, using the transitions allowed in the state space diagram.  A path is the answer to the question "Which moves do we need to make to get from the start state to the goal state?"  The state space is tells us which moves are possible.

Let's look at the 2x2 Toroidal state space again, in a couple of ways.  First, the diagram we've seen once before:

![A visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpace.png){:width="60%"}

Let's use the state at the 12 noon position, with the labels *A, B* on the top row, and *C, D* on the bottom row,  as the goal state.  There are a 4 states that are exactly one move away; these are easy to identify, since we only have to follow one line out of the goal state.  It is possible to calculate how far away from the goal that each state is, by identifying all the states 1 move away, then 2 moves away, etc.  That's how the following diagram was composed.

![The 2x2 state space labelled with the distances from the goal state. ](/TImages/2x2_PathLength.png){:width="60%"}

I've colour-coded the states, replacing the arrangement of 4 tiles with the number of moves needed to get to the goal state.  The goal state is labelled with the number 0, because it requires zero moves to get to the goal state from the goal state.  There are 4 states with the label 1, and a number of states with labels 2 and 3.  There is one state in this diagram with the label 4, which happens to be the only state that is 4 moves away from the goal state.

It is very useful to know the true distance between any state and the goal state.  If we have this information, navigating the state space is easy.  Unless you are at the goal state already, you can always move to a state that is one step closer to the goal state; one of the 4 transitions must move you to a state with a smaller number.  If there is a tie, it doesn't matter which one you use.  To solve a 2x2 Toroidal, we only need to keep one transition to a state with a smaller label; all the other transitions are unnecessary.
For example, a state with the label 3 might be connected to more than one state with the label 2. In the diagram below, I removed all the unnecessary transitions.    

![The 2x2 state space labelled with redundant transitions removed. ](/TImages/2x2_PathLengthPruned.png){:width="60%"}

All the possible states are represented, and each state has exactly one transition to another state that whose label is smaller, i.e., the state is closer to the goal.  It's not very clear laid out in a circle, so let's just move the lines and boxes around, so that the diagram is more organized.

![The 2x2 state space reorganized into a tree structure. ](/TImages/2x2_PathLengthPrunedTree.png)

This organization is called a *tree* in computer science, and it has the property that each state points to at most one other state, though each state may have several states pointing to it.  A tree diagram shows hierarchy, and in this case, it shows common paths from states to the goal state.  And if you're a little confused about the name, yes, computer science typically draws trees upside down.  In this case the goal state is called the *root*, and the branches are below the root.  It's an arbitrary choice, but makes drawing tree-diagrams a little easier: start at the top of the page, and work downwards as far as you need to go.

Now, let's remove those labels, and return to a diagram where the states are shown explicitly.

![The 2x2 state space labelled with redundant transitions removed. ](/TImages/2x2_StateSpacePrunedTree.png)

I've also gone to the trouble to label each transition with a move.   I used only `L` and `U` moves, but remember that in the 2x2 case, every `L` move has an equivalent `R` move; for example the effect of `L1` is the same as `R1` in a 2x2 Toroidal.  It really doesn't matter whether I use `L1` or `R1`.

With this diagram, there is a move for every state, and the move takes you closer to the goal state.  This is enough information to solve the 2x2 Toroidal with the fewest number of moves.  In a sense, this is the map for the state space, and you can get to the goal state from anywhere.

Now, finally, we can reduce this diagram to a form that the computer can use easily to solve 2x2 Toroidals.  We can delete all the transition lines, and simply associate each state with the move that the line represented.  

![The 2x2 state space labelled with redundant transitions removed. ](/TImages/2x2_SolutionTable.png)

This is a pictorial form of a computer science tool called a *look-up table.*  Each state appears in the table, and beside each state is the best move to make.  With this look-up table, solving a 2x2 Toroidal is a matter of looking for the state in the table, taking the associated move and applying it to the Toroidal.  In 4 moves or less, the 2x2 Toroidal will be solved.  The whole table easily fits into a computer's memory.

For 3x3, it would be nearly impossible to build a complete look-up table by hand.  So I adapted the Enhanced IDS solver to build it, and save it to a file; running this program took about 15 seconds on my 2023 MacBookPro.  This is a computation that we only need to do once, provided that we can store the look-up table permanently.  I designed the table to store the state of the Toroidal as a string of digits, along with the move that moves to a better state, and then the better state itself.  The first few lines look like this:

    012345678 None None
    201345678 L1 012345678
    012534678 L2 012345678
    012345867 L3 012345678
    612045378 U1 012345678
    072315648 U2 012345678

The digits 0 through 8 represent the nine tiles, and the sequence of 9 digits represents the Toroidal, by rows, top to bottom and left to right. For example, the sequence `012345678` shows that the first row contains tiles `0`, `1`, and `2`; the second row contains the tiles `3`, `4`, and `5`, etc.
The first line above is the goal state, which has no action (`None`) and no better state (`None`).  The next few lines show states, moves, and the resulting states after the moves.  The table was stored in a file containing 181440 lines (because the 3x3 [state space](/solver/StateSpace.html) is split into 2 equal-sized halves), requiring about 4MB of storage.  

The program that builds the table was able to report on the distance of every possible state to the goal state, even though this information was not stored in the file itself.  As a result, we know that every 3x3 Toroidal can be solved in 8 moves or less.


**Running the algorithm.**  I designed a new Toroidal solver to use the look-up table, and applied it to the same set of 3x3 Toroidals we used in previous articles.  It's important to emphasize that the look-up table is not computed every time we solve a 3x3 Toroidal.  The look-up table is computed once and for all; the solver repeatedly uses the table, but does not repeatedly build the table.

In this look-up table, every state has exactly one prescribed move, so solving any 3x3 Toroidal can be very fast, as fast as looking up states can be.  Based on solving all 100  3x3 Toroidals, the average time to solve any one of them is about 0.0001 seconds (about one-tenth of a millisecond).  Here's a small table comparing the methods I've applied so far, showing average time to solve a single puzzle, based on the same set of 100 Toroidals:

|               | 3x3 Average Time  |
|:------------------------------------|-----------:|
| [Simple IDS](/solver/IDS_A.html)    |   *322s*   |
| [Enhanced IDS](/solver/IDS_B.html)  |   12.6s    |
| Look-up Table                       |   0.0001s  |

It takes some time to build the look-up table, but once the table is built, solving a 3x3 Toroidal takes almost no time at all.  The average time reported above does not include the time to build the table, of course.  The table is stored in a file, which the solver has to read, and this takes a few hundred milliseconds; this time is also not included in the average.

This solution method was possible because the look-up table is a complete map of the state space, computed in advance.  The cost of building this map for 3x3 is relatively low, but the effort pays for itself every time the table is used.  But for Toroidals bigger than 3x3, building a look-up table is not even remotely feasible. Let's revisit the table from the previous article.

| Rows | Columns | Number of States |
|--:|--:|--:|
| 2 | 2 |  4! = 24 |
| 3 | 3 |  9! = 362880 |
| 4 | 4 | 16! = 20922789888000 |
| 5 | 5 | 25! = 15511210043330985984000000 |

Modern desktop and notebook computers are relatively fast and have lots of memory, but to find all the distances in the 4x4 case, it would take months or years, depending on how clever you are in your calculations, and even storing the look-up table for 4x4 Toroidals would require more memory than they currently have.  It's conceivable that the most advanced super-computers currently in operation could manage the 4x4 case, but the 5x5 case is a million million times worse, and not even today's most advanced super-computer could manage it.

This technique of using a look-up table hits a sweet spot for 3x3 Toroidals, but we need another way to navigate the state space for larger Toroidal puzzles.

**Looking forward.**
In the next article, we'll drop the map, and see how to [solve Toroidal using exact distances](/solver/DIST.html).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/LUT.html).
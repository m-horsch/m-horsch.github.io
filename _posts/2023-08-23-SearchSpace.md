---
layout: post
title:  "Navigating the Toroidal State Space"
date:   2023-08-23
categories:  [Technical]
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
In a previous post, [I presented the idea of a state space]({% post_url 2023-08-16-StateSpace %}).  I said that any computer program to solve Toroidals has to navigate the state space.  This is quite a literal statement: A solution to a Toroidal is a path from the start state to the goal state, using the transitions allowed in the state space diagram.  It's the answer to the question "Which moves do we need to make to get from the start state to the goal state?"  

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

**Running the algorithm.**  I wrote a Toroidal solver using this idea, and applied it to the same set of 3x3 Toroidals we used in previous postings.  The 2x2 case was easy enough to do by hand.  For a computer, it would be trivial.
For 3x3, it would be nearly impossible to build the look-up table by hand, but possible with a modern computer; it takes about 15 seconds.  This is a computation that we only need to do once, provided that we can store the look-up table permanently.  I designed the table to store the state of the Toroidal as a string of digits, along with the move that moves to a better state, and then the better state itself.  The first few lines look like this:

    012345678 None None
    201345678 L1 012345678
    012534678 L2 012345678
    012345867 L3 012345678
    612045378 U1 012345678
    072315648 U2 012345678

The digits 0 through 8 represent the nine tiles.
The first line above is the goal state, which has no action and no better state.  The next few lines show states, moves, and the resulting states after the moves.  The table was stored in a file containing 181440 lines (because the 3x3 [state space]({% post_url 2023-08-16-StateSpace %}) is split into 2 equal-sized halves), requiring about 4MB of storage.  I designed the solver to open, read, and store this file once, and solve all 100 Toroidals using it.  

Since every state has exactly one prescribed move, solving any 3x3 Toroidal is very fast.
It was able to solve all 100 3x3 Toroidals in an average time of about 0.0001 seconds (about one-tenth of a millisecond).  Here's a small table comparing the methods I've applied so far, showing average times for the same set of 100 Toroidals:

|               | 3x3 Average Time  |
|:--------------|----------:|
| Simple IDS    |   *322s*  |
| Enhanced IDS  |   12.6s   |
| Look-up Table |   0.0001s  |

It takes some time to build the look-up table, but the evidence shows that once the table is built, solving a 3x3 Toroidal takes almost no time at all.

This solution method was made possible because the distance from each state to the goal state was known.
In practice, there is no known way to calculate exact distances in the Toroidal state space without first exploring the state space.  In other words, the only way we know of to calculate those distances is by counting transitions within the state space itself.  WE have to count from every state to the goal state.  The 2x2 case was easy enough to do by hand.  It would be nearly impossible build a look-up table by hand for 3x3 Toroidals, but easily possible with a modern computer.

After that, it's *hopeless*. Let's revisit the table from the previous posting.

| Rows | Columns | Number of States |
|--:|--:|--:|
| 2 | 2 |  4! = 24 |
| 3 | 3 |  9! = 362880 |
| 4 | 4 | 16! = 20922789888000 |
| 5 | 5 | 25! = 15511210043330985984000000 |

Modern computers are fast and have lots of memory, but to find all the distances in the 4x4 case, it would take the fastest computer months or years, depending on how clever you are in your calculations.  Storing the look-up table for 4x4 Toroidals would require more memory than any modern computer currently has.  Let's not even think about 5x5, because it's a million million times worse.

So a look-up table can be very fast, but it is only practical for relatively small Toroidals.  We need another way to navigate the Toroidal state space.

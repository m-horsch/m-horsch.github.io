---
layout: post
title:  "Navigating Toroidal with Exact Distances"
date:   2023-11-01
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
This article describes the application of a distance table (DIST) to solve Toroidal puzzles of size 3x3 and smaller.  The table stores the actual distance between a specific goal state, and every other possible Toroidal state.  The tables are relatively small, and can easily fit into main memory for most modern personal computing devices.  A simple greedy search algorithm using this table can solve randomly scrambled 3x3 Toroidal puzzles with an average time of about 0.5 milliseconds.  As with the look-up table variant (LUT), this technique is limited in application, as the size of such tables for Toroidals larger than 3x3 is prohibitively large.

### Details
In the previous article, I showed how we could [map the Toroidal state space](LUT.html) by constructing a look-up table (LUT).  In that article, the table stored the state of the Toroidal as a string of digits, along with the move that transitions to a better state, and then the better state itself.  The first few lines looked like this:

    012345678 None None
    201345678 L1 012345678
    012534678 L2 012345678
    012345867 L3 012345678
    612045378 U1 012345678
    072315648 U2 012345678

This is very convenient for 3x3 Toroidals, and smaller, but calculating and storing this information is wildly impractical for larger Toroidals.  

In this short article, I'll show a different kind of look-up table, which will be able to solve the same smallish Toroidals, in a slightly different way.  In the next few articles, we'll see how to use this new idea to tackle larger Toroidals.

Let's revisit the state space diagrams for 2x2 Toroidals.
As a reminder, here's the diagram of the 2x2 case:

![A visualization of the 2x2 Toroidal state space.](/TImages/2x2_StateSpace.png){:width="60%"}

And here's the diagram of the state space, with distances replacing the state representation:

![The 2x2 state space labelled with the distances from the goal state. ](/TImages/2x2_PathLength.png){:width="60%"}

As I wrote earlier, it is very useful to know the true distance between any state and the goal state.  If we have this information, navigating the state space is easy.  Unless you are at the goal state already, you can always move to a state that is one step closer to the goal state; one of the 4 transitions must move you to a state with a smaller number.  If there is a tie, it doesn't matter which one you use.    

In my article about Look-up Tables, I pruned this diagram, removing unneeded transitions, to form a navigational tree.  In this article, we're going to leave it right here, and calculate a look-up table with just states, and corresponding distances, as follows:

    012345678 0
    201345678 1
    012534678 1
    012345867 1
    612045378 1
    072315648 1
    
This table is just a list of states, and their distance to the goal state.  In the above, the first line is the goal state itself.  The following lines are states that are all one move away from the goal.  This is an artifact of the way I calculated the table, ordered by increasing distance to the goal.

I want to emphasize that these distances are true, and not estimates.  We can calculate them for 3x3 Toroidals, and smaller, but not for larger Toroidals.  A program to solve Toroidals can be designed to use these distances to find the minimal solution, and it would be roughly as efficient as the [Look-up Table from before](LUT.html).  As I wrote above:

> Unless you are at the goal state already, you can always move to a state that is one 
> step closer to the goal state; one of the 4 transitions must move you to a state with a 
> smaller number.  If there is a tie, it doesn't matter which one you use.

This description can be literally encoded in a computer program.  In the language of Computer Science, this would be called a *greedy* algorithm.  This kind of algorithm makes decisions based on limited information.  In this case, the algorithm decides which transition to take based on how much closer it can get.  Since the distance information in this particular application is exact, we never need to revisit decisions, and the solution the greedy algorithm finds will be as good as, or better than, any of the possible solutions.  As well, we don't need to take special steps to eliminate useless sequences when using a distance table, because there are no useless steps on any shortest solution.  

In this case, the greedy algorithm makes use of the exact distance information from the look-up table; not all greedy algorithms have access to a table like this.  For Toroidal, the real work is done when the look-up table is constructed.  For the 3x3 case, it was not too much work to do.  For larger Toroidals, the size of the necessary look-up table becomes a problem: no modern desktop computer could store such a table.

In general, greedy algorithms can be applied to many kinds of computational problems, but they may not always work, or they might find a solution that is not among the best solutions.  It very much depends on what kind of information is available, and how well a specific computational problem will suit a greedy algorithm.  

**Running the algorithm.**  I designed a new Toroidal solver to use the table of true distances, and applied it to the same set of 3x3 Toroidals we used in previous articles.  As with the previous article, the table is computed once and for all, and stored in a file.  It takes a few hundred milliseconds to read the file for 3x3 puzzles, but this is done only once, before solving any of the examples.

The table below compares the average time to solve one 3x3 Toroidal, compared with the other techniques we've seen so far.  The row labelled "Distance Table" is new, and the others are from previous articles:


|                | 3x3 Average Time  |
|:---------------|-----------:|
| [Simple IDS](/solver/IDS_A.html)    |   *322s*   |
| [Enhanced IDS](/solver/IDS_B.html)  |   12.6s    |
| [Look-up Table](/solver/LUT.html)   |   0.0001s  |
| Distance Table |   0.0005s  |

Using exact distances takes about 5 times longer than using the look-up table from before, but both are extremely fast, compared to all the previous methods.  The Look-up Table method is faster, because the look-up table dictates exactly which moves to make, immediately and without any ambiguity.  For the Distance Table method, the computer has to look at all the moves it could make, and choose the one that gets it closest.  So there is a bit of extra time needed to decide each additional step.

**Looking forward.**
The problem with the distance table method is not the extra costs, but the table size.  For any Toroidal bigger than 3x3, the tables are too big to create, and too big to store.  If we could calculate the distances quickly and without storing them, we'd be able to solve larger Toroidals.

In the next article, we'll see how to estimate distance in the Toroidal state space and [solve Toroidal using estimated distances](EstimatingDistances).

**Data Provenance.**
Detailed information about the data summarized in this article can be found
[here](/dataprovenance/DIST.html).

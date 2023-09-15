---
layout: post
title:  "Toroidal: An Introduction"
date:   2023-07-12 
categories:  General
---
<style>
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>

![Screenshot of Mike's Toroidal game](/TImages/Screenshot300.png)
**Toroidal** is the name I have given to a puzzle game I have been
working on in my retirement.  You can find my version of it here: [Toroidal]({{ site.toroidal_url }}). The game challenges the player to rearrange a rectangular grid of image tiles, to produce an image of some kind.  You can slide tiles left or right, up or down.  The whole row (or column) moves, and any tile pushed towards the edge of 
the grid reappears on the opposite side.   

<br/>
**Origin.**  Toroidal is basically a 2-dimensional Rubik's Cube, so I make no claims about originality.   When I set out to build this game, I was unaware of any other implementations, but I imagined there had to be at least a few.  I have since found a small number of them.  I might post them on a separate page.  I've used the basic concept as an assignment in first year, as a way to practice 2-dimensional arrays.  First year students were not required to write code to solve Toroidal puzzles, but I did expect them to be able to verify that a given sequence of moves was indeed a solution.  I also used the basic Toroidal puzzle as an application of classical AI techniques for third year students.  

## The Puzzles
**Size.**  Currently (as of this date), the game selects a random puzzle from
my library of about 100 puzzles.   A large proportion of puzzles
are 3x3 grids, though there are 3x4, 4x3, 4x4, and 5x5 puzzles in
the current collection.  

![Block-style puzzle with exchangeable tiles](/TImages/Cross3x3.png)
**Block-style.**  These are puzzles consisting of solid-colour
block tiles, which are more “Rubic’s cube” style.  Usually, block-style puzzles have at least a few tiles of the same colour, which are considered identical; if there are 4 red tiles, then any of them can be placed anywhere a red tile is needed.  Identical tiles are strictly *exchangeable.*  In some block-style puzzles, all the blocks are different colours, and each one has exactly one right place to be.

<br/>
![Image-style puzzle with unique tiles](/TImages/Stars3x3.png)
**Image-style.** These puzzles come from splitting a given image into rows and columns,
which is more like a jigsaw puzzle.   In these puzzles, every tile is *unique;* there are no exchangeable tiles at all.  Many of the images in my image-style puzzles were created using 
[Diffusion Bee](https://diffusionbee.com), which is
an implementation of [Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release) 
ported to macOS.  Sometimes Diffusion Bee produced an image that I felt was interesting 
texturally, but I had to add a bit of colour to make the tiles visually distinctive.  I don't consider it sporting to have an image-style puzzle with tiles that look identical, but are actually unique by virtue of the way an image is split up into tiles.  Sometimes I took a texture from DiffusionBee to add to a vector-graphics pattern to add visual interest.  I don't consider myself truly educated in visual design.  I just added interest or colour according to my own uneducated taste.

## Difficulty
**Solvable by design.**   My puzzle-creation script starts with the tiles in their goal location, and it chooses moves at random 
to scramble the tiles.  Currently, the random moves 
are constrained so that when a random move is chosen, if it immediately 
reverses the effect of the previous move, the move is discarded, and a new random move is chosen. 

**Multiple solutions.** There are always multiple ways to solve any given puzzle.  This means that there are never any dead ends; only detours.  

<br/>![Larger image-style puzzle](/TImages/TexturedQuart5x5.png)
**Length of solution.**  In general, there will be at least one sequence of moves that solves a given puzzle that is shorter than all other solutions; I will call this a *minimal solution.*  The minimum depends on the size of the puzzle, the number of exchangeable tiles, and the number of random moves applied when creating the puzzle.  Often there will be many distinct minimal solutions all having the same length.  In a general sense, a puzzle is more difficult than another if the minimal solution is longer.    On the other hand, the length of a solution is not always a critical aspect of difficulty. I have found that larger puzzles, e.g., 5x5,  don't really feel more difficult to solve.  They just require more moves.  A longer walk in the woods.  I have also created puzzles where the solution is fairly short, and the objective is to find that shortest solution, and this can be challenging in a different way.

<br/>
![Image-style puzzle with one pair of tiles swapped](/TImages/Puzzle4x41S.png)
**Even Dimensions.**  Puzzles that have *at least* one even dimension (e.g., 3x4) can arrive at a configuration that feels like a dead-end.  To be precise, in a puzzle with at least one even dimension, it is common to have the puzzle nearly completely solved, with exactly one pair of tiles out of place, as shown in the image to the left.  The two tiles highlighted with red outline are side-by-side, and out of place; the rest of the puzzle is solved.  If you could just lift the tiles out of the grid and swap them, they would be in the right place.  This configuration is still solvable, but the number of moves needed to swap the tiles is unexpectedly long.  This situation only occurs when there is at least one even dimension, and all tiles are unique.  I will say more about this in a separate posting. 

<br/>![Image-style puzzle with only local structure](/TImages/Stairs3x3.png)
**Visual distinctiveness.**  In another sense, a puzzle is more difficult if the tiles are not visually distinctive.  It takes a careful eye to put the pieces together, like a jigsaw puzzle.  I have found that puzzles with a global structure, like the image-style red and blue puzzle with white stars shown above, are easiest.  If the image features are local, such as the image-style puzzle on the right, it's harder to visualize how the pieces will fit together.  


<br/>
## More discussion
**Playing the game.** I will post a couple of entries on techniques that a human player can use to solve Toroidal.  But after that, I don't think I will say too much about playing the game itself.  

**JavaScript implementation.**  To implement this game, I had to learn a bit more about JavaScript, CSS, and HTML.  I made a deliberate effort to put into practice some of the software engineering concepts I have gotten exposed to during my academic career.  I am not a professional developer with a lot of training in software engineering.  I know just enough about this kind of thing to feel ridiculous about my lack of practical software engineering knowledge.  So I don't think I will write much about the implementation.  I have some plans to revise the way puzzles are stored, and a few other game like things, like remembering personal bests for each player.  I have a few interesting puzzle variations to explore, as well. 

**Toroidal as a search problem.**  I do have a bit to say about the other end of this puzzle, which is the application of "classical" Artificial Intelligence to this problem, to come up with a computer Toroidal solver.  That will be the major topic for this part of my writing about Toroidal. 

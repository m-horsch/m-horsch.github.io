---
layout: post
title:  "The Puzzles"
date:   2023-07-12 
---
<style>
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>


**Size.**  Currently (as of this date), the game selects a random puzzle
from my library of about 100 puzzles.   A large proportion of puzzles
are 3x3 grids, though there are 3x4, 4x3, 4x4, and 5x5 puzzles in the
current collection.

![Block-style puzzle with exchangeable tiles](/TImages/Cross3x3.png)
**Block-style.**  These are puzzles consisting of solid-colour block
tiles, which are more “Rubik’s cube” style.  Usually, block-style
puzzles have at least a few tiles of the same colour, which are
are strictly *exchangeable.*  For example, if there are 4 red tiles,
then any of them can be placed anywhere a red tile is needed.  In some 
block-style puzzles, all the blocks are
unique, and each one has exactly one right place to be.

<br/> 
![Image-style puzzle with unique tiles](/TImages/Stars3x3.png)
**Image-style.** These puzzles come from splitting a given image into
rows and columns, which is more like a jigsaw puzzle.   In these
puzzles, every tile is *unique;* there are no exchangeable tiles at all.
Many of the images in my image-style puzzles were created using
[Diffusion Bee](https://diffusionbee.com), which is an implementation of
[Stable Diffusion](https://stability.ai/blog/stable-diffusion-public-release)
ported to macOS.  Sometimes Diffusion Bee produced an image that I felt
had an interesting texture, but I had to add a bit of colour to make the
tiles visually distinctive.  I don't consider it sporting to have an
image-style puzzle with tiles that look identical, but are actually
unique by virtue of the way an image is split up into tiles.  Sometimes
I took a texture from DiffusionBee to add to a vector-graphics pattern
to add visual interest.  

## Difficulty
**Solvable by design.**   My puzzle-creation script starts with the
tiles in their goal locations, and it chooses moves at random to scramble
the tiles.  Currently, the random moves are constrained so that when a
random move is chosen, if it immediately reverses the effect of the
previous move, the move is discarded, and a new random move is chosen.

**Multiple solutions.** There are always multiple ways to solve any
given puzzle.  This means that there are never any dead ends; only
detours.

<br/>
![Larger image-style puzzle](/TImages/TexturedQuart5x5.png)
**Length of solution.**  In general, there will be at least one sequence
of moves that solves a given puzzle that is shorter than all other
solutions; I will call this a *minimal solution.*  The minimum depends
on the size of the puzzle, the number of exchangeable tiles, and the
number of random moves applied when creating the puzzle.  Often there
will be many distinct minimal solutions all having the same length.  In
a general sense, a puzzle is more difficult than another if the minimal
solution is longer.    On the other hand, the length of a solution is
not always a critical aspect of difficulty. I have found that larger
puzzles, e.g., 5x5,  don't really feel more difficult to solve.  They
just require more moves.  A longer walk in the woods.  I have also
created puzzles where the solution is fairly short, and the objective is
to find that shortest solution, and this can be challenging in a
different way.

<br/> 
![Image-style puzzle with one pair of tiles swapped](/TImages/Puzzle4x41S.png) 
**Even Dimensions.**  Puzzles that have *at least* one even dimension (e.g., 3x4) can arrive at a configuration that feels like a dead-end.  To be precise, in a puzzle
with at least one even dimension, it is common to have the puzzle nearly
completely solved, with exactly one pair of tiles out of place, as shown
in the image to the left.  The two tiles highlighted with red outline
are side-by-side, and out of place; the rest of the puzzle is solved. 
If you could just lift the tiles out of the grid and swap them, they
would be in the right place.  

This configuration can arise in image-based or block-based puzzles, and
is still solvable.  For block-based puzzles, with some exchangeable tiles, 
it's usually pretty easy to manage without any special strategies.  However, for image-based puzzles, where no tiles are exchangeable, solving this configuration 
is difficult, because the number of moves needed to swap the two tiles is unexpectedly long.  This situation only occurs when there is at least one even dimension, and all
tiles are unique.  I will say more about this [in a separate posting](/tutorial/1Swap.html).

<br/>
![Image-style puzzle with only local structure](/TImages/Stairs3x3.png) 
**Visual distinctiveness.**  In
another sense, a puzzle is more difficult if the tiles are not visually
distinctive.  It takes a careful eye to put the pieces together, like a
jigsaw puzzle.  I have found that puzzles with a global structure, like
the image-style red and blue puzzle with white stars shown above, are
easiest.  If the image features are local, such as the image-style
puzzle on the right, it's harder to visualize how the pieces will fit
together.

<br/>
**Looking forward.**
In the next posting, I'll outline the [topics I'll be writing about](/introduction/topics.html).
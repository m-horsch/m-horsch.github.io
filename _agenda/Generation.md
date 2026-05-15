---
layout: post
title:  "Toroidal Puzzle Generation"
date:   2023-09-06
---
It's almost too easy to randomly shuffle a Toroidal, so I have given little thought to it.  But I know from other kinds of puzzles that the obvious easy technique can leave biases in the output.  For example, Latin Squares (which are similar to Sudoku puzzles) can be created by taking a simple arrangement, and randomly shuffling rows and columns.  And it is known that doing so does not created puzzles that uniformly cover the space.  A citation would be helpful, but it's been 25 years, and I haven't taken time yet to track down that reference.  

I know this is a problem for me because every 4x4 Toroidal I have solved has a solution with an even number of moves.  This is not a coincidence, because my generator typical uses an even number of random moves to create each puzzle.  But I have no idea whether my generator samples unevenly from the space of possible puzzle.

The script to generate grids of numbers is basic.  The script to generate puzzles for the game was a total unmitigated kludge, and no longer functions.  

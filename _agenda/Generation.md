---
layout: post
title:  "Toroidal Puzzle Generation"
date:   2026-05-29
---
For my solvers, a sample puzzle is nothing more than a long list of numbers.  The solver doesn't benefit from images or blocks, and so the data for solvers is very simple.

The Toroidal game itself requires each puzzle to have certain extra data, including the location of the image tiles, the goal and start positions, and other information.  Naturally I created a script to put all this information together, so that the Toroidal game can read the data, and present everything to the player.

The script to generate puzzles for the game was a total unmitigated kludge, and no longer functions.  I also have plans to improve the way I store the extra data needed for the game. 

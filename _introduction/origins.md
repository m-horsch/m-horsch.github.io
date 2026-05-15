---
layout: post
title:  "Why Toroidal?"
date:   2023-07-14
---
<style>
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>


**Origins.**
A number of years ago, I needed a new search problem for my 3rd year Intro AI class to sink their teeth into.  Most of the problems that are given by profs in such classes actually come from textbooks, or from the AI research literature.  The [sliding puzzle](https://en.wikipedia.org/wiki/Sliding_puzzle) had long been used in AI classes as an example.  I had also been using Latin Squares, Sudoku, and other grid-based puzzles.  

As a result of the common use of these examples, one can find solutions, including code in any programming language, posted (and reposted) all over the Web.  Only a small proportion of students practice plagiarism at the 3rd year level, but it seems appropriate for a prof to find problems that students cannot simply solve by copy/paste.

I can't recall exactly how the Toroidal idea come to mind, but I recognized it as a variant of the Rubik's Cube immediately.  Since the most important consideration for me at the time was *novelty,*  I did a bit of Googling.  I couldn't find anything related to this idea.  I may not be using the most appropriate search terms, but I was as diligent as I felt any undergraduate student would be.


**The name.**  When I first described the problem to my students, I had not chosen the name Toroidal.  I called it a *Torus Square* in my first year class, and as a joke, I referred to it in my AI class as *The Kessel Run Problem.*  I frequently rename problems in an absurd or nonsensical way, to frustrate the small minority of students who rely too heavily on Google.  

When I finally took the time during my retirement to turn the concept into a puzzle game, I had already been playing [Wordle](https://www.nytimes.com/games/wordle/index.html)  for about a year.  I liked the idea of a short game that would change on a daily basis.  The name "Toroidal" both describes the fundamental nature of the puzzle, and alludes to the name "Wordle."


**The implementation.**
My first implementation of a playable Toroidal game was a command-line version written in Java.  The tiles were numbers, and these were displayed in a simple grid on a console window.  The player would type in a move, e.g. `L1`, and then the game engine would apply the move and display the resulting grid.  I thought it would be an easy transition to move to a graphics based implementation.  It really didn't help much, as only the very simplest parts of the model were reused.    

![Screenshot of the Original Java Swing Implementation](/TImages/JavaSwingT.png){:width="35%"}
Eventually, I worked out a Java Swing implementation with the grid displayed in a Swing frame.  That version is shown on the left.  The yellow square frame can be moved by the cursor keys, and when SHIFT is held down, the cursor keys would move the row or column that the yellow square was on.  

I restarted the whole project in JavaScript, knowing very little about JavaScript, and nothing really about how to animate moving tiles, or respond to mouse drags and touch screen touches.  The first few stages of the implementation were accelerated through the use of ChatGPT, which gave me some good advice, particularly about JavaScript's interaction with HTML5.  Moving to JavaScript also allowed me to get a bit more creative with Puzzle visuals.  I built hundreds of Toroidal instances over the course of a few months, varying the image data, the number of tiles, and the amount of randomization.
<br/>

**The research.**
I developed the puzzle game first, but I was really more interested in the problem of solving Toroidal puzzles.  I had learned from my 3rd year students that the puzzle was harder than it seemed at first.  It was a surprise to me that Toroidal resisted simple applications of the techniques I had been teaching for decades.  I really wanted to know how hard Toroidals are to solve.

I was aware that there had been some research on algorithms that enabled computers to solve Rubik's Cube, but I had never looked deeply into them.  It should not be surprising to learn that digging into this literature a little helped me a lot, and there are a few ideas that I have not yet turned into implementations.  Let's see how many I can get to!

[Back to the main page.](\index.html)
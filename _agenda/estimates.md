---
layout: post
title:  "Finding more accurate distance estimates"
date:   2025-12-26
---

### Outline
  * I've used the following approaches to estimate distances in Toroidal.
    * Misplaced Tile Count (MTC)
    * Total Manhattan Distance (TMD)
    * Several variations of these, using division
  * I've considered, but not yet tried, building machine learning models to learn distance
  * As given, the toroidal state space has loops, because tiles wrap on the edges.
  * However, one can imagine the Toroidal grid as being a "window" on an infinite grid with one dimension for each row and column.  In this space, the manhattan distance is true.  There are many locations on this "grid" that look similar, but are described by different paths.
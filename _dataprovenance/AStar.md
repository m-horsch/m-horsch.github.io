---
layout: post
title:  "Data provenance: Prioritizing Promising Sequences"
date:   2025-12-28
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

Revised: 2026-08-13

### Cross-references
This document describes the provenance of the data presented in the following article:
  * [Prioritizing Promising Sequences](/solver/AStar)

### Input Data
  * `Data/Provenance/Common_Inputs/3x3_N250_K50.txt`
  * $$N=250, K=50$$, no cycle checking   
  
### Solver configuration
  * `Data/Provenance/AStar/AStar.sh Manhattan`
  * `Data/Provenance/AStar/AStar.sh ManCeil`

### Output data
  * `Data/Provenance/AStar/2026-08-13_AStar_Manhattan_Symmetric_3x3_N250_K50.txt`  
  * `Data/Provenance/AStar/2026-08-13_AStar_ManCeil_Symmetric_3x3_N250_K50.txt`

### Notes:
  * Python version: 3.13.13
  * Original data reported slightly slower times, based on a smaller dataset, and earlier version of Python.
  

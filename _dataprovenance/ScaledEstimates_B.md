---
layout: post
title:  "Data provenance: title: Preventing useless sequences for greedy search"
date:   2025-12-21
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

Revised: 2026-08-12

### Cross-references
This document describes the provenance of the data presented in the following article:
  * [ Preventing useless sequences for greedy search](/solver/ScaledEstimates_B)

### Input Data
  * `Data/Provenance/Common_Inputs/3x3_N250_K50.txt`
  * $$N=250, K=50$$, no cycle checking   

### Solver configuration
  * `Data/Provenance/DistanceEstimates/Greedy.sh Symmetric ManFloor`
  * `Data/Provenance/DistanceEstimates/Greedy.sh Symmetric ManVCeil`

### Output data
  * `Data/Provenance/DistanceEstimates/2025_12_21_hval_tables.txt`  
  * `Data/Provenance/DistanceEstimates/2026-08-12_Greedy_ManFloor_Symmetric_3x3_N250_K50.txt`
  * `Data/Provenance/DistanceEstimates/2026-08-12_Greedy_ManCeil_Symmetric_3x3_N250_K50.txt`

### Notes:
  * Python version: 3.13.13
  * Original data reported slightly slower times, based on a smaller dataset, and earlier version of Python.

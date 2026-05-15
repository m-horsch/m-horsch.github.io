---
layout: post
title:  "Python Version Matters"
date:   2026-04-17
---
<style>
table
{
    max-width: 0px;
    margin-left:auto; 
    margin-right:auto;  
}
</style>

My experiments to date have all been done with Python 3.10.10.

I had been doing some refactoring and tidying up in my codebase.  When I compared the output of the new design to the output from the previous design, I noticed that the new version was so much faster.  As in, solving times cut in half, better.  I haven't confirmed this across the board yet. 

It took me a while to figure out why there was such a speedup.  I didn't change any of the core solving modules.  I was working on the code that glues the solving modules together.  
The difference in speed had nothing to do with my refactoring (that's fine).  The speedup was due to accidentally using a different version of Python. 

I was going to re-run my experiments anyway.  But I was hoping to avoid revising the articles that report on the experiments.  

---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
<style>
img
{
    display:inline-block;
    float:left;
    margin-right:15px;
}
</style>

# What is Toroidal?

![Screenshot of Mike's Toroidal game](/TImages/Screenshot300.png)
**Toroidal** is the name I have given to [a puzzle game]({{ site.toroidal_url }})
I have been working on in my retirement.  The game challenges the player to
rearrange a rectangular grid of image tiles, to produce an image of some
kind.  This site is devoted to the implementation of the game and the study 
of its solution. The sections below provide a guided tour of my writings on the topic.
<br/><br/>

<br/>
# Introduction to Toroidal
The following articles provide a more thorough (non-technical) introduction.

<OL>
{% for tdoc in site.introduction %}
  <li>
    <a href="{{ tdoc.url }}">
      {{ tdoc.title }}
    </a>
  </li>
{% endfor %}
</OL>



# How to Solve Toroidal for Humans
The following articles describe a method that beginners can learn to solve 
Toroidal puzzles.  

<OL>
{% for tdoc in site.tutorial %}
  <li>
    <a href="{{ tdoc.url }}">
      {{ tdoc.title }}
    </a>
  </li>
{% endfor %}
</OL>

# Computer Algorithms to Solve Toroidal

The following articles describe my exploration of Classical AI algorithms to solve 
Toroidal.  I tried to keep these descriptions non-technical, and accessible to 
non-specialists.  The articles are intended to be read sequentially, but I've tried to give at least a little context at the start, so you probably can jump in anywhere.
<OL>
{% for sdoc in site.solver %}
  <li>
    <a href="{{ sdoc.url }}">
      {{ sdoc.title }}
    </a>
  </li>
{% endfor %}
</OL>


# Technical details

Short articles about technical matters and results, presented in a way that can be easily found.
<OL>
{% for xdoc in site.technical %}
  <li>
    <a href="{{ xdoc.url }}">
      {{ xdoc.title }}
    </a>
  </li>
{% endfor %}
</OL>

# Assorted Commentary

Various short articles and footnotes.  Some of these might be a bit technical.
<OL>
{% for cdoc in site.commentary %}
  <li>
    <a href="{{ cdoc.url }}">
      {{ cdoc.title }}
    </a>
  </li>
{% endfor %}
</OL>

# My To-Do List

What's on the agenda.  The order reflects when the topics were added, but not necessarily their importance.
<OL>
{% for adoc in site.agenda %}
  <li>
    <a href="{{ adoc.url }}">
      {{ adoc.title }}
    </a>
  </li>
{% endfor %}
</OL>

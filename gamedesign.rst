===========
Game Design
===========

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


Overview
========

Time and sequencing are often a core part of a game’s architecture.

If two pieces of code are coupled, it means you can’t understand one without understanding the other. 
If you decouple them, you can reason about either side independently. 

Many patterns that make your code more flexible rely on virtual dispatch, interfaces, pointers, messages and other mechanisms that all have at least some runtime cost.

Writing code to do what you want is actually pretty easy. 
What’s hard is writing code that’s easy to adapt when what you want changes. 

Software Architecture 101 tells us that different domains in a program should be kept isolated from each other.

Patterns
========

- reward
  observer: subject notifies observers synchronously

- event
  command

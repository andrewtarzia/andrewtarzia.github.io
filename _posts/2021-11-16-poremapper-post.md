---
layout: archive
title: 'PoreMapper: Python project for mapping molecular pores'
date: 2021-11-06
permalink: /posts/2021/11/poremapper-post/
author_profile: true
---

A quick and easy Python package for making blobs and pores
------

Why?
------

I work mostly with metal-organic cages and, in that field, the pore is often visualised with a software called VOIDOO, which makes very pretty images. Simply, I wanted to recreate that, quickly (just for visualisation) and in Python (to interface with an [stk](https://stk.readthedocs.io/en/stable/) molecule). I initially planned on performing cheap dynamics on what I termed a ``Blob`` inside the cage, allowing it to mould to the interior surface. However, that was overcomplicating a relatively simple solution: just inflate a balloon inside the pore! 🎈🎈


How?
------

I will start by noting that a lot of the more frustrating parts of this code were taken directly for [pyWindow](https://github.com/marcinmiklitz/pywindow), which was developed by Marcin Miklitz during his time in the Jelfs group. I thank him for saving me some time! Additionally, I must thank Austin Mroz (*URL*), a postdoc in the Jelfs group, who shared with me the atomic radii from her recent work: *URL*. These radii are used for all ``Atom``s in the ``Host`` class.

Introduce classes.

Decribe inflation procedure.

Decribe how a pore is made from a blob.

Describe window detection here.


Code examples here:



Examples?
------

Show blowing up in CC3, with time series - kind of useful.

Blobs provide a window pathway.

Show a non porous example.

Show the Nitschke series.. Careful of resolution here.



Limitations?
------

Window detection.

the window calculation is most costly currently

Resolution/cost.

Show MOC2 -- clustering is not perfect, warning! pyWindow is better! - Covers both limits.

What next?
------

This package is simple and after spending a short amount of time writing it and being very happy with the outcome, for visualisation at least, I decided that merging it with pyWindow (more code from the Jelfs group: [pyWindow](https://github.com/marcinmiklitz/pywindow)) would be best. This would include rewrite/clean-up/improvements/bug-fixes to pyWindow, which I hope to report on soon!

Please, test it, use it, break it and send me feedback!

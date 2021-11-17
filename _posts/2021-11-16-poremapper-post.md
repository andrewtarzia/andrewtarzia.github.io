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

PoreMapper provides a ``Host`` class, which reads in XYZ coordinates and atom types: this is the cage of interest. The calculation is handled by the ``Inflater`` class, which takes only one argument: the ``bead_sigma`` or radius of the `Bead`s that will be used to map the pore. The ``Inflater`` has two methods: `get_inflated_blob` and `inflate_blob`, which perform the same process, except `get_inflated_blob` only yields the final result, not each step.

The process begins with a `Blob` made up of `Bead`s in an idealised spherical geometry with radius of 0.1 angstrom and a number of beads defined by th size of the `Host`. Currently the `Blob` is placed at the centroid of the `Host`. Over 100 steps, the position of each `Bead` in the `Blob` is translated outwards in small increments until the maximum diameter of the `Host` is reached. If at any point a `Bead` is within contact distance (bead radii + atom radii) of a `Host` atom, it is no longer moved in following steps and added to the `Pore` class (a subset of beads in the initial `Blob`). The output of the calculation is an `InflationResult`, which contains the `Blob` and `Pore` for futher analysis.

A `Blob` provides pathways out of the molecule, which we analyse, using simple clustering of points, to find the windows of the `Host`. This process is currently limited (see below) and pyWindow is recommended! Regardless, the visualisation of the pathways, and collection of those coordinates, may be useful:

![Inflation of a blob inside the porous organic cage, CC3](/assets/cc3blob.png)

A `Pore` provides a visualisation of the inside of `Host` and the class provides analyses including `get_mean/max_distance_to_com` for pore radii, `get_volume` for pore volume and `get_asphericity` for pore shape. Again, the key for me was visualisation:

![Inflation of a pore inside the porous organic cage, CC3](/assets/cc3pore.png)

Most importantly, PoreMapper is simple to use in a Python project! Here is a code example of running a simple analysis of a host (CC3):

FIGURE with code

Examples?
------

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

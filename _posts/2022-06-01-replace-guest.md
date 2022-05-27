---
layout: archive
title: 'A script to replace guests in complexes'
date: 2022-06-01
permalink: /posts/2022/06/replace-post/
author_profile: true
---

A script to swap hosts and guests in complexes
------

Why?
------

I work mostly with metal-organic cages and, in that field, the pore is often visualised with a software called VOIDOO ([see here](https://pubmed.ncbi.nlm.nih.gov/15299456/)), which makes very pretty images. I wanted to recreate that, quickly (just for visualisation) and in Python (to interface with an [_stk_](https://stk.readthedocs.io/en/stable/) molecule). I initially planned on performing cheap dynamics on what I termed a ``Blob`` inside the cage, allowing it to mould to the interior surface. However, that was overcomplicating the issue. Instead, just inflate a balloon inside the pore!



How?
------

`NetworkX` is very powerful here. The entire script in the examples directory is shown below.





I will start by noting that a lot of the more frustrating parts of this code were taken directly for [pyWindow](https://github.com/marcinmiklitz/pywindow), which was developed by Marcin Miklitz during his time in the Jelfs group. I thank him for saving me some time! Additionally, I must thank Austin Mroz, a postdoc in the Jelfs group, who shared with me the atomic radii from their recent work: [STREUSEL](https://github.com/hmsoregon/STREUSEL) (paper [here](https://chemrxiv.org/engage/chemrxiv/article-details/61b284180e35ebcbb19bba3b)). These radii are used for all ``Atom``s in the ``Host`` class.

PoreMapper provides a ``Host`` class, which reads in XYZ coordinates and atom types: this is the cage of interest. The calculation is handled by the ``Inflater`` class, which takes only one argument: the ``bead_sigma`` or radius of the `Bead`s that will be used to map the pore (i.e., resolution). The ``Inflater`` has two methods: `get_inflated_blob` and `inflate_blob`, which perform the same process, except `get_inflated_blob` only yields the final result, not each step.

The process begins with a `Blob` made up of `Bead`s in an idealised spherical geometry with radius of 0.1 Angstrom and a number of beads defined by the size of the `Host`. The `Blob` is placed at the centroid of the `Host`. Over 100 steps, the position of each `Bead` in the `Blob` is translated outwards in small increments until the maximum diameter of the `Host` is reached. If at any point a `Bead` is within contact distance (bead radii + atom radii) of a `Host` atom, it is no longer moved in following steps and added to the `Pore` class (a subset of beads in the initial `Blob`). The output of the calculation is an `InflationResult`, which contains the `Blob` and `Pore` for futher analysis.

A `Blob` provides pathways out of the molecule, which we analyse, by clustering the points (`MeanShift` usage from `sklearn`), to find the windows of the `Host`. This process is currently limited (see below) and **pyWindow** is recommended! Regardless, the visualisation of the pathways, and collection of those coordinates, may be useful. This figure shows the inflation of the blob in CC3:


A `Pore` provides a visualisation of the inside of `Host` and the class provides analyses including `get_mean/max_distance_to_com` for pore radii, `get_volume` for pore volume and `get_asphericity` for pore shape. Again, the key for me was visualisation. This figure shows the inflation of the pore in CC3:



Most importantly, PoreMapper is easy to use in a Python project! Here is a code example of running an analysis of a host from an XYZ file:

```python
import pore_mapper as pm

# Read in host from xyz file.
host = pm.Host.init_from_xyz_file(path=f'{name}.xyz')
host = host.with_centroid([0., 0., 0.])

# Define calculator object.
calculator = pm.Inflater(bead_sigma=1.0)

# Run calculator on host object, analysing output.
final_result = calculator.get_inflated_blob(host=host)

# Analysis.
windows = final_result.pore.get_windows()
print(f'windows: {windows}, pore_volume: {pore.get_volume()}') 

# Write final structure.
host.write_xyz_file(f'{name}_final.xyz')
final_result.pore.get_blob().write_xyz_file(f'{name}_blob_final.xyz')
final_result.pore.write_xyz_file(f'{name}_pore_final.xyz')

```

Examples and limitations.
------

Currently, the provided script swaps out the smallest molecule in a complex for the new molecule (defined from `.mol` files). However, the tutorial below shows that we can swap the host or guest for any `stk` molecule. Additionally, we could easily extend this to systems with more than two distinct molecules. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/SeSRnG4LbnE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jupyter notebooks are available [here](https://github.com/andrewtarzia/stk-examples)


**Please, test it, use it, break it and send me feedback!**

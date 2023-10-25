---
layout: archive
title: "software"
permalink: /software/
author_profile: true
---

## structure generation of (supra)molecular materials

I am a developer of [<i>stk</i>](https://github.com/lukasturcani/stk) for the generation of molecular and periodic material structures. I recently put together a play list of tutorials on <i>stk</i> usage: [<i>stk</i> tutorials](https://www.youtube.com/watch?v=mPr9D7nCQ84&list=PLIWYdPQ9hLzVngMF8NOkiApMtgc_ZwZgO) and have some examples of <i>stk</i> usage [here](https://github.com/andrewtarzia/stk-examples).

I have some code available on [GitHub Gists](https://gist.github.com/andrewtarzia).

[<i>stko</i>](https://github.com/JelfsMaterialsGroup/stko) contains wrappers for optimisation and property calcualtion of <i>stk</i> molecules

I have also developed some low-cost Monte Carlo-based algorithms for optimisation <i>stk</i> molecule optimisation ([MCHammer](https://github.com/andrewtarzia/MCHammer)) and host-guest conformer generation/optimisation ([SpinDry](https://github.com/andrewtarzia/SpinDry)).

## preparing building blocks for structure generation

I recently developed [bbprepared](https://github.com/andrewtarzia/bbprepared) that contains algorithms and tests (examples) for preparing building blocks for <i>stk</i> usage.

[Here](https://youtu.be/dbQwhlpf5Jc) is a video on its usage.

## coarse-grained structure generation

[CGExplore](https://github.com/andrewtarzia/CGExplore) is a library for using <i>stk</i> with coarse-grained molecules to afford more efficient high-throughput screening and rational design. This library is under-development but it's first usage is written up [here](https://chemrxiv.org/engage/chemrxiv/article-details/64c7aed1658ec5f7e57cf4d8).


## calculating molecular size

[mol-ellipsize](https://github.com/andrewtarzia/mol-ellipsize) is an efficient method for calculating molecular size based on fitting an ellipsoid to the vdw cloud of a molecule. Examples are provided within the github repository. See a blog post [here](/posts/2022/01/molellipsize-post/).


## visualising molecular pores

[PoreMapper](https://github.com/andrewtarzia/PoreMapper) is a very efficient way to map the inside of a molecular pore for visualisation and analysis. See a blog post [here](/posts/2021/11/poremapper-post/).

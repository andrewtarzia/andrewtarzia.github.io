---
layout: archive
title: "software"
permalink: /software/
author_profile: true
---

# open-source structure generation of (supra)molecular materials

I am a developer of [<i>stk</i>](https://github.com/lukasturcani/stk) for the generation of molecular and periodic material structures. I recently put together a play list of tutorials on <i>stk</i> usage: [<i>stk</i> tutorials](https://www.youtube.com/watch?v=mPr9D7nCQ84&list=PLIWYdPQ9hLzVngMF8NOkiApMtgc_ZwZgO).

Additional software packages I develop that work in tandem with <i>stk</i>:
* [<i>stko</i>](https://github.com/JelfsMaterialsGroup/stko) : Wrappers for optimisation and property calcualtion of <i>stk</i> molecules
* [MCHammer](https://github.com/andrewtarzia/MCHammer) : Efficient MC-based optimisation of chemical structures based on alchemical bond and nonbonded potential functions (implemented into <i>stk</i> as an Optimizer class)
* [SpinDry](https://github.com/andrewtarzia/SpinDry) : Efficient MC-based host-guest conformer generation of <i>stk</i> molecules based on alchemical potential functions

I also have some examples of <i>stk</i> usage [here](https://github.com/andrewtarzia/stk-examples).


# calculating molecular size

[mol-ellipsize](https://github.com/andrewtarzia/mol-ellipsize) is an efficient method for calculating molecular size based on fitting an ellipsoid to the vdw cloud of a molecule. Examples are provided within the github repository.

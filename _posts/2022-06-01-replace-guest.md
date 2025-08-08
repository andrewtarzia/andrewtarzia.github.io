---
layout: archive
title: 'A script to replace guests in complexes'
date: 2022-06-01
permalink: /posts/2022/06/replace-post/
author_profile: true
---

Why?
------

Simply put, a co-worker asked if `stk` could swap one guest for another in a host-guest complex (regardless of chemistry), and I already had some code for doing it, but thought it is pretty useful for everyone. So, here we are!


How?
------

The entire script in the examples directory is shown below and is available, alongside Jupyter notebooks used in the video below, [here](https://github.com/andrewtarzia/stk-examples).
This process uses `stk` to read the host-guest and new guest molecules. 
But, [`NetworkX`](https://networkx.org/) is the real hero here!
Basically, we convert the `stk.Molecule` class to a `NetworkX.graph` based on the bonds and atoms in the molecule.
Then, use the `NetworkX.connected_components(graph)` to get atoms that are not bonded (e.g. host and guest molecules).
The rest is simple: a helper function to collect either the biggest (host) or smallest (guest) component, and then build a new `stk.host_guest.Complex` `ConstructedMolecule` from the host and the new guest.
A reasonable conformer is produced using `SpinDry` (`stk.Spinner`).
What I like about this is that `stk` does not care about what the host and guest actually are - so use your imagination about what structural replacements you can do!


```python
import stk
import networkx as nx
import sys
import os


def get_disconnected_components(molecule):

    # Produce a graph from the molecule that does not include edges
    # where the bonds to be optimized are.
    mol_graph = nx.Graph()
    for atom in molecule.get_atoms():
        mol_graph.add_node(atom.get_id())

    # Add edges.
    for bond in molecule.get_bonds():
        pair_ids = (
            bond.get_atom1().get_id(), bond.get_atom2().get_id()
        )
        mol_graph.add_edge(*pair_ids)

    # Get atom ids in disconnected subgraphs.
    components = {}
    for c in nx.connected_components(mol_graph):
        c_ids = sorted(c)
        molecule.write('temp_mol.mol', atom_ids=c_ids)
        num_atoms = len(c_ids)
        newbb = stk.BuildingBlock.init_from_file('temp_mol.mol')
        os.system('rm temp_mol.mol')

        components[num_atoms] = newbb

    return components


def extract_host(molecule):
    components = get_disconnected_components(molecule)
    return components[max(components.keys())]


def extract_guest(molecule):
    components = get_disconnected_components(molecule)
    return components[min(components.keys())]


def main():
    if (not len(sys.argv) == 3):
        print(
            f'Usage: {__file__}\n'
            '   Expected 2 arguments: host_with_g_file new_guest_file'
        )
        sys.exit()
    else:
        host_with_g_file = sys.argv[1]
        new_guest_file = sys.argv[2]

    # Load in host.
    host_with_guest = stk.BuildingBlock.init_from_file(host_with_g_file)
    
    # Load in new guest.
    new_guest = stk.BuildingBlock.init_from_file(new_guest_file)
    
    # Split host and guest, assuming host has more atoms than guest.
    host = extract_host(host_with_guest)
    old_guest = extract_guest(host_with_guest)
    
    # Build new host-guest structure, with Spindry optimiser to 
    # do some conformer searching.
    new_host = stk.ConstructedMolecule(
        stk.host_guest.Complex(
            host=stk.BuildingBlock.init_from_molecule(host),
            guests=(stk.host_guest.Guest(new_guest), ),
            # There are options for the Spinner class, 
            # if the optimised conformer is crap.
            optimizer=stk.Spinner(),
        ),
    )
    
    # Write out new host guest.
    new_host.write('new_host_guest.mol')


if __name__ == '__main__':
    main()
```

Examples and limitations.
------

Currently, the provided script swaps out the smallest molecule in a complex for the new molecule (defined from `.mol` files). However, the tutorial below shows that we can swap the host or guest for any `stk` molecule. Additionally, we could easily extend this to systems with more than two distinct molecules. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/J29eoc9CZ98" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


**Please, test it, use it, break it and send me feedback!**

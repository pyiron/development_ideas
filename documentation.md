# Documentation

Documentation is to be made for developers and users.

## Documentation for the functions in the node library (`pyiron_nodes`)

Currently the nodes repo is a prototype with a bunch of nodes which have scant to no documentation. Going forward this would have to change to ensure better usability of the functions and to meet FAIR standards. Documentation will also be necessary for publication of data generated by these nodes in various platforms like openBIS. Thus, good documentation practices must be enforced keeping in mind that the nodes repo would be a community driven effort and not just a gated-community for developers. 

An [example](https://docs.lammps.org/fix_nh.html) of good documentation for scientifically relevant functions would be the LAMMPS docs. LAMMPS also has [glossary pages](https://docs.lammps.org/Commands_fix.html) for various categories. Perhaps these can be used as role models for our own documentation. The content for the web-based documentation could come from well written docstrings:
- Explanations of the inputs and outputs from a scientific perspective, conventions (e.g., meaning of positive and negative values), and any typing restrictions
- Examples of inputs if necessary
- Description of what exactly the function is doing and how it is expected to fit in a workflow (scientific and/or - mostly "and" - technical perspective)
- Relevant references with DOIs if any

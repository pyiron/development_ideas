# Specs 
We define specs to explain the structure of the pyiron project with a specific focus on how the individual components of pyiron interact with each other. pyiron in this case refers to both `pyiron_workflow` and `pyiron_base` as the two current interfaces we provide as part of the pyiron project. 

On an abstract level we see as specs as defining the interfaces between the building blocks which together form the pyiron project. Peter Naur wrote an article on [programming as theory building](https://pages.cs.wisc.edu/~remzi/Naur.pdf) which highlights that effective extension of an existing program requires more than just the access to the whole code, especially it requires an understanding of the scope of the program and the concepts it is based on. 

# Definitions

- A node is an atomic unit of a workflow which executes an underlying function using one or more inputs to return one or more outputs.

# Concepts [^1]

- pyiron is simple
  - Any python function can be turned into a node in one line [^2]
- pyiron is interoperable
  - As long as the underlying function can run on different platforms, pyiron can run the node on any platform.
- pyiron is HPC-compatible
  - Any node can be submitted to the queueing system
- pyiron keeps data
  - The input and the output of any node can be stored in a file system and loaded after the session is closed
- pyiron allows for collaboration
  - Nodes can be imported via the conventional python `import`-statement
- pyiron is modular 
    - The users should have the option to select which of the pyiron core modules they want to use. 

[^1]: Currently the list does not follow a specific order.
[^2]: Currently not true for functions which might return very different types and has multiple return statements.

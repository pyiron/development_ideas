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
- pyiron tracks the source
  - Users can add the DOI of the underlying methods/software packages to the node

[^1]: Currently the list does not follow a specific order.
[^2]: Currently not true for functions which might return very different types and has multiple return statements.

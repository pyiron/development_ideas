# Concepts

- pyiron is simple
  - Any python function can be turned into a node in one line [^1]
- pyiron is interoperable
  - As long as the underlying function can run on different platforms, pyiron can run the node on any platform.
- pyiron is HPC-compatible
  - Any node can be submitted to the queueing system
- pyiron keeps data
  - The input and the output of any node can be stored in a file system and loaded after the session is closed
- pyiron allows for collaboration
  - Nodes can be imported via the conventional python `import`-statement


[^1]: Currently not true for functions which might return very different types and has multiple return statements.

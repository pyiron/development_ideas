# Definitions

- A node is an atomic unit of a workflow which executes an underlying function using one or more inputs to return one or more outputs.
- The pyiron core modules are the user interfaces `pyiron_xyflow`, `pyiron_workflow` and `pyiron_base` as well as the underlying modules like the newly defined storage module and database module plus `executorlib`.

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

# User Groups
While `pyiron` originally only addressed the atomistics community, the new generation of `pyiron` has a much wider range of potential users. They can be roughly defined into the following groups:

* **performance oriented expert user**: A user interested in maximum performance might just use `executorlib` and develop their own minimalistic storage solution to only store the material properties they calculated without storing the results of individual calculation. 
* **minimalist**: A user who previously used `snakemake` switches to use `pyiron_base` based on its minimalistic syntax which internally uses the newly defined storage module and database module plus `executorlib` and the auto-completion based user interface `pyiron_base` introduced. 
* **workflow expert**: A user interested in graph based workflows but without access to a Jupyter environment on their HPC might use `pyiron_workflow` which internally uses the newly defined storage module and database module plus `executorlib`.
* **beginner user**: A user who has less experience with python programming and enjoys the overview provided by an visual programming interface might use `pyiron_xyflow` which is based on `pyiron_workflow` which internally uses the newly defined storage module and database module plus `executorlib`.

The different user groups use a different selection of pyiron core modules. 

[^1]: Currently the list does not follow a specific order.
[^2]: Currently not true for functions which might return very different types and has multiple return statements.

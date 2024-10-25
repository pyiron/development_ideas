# Database
Even though this spec is called database we hide the backend implementation to the user. This allows us to experiment and opens the possibility to exchange backends at some point if we want to.

## What is it supposed to do?
It stores/restores `pyiron_workflow` nodes, remembering its connections. 
The following data is of interest:
 - input of the node, either the data directly or a link to another node's output
 - output (optional, only if data is expensive to compute)
 - the node itself, meaning its source code directly or some way to restore it
 - date of creation
 - creator
The key features are:
 - It allows to uniquely identify a node.
 - It allows to completely restore a workflow and possibly all outputs.
 - It allows to reuse nodes and subgraphs of nodes in different workflows.
 - It provides usage statistics
   - How often is one specific node used?
   - What nodes did a specific user add and what connections did they make?

## Interface
We do not expose any backend related stuff, e.g. tables. This way we can for example switch between `postgresql` and `neo4j` if we want to.  
`CRUD` for nodes:
 - `CREATE`
 - `READ`
 - `UPDATE`, do we want to have this? Depending on the change might invalidate following nodes.
 - `REMOVE`, we cannot really allow this as it most probably breaks a lot of graphs

## What features are required for this?
 - input and output needs to be serizable (Storage spec?)
 - input needs to be hashable
 - node's logic needs to be hashable

## Technical difficulties
 - uniquely hashing inputs
   - whitespaces
   - order of elements
 - how to restore node's logic
   - reinstantiate it with available class declarations
     - how to keep track of changes
   - using `cloudpickle`
     - allows for arbitary code execution
     - how many dependencies do we also store (size?)

## Open questions
 - What does fair mean in this context? Is the backend required to store all information in human-readable form? performance vs readability.

## Previous discussions
 - https://github.com/pyiron/pyiron_workflow/issues/126
 - prototype implementation by Joerg: [hash_based_db_node_storage.ipynb](https://github.com/pyiron/pyiron_nodes/blob/main/notebooks/hash_based_db_node_storage.ipynb)

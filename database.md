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
 - It allows to reuse nodes and subgraphs of nodes in different workflows in a pre-executed state.
 - It provides usage statistics
   - How often is one specific node used?
   - What nodes did a specific user add and what connections did they make?

## Interface
We do not expose any backend related stuff, e.g. tables. This way we can for example switch between `postgresql` and `neo4j` if we want to.  
`CRUD` for nodes:
 - `CREATE`
 - `READ`
 - `UPDATE`, do we want to have this? Depending on the change this might invalidate following nodes.
 - `REMOVE`, we cannot really allow this as it most probably breaks a lot of graphs

## What features are required for this?
 - input and output needs to be serizable (Storage spec?)
   - First, inefficient attack already available: all nodes are serializable in the context of their graph, so worst-case we can store the graph save file and the semantic path from the graph root to the node in question, such that the node can be accessed by deserializing the entire graph and following the semantic path to the node in question. Hierarchical storage would massively improve efficiency here.
 - input needs to be hashable
   - Outline of attack available in Joerg's [`pyiron_nodes` hashing](https://github.com/pyiron/pyiron_nodes/blob/main/pyiron_nodes/development/hash_based_storage.py), e.g. [chaining hash references until reaching data that is actually hashable](https://github.com/pyiron/pyiron_nodes/blob/bb4a4c8cc4a57c2c028a591131e76c0d19fa3956/pyiron_nodes/development/hash_based_storage.py#L477). May serve only as a conceptual template depending on the "database" back end actually used.
 - node's logic needs to be hashable
   - Where the output cannot be stored in the database, it may be necessary to store a path to the node's serialized storage location to accomplish this  

## Technical difficulties
 - How do we make this opt-in? Workflows should work without it, using it for some nodes, all nodes?
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
 - Can the `pyiron_base` database be integrated into this? If possible, we want to have a single storage solution for `pyiron_base` and `pyiron_workflow`.
 - Can we transfer the existing simulations from `pyiron_base` into this storage concept.

## Previous discussions
 - https://github.com/pyiron/pyiron_workflow/issues/126
 - prototype implementation by Joerg: [hash_based_db_node_storage.ipynb](https://github.com/pyiron/pyiron_nodes/blob/main/notebooks/hash_based_db_node_storage.ipynb)

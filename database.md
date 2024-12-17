# Database
Even though this spec is called database we hide the backend implementation to the user. This allows us to experiment and opens the possibility to exchange backends at some point if we want to.

## What is it supposed to do?
It stores/restores `pyiron_workflow` nodes, remembering its connections. 
The following data is of interest:
 - input of the node, either the data directly or a link to another node's output
 - output (optional, only if data is expensive to compute)
 - the node itself, meaning its source code directly or some way to restore it
 - a human-readable name making it possible to identify what the node does
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
```python
def store_node_outputs(node: Node) -> str:
    """
    Store a node's outputs into an HDF5 file.

    Args:
        node (Node): The node whose outputs should be stored.

    Returns:
        str: The file path where the node's outputs are stored.
    """

def restore_node_outputs(node: Node) -> bool:
    """
    Restore a node's outputs from a stored HDF5 file, given by node.hash.
    
    Args:
        node (Node): the node whose outputs should be restored.
    
    Returns:
        True if the outputs were restored, False if not.
    """

def store_node_in_database(
    db: CacheDatabase,
    node: Node,
    store_outputs: bool = False,
    store_input_nodes_recursively: bool = False,
) -> str:
    """
    Store a node in a database.

    This function stores all the information that is required to restore a node from the
    database.

    Args:
        db (CacheDatabase): The database to store the node in.
        node (Node): The node to store.
        store_outputs (bool): Whether to store the outputs of the node as well.
        store_input_nodes_recursively (bool): Whether to store all the nodes that are
            connected to the inputs of the node recursively.

    Returns:
        Node hash.
    """

def restore_node_from_database(
    db: CacheDatabase, node_hash: str, parent: Workflow
) -> Node:
    """
    Restore a node from the database.

    The node is reconstructed from the database by calling recreate_node and
    adding it to the given parent workflow. The node's inputs are then restored
    either by connecting them to other nodes in the workflow or by setting their
    values directly.

    Args:
        db (CacheDatabase): The CacheDatabase instance to read from.
        node_hash (str): The hash of the node to restore.
        parent (Workflow): The workflow to add the restored node to.

    Returns:
        The restored node.
    """

def get_hash(obj_to_be_hashed: Node | dict) -> str:
```

## What features are required for this?
 - input and output needs to be serializable (Storage spec?)
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

# Big Picture - Language 
High level definition of a node based workflow
- What is required for a complete representation? 

## Nodes
![node](../images/node.png)
- defined by a pure Python function
- a node has one or more input channels and / or one or more output channels 

## Channel
![channel](../images/channel.png)

- supports optional type hints (strongly encouraged)
- knows who it is connected to (technical implementation)
- knows who it is parent node is (technical implementation)

### Encouraged Channel Type Hints
- primitive Python data types 
    - None
    - int
    - str
    - float
    - list (of primitive Python data types)
    - dict (of primitive Python data types)
- Converter to JSONable data types for:
    - np.array
    - dataclasses (with fields of such dataclasses or primitive Python data types)

## Edges
- directed edges - difference between input and output
- $[((n_i, p_i) -> (n_j, p_j)), ...]$
- an edge connects channels between nodes 

## Workflow 
- graph network of nodes and edges 
- instance 

## Marco Node 
![macro](../images/macro.png)

- function that defines a workflow 
- class definition 

## Challenges 
- Cyclic Workflows - dynamic instantiation of nodes
- Pass a node as input 
- pass in a node into a workflow - pass in LAMMPS in Murnaghan
- a modified macro should result in a different hash 
- Convert dataclass to many channels - the other direction of data to dataclass already works
- high throughput studies - currently in pyiron_base represented by the database and pyiron table 
- decide which nodes should be hashed 

# Features for Execution
- replace one node by another in a given workflow / macro 
- composability 
- FAIR 
- settings as optional input to the nodes def(a, b, c, settings)
- setting is a primitive dataclass - this could ideally be even standardized between different workflow frameworks 

## Ontological typing 

## Storage 

### Storage of Edges 
- requires JSONable data on the edges 
- for complex objects which are not JSONable - store the recipe rather than the data
- for complex objects the developer of the node is responsible to provide a file format or a JSON conversion to store the object on disk

### Storage of Workflows
- list of nodes 
- list of edges 
- data for the edges
- transfer output vs. transfer recipe 
- pickle as fall back solution - hashed based storage as default 
- hashes are defined recursively 

#### Challenges
- fenics and other codes with c-bindings cannot be reduced 
- reload graphs with data only saved on a subset of the nodes
- ~~Is it too restrictive to require all structures need to be either JSONable or dataclasses of primitive types or primitive types?~~ It issues a warning when a user tries to save non-serializable data.
- A structure generated with ASE - without using a node - does not include any information how it was created. 

#### Nodes 
- nodes are defined by the library path - class
- label of the node
- version 
- dynamically defined nodes are challenging to store

### Hashable Storage

### Research Data Management 

### Lazy Import 


## Database

- Database stores: hash(i_1, ..., i_n, f), inputs, node, output.
- Output may or may not be missing
- When a workflow is reloaded and the output is called, it checks the **hashes** recursively
- Example: https://github.com/pyiron/pyiron_nodes/blob/main/notebooks/hash_based_db_node_storage.ipynb

## pyiron Node Stores
- local node store
- global node store on the web
- use conda for distribution 
- store nodes in a specific path `${PREFIX}/share/pyiron_nodes`
- use python entry points 
- `pyiron_nodes` provides lazy import for all nodes packages
- https://github.com/pyiron/pyiron_nodes
- nodes stores could be developed in projects like MaterialDigital and NFDI Matwerk (there can be more than one node store)

## Execution 

# Specs 
[see Github](https://github.com/pyiron/specs) - add pseudo code for each interface 

# Strategy 

# Backwards Compatibility

# Tasks 
What do we need to develop in the next three to six months. 

# Database

## What do we need to store?
These are the different places in `pyiron` where we want to store/cache/search blobs of data efficiently.

### Final job results
After a complete `pyiron` or `pyiron_workflow` run a HDF5 file is created with the simulation results.
These results should be indexed to allow for fast searching/filtering not only for own jobs but also for jobs others habe done.  
Requirements:  
- must be capable of storing millions of entries
- must support some dependencies/hierarchy between the jobs
- in contrast to the current use-case in `pyiron_base` it should not keep track of the status of calculation
- it should keep track of how the result was obtained (e.g. storing the graph, storing the notebook, etc)?
#### Possible implementation
`pyiron` currently uses a relational database for this purpose.

### A store for `pyiron_workflow` nodes.
`pyiron_workflow` nodes are pure functions that can be written by anyone. To share these between different jobs and users a centralized storage solution is necessary.

### A cache to avoid expensive recalculation of `pyiron_workflow` nodes.
Since `pyiron_workflow` nodes are pure functions the same input always gives the same output. 
This allows for [memoization](https://en.wikipedia.org/wiki/Memoization) which can be very beneficial for expensive simulations.
#### Possible implementation
While for `pyiron_workflow` there is not a database yet, Joerg developed a first prototype for hashing nodes: [hash_based_db_node_storage.ipynb](https://github.com/pyiron/pyiron_nodes/blob/main/notebooks/hash_based_db_node_storage.ipynb)

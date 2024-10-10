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
##### Search engine like database
- The database is completely independent of pyiron.
- The single source of truth is the HDF5 file pyiron creates.
- The purpose of the database to enable/speed-up searching and filtering.
- We try to keep the database as up-to-date as possible, but this is not guaranteed (mainly to avoid crashing pyiron jobs due to the database beeing not available at times)
  - updates are done with crawling the file system at some interval and push notifications to the database when someone opens a pyiron project
  - this way we could also flag if the project is public (i.e. the crawler can read the HDF5 file)

### A store for `pyiron_workflow` nodes.
`pyiron_workflow` nodes are pure functions that can be written by anyone. To share these between different jobs and users a centralized storage solution is necessary.

### A cache to avoid expensive recalculation of `pyiron_workflow` nodes.
Since `pyiron_workflow` nodes are pure functions the same input always gives the same output. 
This allows for [memoization](https://en.wikipedia.org/wiki/Memoization) which can be very beneficial for expensive simulations.
#### Possible implementation
While for `pyiron_workflow` there is not a database yet, Joerg developed a first prototype for hashing nodes: [hash_based_db_node_storage.ipynb](https://github.com/pyiron/pyiron_nodes/blob/main/notebooks/hash_based_db_node_storage.ipynb)

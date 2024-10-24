# Backwards Compatibility
To provide the users with a seamless transition from the current `pyiron_atomistics` / `pyiron_base` environment to `pyiron_workflow` it is essential to develop a migration strategy.

## Current Prototypes 
* The existing `pyiron_atomistics` jobs, in particular the LAMMPS job, SPHINX job and VASP job can be read with [`pyiron_dataclasses`](https://github.com/pyiron/pyiron_dataclasses) to retrieve dataclasses.
* The dataclasses can be divided into two types, ones which represent the input of the job and ones which represent the output of the job. For [`LAMMPS`](https://github.com/pyiron/pyiron_atomistics/pull/1472) and [`VASP`](https://github.com/pyiron/pyiron_atomistics/pull/1473) we already have python functions which could use the input dataclasses and return the same dataclasses after the successful calculation.
* The example I developed during the pyiron hackathon demonstrates how these python functions can be used inside [`pyiron_workflow`](https://github.com/pyiron/hackathon-2024/blob/main/notebooks/compare_workflow.ipynb).

With this strategy both `pyiron_atomistics` and `pyiron_workflow` are matched to the same dataclasses. With the additional work on the [`pyiron-conceptual-dict`](https://github.com/pyiron-dev/pyiron-conceptual-dict) these dataclasses can also be mapped to the [`cmso-ontology`](https://github.com/OCDO/cmso-ontology) which again can be used for mapping the data to research datamanagement systems like it is discussed in [`pyiron_rdm`](https://github.com/pyiron/pyiron_rdm). 

## Next Steps
While the prototype demonstrates that it is technically possible to achieve backwards compatibility, there are likely a number of challenges which still need to be addressed to provide previous users of `pyiron_atomistics` / `pyiron_base` with a migration strategy. 

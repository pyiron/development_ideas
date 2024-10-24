# Backwards Compatibility
To provide the users with a seamless transition from the current `pyiron_atomistics` / `pyiron_base` environment to `pyiron_workflow` it is essential to develop a migration strategy.
Critical to this is the ability to access data from existing job objects in the new node framework.
Further, it is important to provide most (but not necessarily) all existing functionality in the new framework, especially domain-specific functionality (e.g. tools from `pyiron_atomistics` like running LAMMPS, calculating a Murnaghan curve, etc.)

## Data Compatibility
* The existing `pyiron_atomistics` jobs, in particular the LAMMPS job, SPHINX job and VASP job can be read with [`pyiron_dataclasses`](https://github.com/pyiron/pyiron_dataclasses) to retrieve dataclasses.
* The dataclasses can be divided into two types, ones which represent the input of the job and ones which represent the output of the job. For [`LAMMPS`](https://github.com/pyiron/pyiron_atomistics/pull/1472) and [`VASP`](https://github.com/pyiron/pyiron_atomistics/pull/1473) we already have python functions which could use the input dataclasses and return the same dataclasses after the successful calculation.
* One example mapping `pyiron_atomistics` (and `pyiron/atomistics`) in this way is from Jan in the [pyiron hackathon](https://github.com/pyiron/hackathon-2024/blob/main/notebooks/compare_workflow.ipynb).

With this strategy both `pyiron_atomistics` and `pyiron_workflow` are matched to the same dataclasses. With the additional work on the [`pyiron-conceptual-dict`](https://github.com/pyiron-dev/pyiron-conceptual-dict) these dataclasses can also be mapped to the [`cmso-ontology`](https://github.com/OCDO/cmso-ontology) which again can be used for mapping the data to research datamanagement systems like it is discussed in [`pyiron_rdm`](https://github.com/pyiron/pyiron_rdm). 

## Functionality Compatibility 
* How to integrate the `interactive` pyiron jobs, which allow couping during the run time in a functional approach?
* How to integrate the map-reduce pattern provided by the pyiron table in a functional appraoch?
* How to validate that the complexity for new users does not increase? `pyiron_atomistics` and `pyiron_base` were optimized to enable rapid prototyping by allowing users to develop simulation protocols with as few characters as possible. To benchmark the complexity of the user interface we have a collection of [use cases in `pyiron_base`](https://github.com/pyiron/pyiron_base/tree/main/tests/usecases) which could also be implemented in `pyiron_workflow`. 

## Next Steps
While the data compatibility prototype demonstrates that it is technically possible to achieve backwards compatibility, there are likely a number of challenges which still need to be addressed to provide previous users of `pyiron_atomistics` / `pyiron_base` with a migration strategy. 

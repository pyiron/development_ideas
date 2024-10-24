# Backwards Compatibility
To provide the users with a seamless transition from the current `pyiron_atomistics` / `pyiron_base` environment to `pyiron_workflow` it is essential to develop a migration strategy.
Critical to this is the ability to access data from existing job objects in the new node framework.
Further, it is important to provide most (but not necessarily) all existing functionality in the new framework, especially domain-specific functionality (e.g. tools from `pyiron_atomistics` like running LAMMPS, calculating a Murnaghan curve, etc.)

## Data Compatibility
* The existing `pyiron_atomistics` jobs, in particular the LAMMPS job, SPHINX job and VASP job can be read with [`pyiron_dataclasses`](https://github.com/pyiron/pyiron_dataclasses) to retrieve dataclasses.
* The dataclasses can be divided into two types, ones which represent the input of the job and ones which represent the output of the job. For [`LAMMPS`](https://github.com/pyiron/pyiron_atomistics/pull/1472) and [`VASP`](https://github.com/pyiron/pyiron_atomistics/pull/1473) we already have python functions which could use the input dataclasses and return the same dataclasses after the successful calculation.
* One example mapping `pyiron_atomistics` (and `pyiron/atomistics`) in this way is from Jan in the [pyiron hackathon](https://github.com/pyiron/hackathon-2024/blob/main/notebooks/compare_workflow.ipynb).

With this strategy both `pyiron_atomistics` and `pyiron_workflow` are matched to the same dataclasses. With the additional work on the [`pyiron-conceptual-dict`](https://github.com/pyiron-dev/pyiron-conceptual-dict) these dataclasses can also be mapped to the [`cmso-ontology`](https://github.com/OCDO/cmso-ontology) which again can be used for mapping the data to research datamanagement systems like it is discussed in [`pyiron_rdm`](https://github.com/pyiron/pyiron_rdm). 

Using dataclasses in this way optimally streamlines the data migration process, and is currently our preferred course of action.
However, in general, it is not strictly necessary; for job-based packages without a dataclass interface, a fallback solution is always available to make a bespoke node which takes the saved job file as input and parse the HDF5 for relevant input and output.

## Functionality Migration: `pyiron_workflow` content TODOs 
* Expose `interactive` pyiron job functionality in a node framework. As a first attack, it should be possible to create one node (per interactive engine) that returns an interactive interpreter object, and other nodes that take this object and perform modifications to run stuff in that interpreter; combined with the possibility to create while-loops, this exposes the existing functionality.
  * Where multiple interpreters are available, we should consider breaking these nodes into "generic" interpreter modifications that are common (e.g. changes to the atomic structure), which are able to parse generic commands into interpreter specific commands.
* As a starting point, reproduce functionality for the core [use cases in `pyiron_base`](https://github.com/pyiron/pyiron_base/tree/main/tests/usecases). 

## Next Steps
While the data compatibility prototype demonstrates that it is technically possible to achieve backwards compatibility, there are likely a number of challenges which still need to be addressed to provide previous users of `pyiron_atomistics` / `pyiron_base` with a migration strategy. 

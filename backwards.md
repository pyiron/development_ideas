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

## What to do about pyiron tables?

How to integrate the map-reduce pattern provided by the pyiron table in a functional approach? 
Should we?

To straightforwardly maintain FAIR properties provided by `pyiron_workflow`, the most direct solution is for users to supply data processing nodes at the end of a given workflow -- i.e. we don't provide any explicit infrastructure support for this type of behaviour, users just analyze data from a workflow in whatever bespoke manner they see fit.
The upside is trivially retaining FAIR, the downside is a lack of cross-workflow data aggregation.

At the opposite extreme, we could provide a set of functions take a node or nodes (incl. workflows, which are also nodes), and scans over them for particular properties, perhaps in extremely close analogy to the existing table functions.
While the database behaviour for `pyiron_workflow` is not complete, there is a good chance it could be leveraged for such an attack as well.
In contrast to the last attack, this would support cross-workflow data aggregation, but it is not clear how to accomplish it in a FAIR context.

One avenue by which we _might_ provide the best of both worlds is by leveraging the fact that the _graph_ root and the _semantic_ root are not necessarily identical. 
I.e. a given workflow(/macro) has a parent-most node that functions as the "graph root", and currently this is _also_ the "semantic root" -- but in principle it should be possible to decouple these things.
In such a decoupled world, a "graph" would be a collection of runnable nodes, but these could still retain FAIR connections by giving them "semantic" parent-child relationships to extra-graph semantic objects linking together different graphs.
If data aggregation tools were attached to these non-node semantic parent objects, and different workflows were placed as children of those same semantic parents, we may be able to operate the aggregation in a semantically connected (and thus FAIR) way...

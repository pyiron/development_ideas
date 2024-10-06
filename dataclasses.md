# Data Classes
Python [dataclasses](https://docs.python.org/3/library/dataclasses.html) are part of the python standard library and were originally developed to  [define classes which exist primarily to store values which are accessible by attribute lookup](https://peps.python.org/pep-0557/#rationale). In pyiron dataclasses are used for data transfer between functions, they define the generic interface so ideally two simulation codes which implement the same methodology e.g. density functional theory (DFT), use the same dataclasses as input and produce the same dataclasses as output, so switching from one simulation code to another is as easy as changing the function while the rest of the workflow remains the same. 

Dataclasses are preferable over individual input arguments as they allow grouping parameters which belong to the same context. 

## Interface 
Data classes can be nested, i.e. a data class can contain elements that are data classes.

Optional convenience features:
- overload dataclass to make it behave like a dictionary
  - allows calling python functions with my_func(**my_dataclass)
- dataclass elements have type hints
  - make functions/nodes more robust by guaranteeing that a field has a well-defined type
  - allows us to check for compatible input/output pairs
  - provide the GUI with information about the optimal input widget (e.g. checkbox for bool) 
- writable/loadable to json and hdf5 (for storage)
- creatable by import (must be in a file-based module)
  - allows us to create a data class instance by knowing the import path and values of the fields

## Challenges
The priliminary implementation of dataclasses in [pyiron_dataclasses](https://github.com/pyiron/pyiron_dataclasses) highlighted that it is important to test dataclasses for backwards compatibility. While the storage format might change more frequently, the dataclass format should remain more or less stable and it should be versioned to enable the import of previous calcualtion. While this is something which was originally promised for pyiron it was not achieved so far.

The dataclasses in [pyiron_dataclasses](https://github.com/pyiron/pyiron_dataclasses) can pretty much account for all the information in various `pyiron_atomistics` jobs (currently Lammps, Vasp, Sphinx) since all the information is stored in the HDF5 file which the dataclass uses. However, the dataclasses in `pyiron_nodes` are parts of workflows which do not account for all information in th graph. A "`wf.get_master_dataclass()`" thingy might be needed that automatically collects all aspects of the workflow graph which inlcudes sub-dataclass nodes (also including the functions themselves themselves as strings??). The same would apply for graphs created in `pyiron_base`. [^1]

## Existing Implementations
* [pyiron_dataclasses](https://github.com/pyiron/pyiron_dataclasses) The `pyiron_dataclasses` module implements dataclasses to read `pyiron_atomistics` job objects, namely LAMMPS calculation, Sphinx calculation and VASP calculation from HDF5 files and represent them as dataclasses. In addition, the dataclasses from `pyiron_dataclasses` are used in `pyiron_base` to represent the executable object and server object attached to every job. 
* [pyiron/pyiron_atomistics#1571](https://github.com/pyiron/pyiron_atomistics/pull/1571) Includes additional dataclasses for `calc_minimize()` and `calc_md()`. 
* [pyiron_nodes](https://github.com/pyiron/pyiron_nodes/blob/main/pyiron_nodes/atomistic/calculator/data.py) Contains additional dataclasses to be used in combination with `pyiron_workflow`. 


[^1]: (Tara) I think this point is closely linked to the storage specs, but I thought it fit here better than in storage.md

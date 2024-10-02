# Data Classes
As a generic interface to connect functions. To have the same interface for different simulation codes, it would be great to have the same functions with the same input and/or output arguments available for different simulation codes. But instead of having a big list of input/output arguments, it's better to use data classes. Currently these data classes are scattered in different modules, so it would be great to unify them in one place: https://github.com/pyiron/pyiron_base/blob/main/pyiron_base/dataclasses/job.py , pyiron/pyiron_atomistics#1571 and https://github.com/pyiron-dev/pyiron-hdf5-format/tree/main/pyiron_io/dataclasses.

In pyiron_nodes examples of data classes for input and output are given, e.g. in
https://github.com/pyiron/pyiron_nodes/blob/main/pyiron_nodes/atomistic/calculator/data.py

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


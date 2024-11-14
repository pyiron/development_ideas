# Executor
The executor takes a [task](https://docs.python.org/3/library/concurrent.futures.html), assigns it to the available computing resources and returns a `concurrent.future.Future` object. This interface is defined in the python standard library.

## For Users
### Features 
* The executor enables the assignment of computing resources. This is primarily achieved by interfacing with queuing systems like flux or SLURM. [resource-assignment](https://executorlib.readthedocs.io/en/latest/examples.html#resource-assignment)
* The executor can execute dependent tasks, so when a task receives the future object of a previous task, then the task is only scheduled for execution once the future object is done. [coupled-functions](https://executorlib.readthedocs.io/en/latest/examples.html#coupled-functions)
* The executor introduces a caching mechanism, based on `cloudpickle` which stores the function its arguments and output in an HDF5 file. This HDF5 file is used to submit the function to the queuing system - even via SSH from a separate computer as demonstrated in [pyiron-dev/remote-executor](https://github.com/pyiron-dev/remote-executor/blob/main/example.ipynb). The submitted function remains submitted / running - even when the executor is stopped. When the same function is submitted to the executor again, the executor identifies this function is already cached and returns the output directly.

### Requirements 
A any function / callable which is submitted to an executor has to be serializable. In the case of `concurrent.future.Executor` the serialization uses `pickle` in the case of `executorlib` [`cloudpickle`](https://github.com/cloudpipe/cloudpickle) is used. By using the `cloudpickle` by value pickle capability `executorlib` can execute callables defined in the jupyter notebook, while the same is not possible with the executors defined in the standard library.

## For Developers 
The API of the `Executor` class is defined based on there functions: 
* [`submit(fn, /, *args, **kwargs)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.submit) - schedules the callable, `fn`, to be executed as `fn(*args, **kwargs)` and returns a `Future` object representing the execution of the callable.
* [`map(fn, *iterables, timeout=None, chunksize=1)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.map) - Similar to [`map(fn, *iterables)`](https://docs.python.org/3/library/functions.html#map) except: the iterables are collected immediately rather than lazily; `fn` is executed asynchronously and several calls to fn may be made concurrently.
* [`shutdown(wait=True, *, cancel_futures=False)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.shutdown) - Signal the executor that it should free any resources that it is using when the currently pending futures are done executing. 

For more detials [docs.python.org](https://docs.python.org/3/library/concurrent.futures.html) - the explanations above are shortened.

## Required Developments 
* The goal is to unify the two interfaces in `executorlib` to provide the caching of the `Fileexecutor` as optional feature. 
* For both `pyiron_base` and `pyiron_workflow` it would be convenient if users could send the entire graph to the HPC for execution, without relying on a connection to an external process monitoring the graph as it is currently implemented in `pyiron_workflow`. This can be achieved if a series of smaller nodes with similar resource requirements can be converted into one larger task which is then executed by `executorlib` as a single job in the queuing system. So a node in `pyiron_workflow` does not have a one to one relation to an task in `executorlib`. In `executorlib` tasks are primarily defined by having similar resoruce requirements, only when a process has changing resource requirements, then it makes sense to separate them into individul tasks in `executorlib`. In the same way `executorlib` can be used to efficiently parallelize the execution.  
* Implement remote file handling. For example when two VASP calculation are submitted to an HPC cluster, with the second VASP calculation depending on the `WAVECAR` from the first VASP calculation. We do not want to copy the `WAVECAR` from the HPC to the workstation and then back from the workstation to the HPC. Still this level of remote file handling is currently not yet implemented in `executorlib`.
* Update documentation of `executorlib`. Currently prototypical features like the submission to an queuing system outside the queuing system allocation are only documented as prototypical examples [pyiron-dev/remote-executor](https://github.com/pyiron-dev/remote-executor) but are not yet included in the official documentation.
* Extend `pyiron_base` to always use `executorlib` for the submission of jobs. Currently, `pyiron_base` still directly interacts with `pysqa` which results in duplicated code. This can be streamlined by using `executorlib` directly. 

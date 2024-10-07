# Executor

An executor takes a task, assigns it to the available computing resources and returns a `concurrent.future.Future` object. This interface is defined in the python standard library.

The API of the `Executor` class is defined based on there functions: 
* [`submit(fn, /, *args, **kwargs)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.submit) - schedules the callable, `fn`, to be executed as `fn(*args, **kwargs)` and returns a `Future` object representing the execution of the callable.
* [`map(fn, *iterables, timeout=None, chunksize=1)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.map) - Similar to [`map(fn, *iterables)`](https://docs.python.org/3/library/functions.html#map) except: the iterables are collected immediately rather than lazily; `fn` is executed asynchronously and several calls to fn may be made concurrently.
* [`shutdown(wait=True, *, cancel_futures=False)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.shutdown) - Signal the executor that it should free any resources that it is using when the currently pending futures are done executing.

For more detials [docs.python.org](https://docs.python.org/3/library/concurrent.futures.html) - the explanations above are shortened.
This is the target interface for pyiron tools.

## Downstream -- `pyiron_workflow`

* An `executor` or information for creating one can be assigned to a node, and that node will use it for the core functionality of the node
* The executor should inherit from `concurrent.futures.Executor`, but core is simply that it provides a corresponding `submit` method, and returns a `concurrent.futures.Future` object
* Updating the graph state with the returned value requires the ability to assign a callback function to the future
* It is possible, but not conveniently wrapped, for a graph execution to survive the shutdown of the parent python process (i.e. if the "executor" runs stuff on HPC resources), but requires the executor serializing the result to file in a particular way
* `executorlib.Executor` thus fulfils basic `pyiron_workflow` requirements, and is promoted as a go-to use case

### TODO
* Make the interface to executors that survive the shutdown of the parent process more convenient


## Downstream -- `pyiron_base`

`pyiron_base` currently directly interacts with `pysqa` rather than using an executor.

### TODO
* Extend `pyiron_base` to always use `executorlib` for the submission of jobs.

## Upstream -- `executorlib`

* The executor enables the assignment of computing resources. This is primarily achieved by interfacing with queuing systems like flux or SLURM. [resource-assignment](https://executorlib.readthedocs.io/en/latest/examples.html#resource-assignment)
* The executor can execute dependent tasks, so when a task receives the future object of a previous task, then the task is only scheduled for execution once the future object is done. [coupled-functions](https://executorlib.readthedocs.io/en/latest/examples.html#coupled-functions)
* The executor introduces a caching mechanism, based on `cloudpickle` which stores the function its arguments and output in an HDF5 file. This HDF5 file is used to submit the function to the queuing system - even via SSH from a separate computer as demonstrated in [pyiron-dev/remote-executor](https://github.com/pyiron-dev/remote-executor/blob/main/example.ipynb). The submitted function remains submitted / running - even when the executor is stopped. When the same function is submitted to the executor again, the executor identifies this function is already cached and returns the output directly.

### Requirements 
A any function / callable which is submitted to an executor has to be serializable. In the case of `concurrent.future.Executor` the serialization uses `pickle` in the case of `executorlib` [`cloudpickle`](https://github.com/cloudpipe/cloudpickle) is used. By using the `cloudpickle` by value pickle capability `executorlib` can execute callables defined in the jupyter notebook, while the same is not possible with the executors defined in the standard library.

### TODO

* The goal is to unify the two interfaces in `executorlib` to provide the caching of the `Fileexecutor` as optional feature.
* A series of smaller nodes with similar resource requirements should be be convertable into one larger task which is then executed by `executorlib` as a single job in the queuing system, i.e. task grouping. 
* Implement remote file handling. For example when two VASP calculation are submitted to an HPC cluster, with the second VASP calculation depending on the `WAVECAR` from the first VASP calculation. We do not want to copy the `WAVECAR` from the HPC to the workstation and then back from the workstation to the HPC. Still this level of remote file handling is currently not yet implemented in `executorlib`.
* Update documentation of `executorlib`. Currently prototypical features like the submission to an queuing system outside the queuing system allocation are only documented as prototypical examples [pyiron-dev/remote-executor](https://github.com/pyiron-dev/remote-executor) but are not yet included in the official documentation.


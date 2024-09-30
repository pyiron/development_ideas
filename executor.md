# Executor
The executor takes a task, assigns it to the available computing resources and returns a `concurrent.future.Future` object. This interface is defined in the python standard library.

## `concurrent.future.Executor`
The API of the `Executor` class is defined based on there functions: 
* `submit(fn, /, *args, **kwargs)` - schedules the callable, `fn`, to be executed as `fn(*args, **kwargs)` and returns a `Future` object representing the execution of the callable.
* `map(fn, *iterables, timeout=None, chunksize=1)` - Similar to `map(fn, *iterables)` except: the iterables are collected immediately rather than lazily; `fn` is executed asynchronously and several calls to fn may be made concurrently.
* `shutdown(wait=True, *, cancel_futures=False)` - Signal the executor that it should free any resources that it is using when the currently pending futures are done executing. 

For more detials [docs.python.org](https://docs.python.org/3/library/concurrent.futures.html) - the explanations above are shorten.

## `executorlib`
`executorlib` extends this interface in three ways: 
* It enables the assignment of resources beyond just the number of cores / threads. This is primarily achieved by interfacing with queuing systems like flux or SLURM. [resource-assignment](https://executorlib.readthedocs.io/en/latest/examples.html#resource-assignment)
* It introduces dependent tasks, so when a task receives the future object of a previous task, then the task is only scheduled for execution ones the future object is done. [coupled-functions](https://executorlib.readthedocs.io/en/latest/examples.html#coupled-functions)
* The `Fileexcutor` introduces a caching mechanism, based on `cloudpickle` which stores the function its arguments and output in an HDF5 file. This HDF5 file is used to submit the function call to a queuing system - even via SSH from a separate computer as demonstrated in [pyiron-dev/remote-executor](https://github.com/pyiron-dev/remote-executor/blob/main/example.ipynb). This process remains submitted / running - even when the executor is stopped. When the same function is submitted to the executor again, the `Fileexcutor` identifies this function is already cached and returns the output directly.

The goal is to unify the two interfaces in `executorlib` to provide the caching of the `Fileexecutor` as optional feature. 

## Limitation / Requirements
A callable which is submitted to an executor has to be serializable. In the case of `concurrent.future.Executor` the serialization uses `pickle` in the case of `executorlib` `cloudpickle` is used. By using the `cloudpickle` by value pickle capability `executorlib` can execute callables defined in the jupyter notebook, while the same is not possible with the executors defined in the standard library. 

## SPEC
Based on the definition of the `concurrent.future.Executor` in combination with the recent developments in `executorlib` I have the feeling we covered the most essential functionality of an executor required for our workflow application. Still this topic is open for discussion. 

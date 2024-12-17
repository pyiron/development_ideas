# Storage Interface 

## Group members
@XzzX @JNmpi @liamhuber @samwaseda @ligerzero-ai @niklassiemer 

## What is it supposed to do?
❗**We expect there are no changes in the python environment, i.e. the exact same environment is used during store and load! We try our best to detect changes and warn the user but we neither guarantee to detect changes nor do we guarantee restored data does work correctly if the environment has changed!**  

Provide load and store functionality for:
- workflows
    - nodes (only libpath, no source code)
    - connections
    - input/output data (switchable)
- single nodes
    - node (only libpath, no source code)
    - input/output data (switchable)
- input/output data

❓Should data be storeable independent of the workflow? (FAIR?)  

## Implementation details?

### Current state
There is already a `pickle` based load/store solution:  
https://github.com/pyiron/pyiron_workflow/blob/main/pyiron_workflow/node.py#L895-L915

There is a proof-of-concept implementation of a `GenericStorage` in `tinybase`.

### Front end
- Provide corresponding methods for the objects to be saved/loaded
    - `save_workflow(wf, backend, filename, metadata: dict | None = None, data_filter_regex: str | None = None)`
- Introduce a `backend` argument that selects how things should be saved.
    - Can be a list of backends. If one backend fails, the next one is tried. 
- ❓ Interface: `wf.save(filename, backend)` vs `save(wf, filename, backend)`? As nodes?
    - free function interface has benefits during loading as it does not require a valid class instance
    - ✔️ We decided to go with `save(...)` first and might provide a class function later.

### Middle end
#### Envisioned usage
```python
with XXXStorage(filename) as storage:
    storage["a"] = 5
    b = storage["b"]
```
`storage` should be usable like a dict and support the creation of groups like `h5py` does.

#### GenericStorage
```python
class StorageGroup(abc.ABC, MutableMapping[str, Any]):
    """API for organizing/loading/storing stuff"""

    @abc.abstractmethod
    def create_group(self, key: str) -> StorageGroup:
        pass

    @abc.abstractmethod
    def get(self, key: str, default: Any = None):
        pass

    @abc.abstractmethod
    def require_group(self, key: str) -> StorageGroup:
        pass


class GenericStorage(abc.ABC):
    """Context manager for the storage blob, i.e. file open/close, etc"""

    @abc.abstractmethod
    def close(self):
        pass

    @abc.abstractmethod
    def __enter__(self) -> StorageGroup:
        pass

    @abc.abstractmethod
    def __exit__(self, exc_type, exc_value, traceback):
        pass
```

#### Back end
##### Our dream back-end
- Our dream storage interface is designed to store input and output data for long-term storage and archiving.
- It should allow loading partial objects, or saving to an existing file to partially update a saved object.
- It should facilitate browsing/lazy loading -- i.e. we can see what is stored in it and metadata about the stored object without fully reinstating the (partial) object as a python object.
- It stores as much versioning information as possible (module version, git hash if module is in a git repo, maybe even a hash of the raw source code?), and gives users some freedom for how strictly they want to enforce versioning at load time (ranging from "just go for it", to "look at the metadata of what is about to be loaded -- does my current environment match that metadata? If not throw an exception!").
- It is fast (save/load cycle comparable to `pickle`).
- It is memory efficient (storage footprint comparable to `pickle`).
- It should include options for semantically grouping together workflows in different storage locations which could be useful for a variety of purposes like publishing, combining smaller workflows into larger workflows etc.

##### pickle
:+1: convenient on-click solution  
:+1: memory efficient (binary format)  
:-1: not everything can be stored  
:-1: source code of serialized objects needs to be available when restoring  
:-1: no versioning  
:-1: no metadata  
:-1: not human-readable (binary format)  
👎 not suitable for longtime storage

##### cloudpickle

##### HDF5
Implementation of `GenericStorage` using `h5py`

##### JSON
- preferred output format
- guaranteed if input/output described by workflow objects (nodes, dataclasses, primitive data types)

# Appendix
## Tinybase Interface

The interface in `tinybase` works some what analogous to the original objects in base, but streamlined in a way that make polymorphism easier.
What matters for users is that once they obtain a `storage` object, they can `__setitem__` and `__getitem__` any object into the storage.
`create_group` is available to allow hierachical storage.
Tinybase handles everything else.

For developers this means every storage backend needs to implement an interface, the [GenericStorage](https://github.com/pyiron/pyiron_contrib/blob/53907adaf1070a6112a8d3697dc180d8cdacb22a/pyiron_contrib/tinybase/storage.py#L15).
Complex objects can implement a [Storable](https://github.com/pyiron/pyiron_contrib/blob/53907adaf1070a6112a8d3697dc180d8cdacb22a/pyiron_contrib/tinybase/storage.py#L373) interface that then knows how to put itself into such a storage.
This does not mean that they need to.  
tinybase already has some provisions (for lists, pickles and a prototype for getstate/setstate) to fallback on automatic storables.
Under the links above there are also example implementations for both interfaces.
They are all quite short.

### User facing Interface

```python
class GenericStorage:
    def __getitem__(self, key):
        ...
    def __setitem__(self, key, value):
        ...
    def __delitem__(self, key):
        ...
    def create_group(self, key):
        ...
```

### Dev facing Interface

```
class GenericStorage:
    def __getitem__(self, item: str) -> Union["GenericStorage", Any]:
        pass
    def _set(self, item: str, value: Any):
        pass
    def __delitem__(self, item: str):
        pass
    def create_group(self, name):
        pass
    @property
    def name(self):
        pass
```

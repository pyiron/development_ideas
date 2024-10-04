# Storage Interface 

## Requirements and promises for users

Users should be able to store any python object which could be pickled.

They can modify the storage process by modifying the standard routines used by `pickle`, i.e. `__reduce__`, `__getstate__`, `__setstate__`, `__getnewargs__`, `__getnewargs_ex__`, and/or `__reduce_ex__`.

In addition to whatever other benefits a particular backend brings, python objects should be able to be (de)serialized using these methods in a way that is consistent with the behavior of `pickle`.

> `cloudpickle` idea: Users always get successful storage, even for un-pickleable object -- but at the cost that they might lose benefits of their backend as we simply `cloudpickle` their object and then store the bytestream

pyiron is designed to provide default serialization to formats suitable for long-term storage such as hdf5 or json, without requiring the user to write/modify storage routines such as `__getstate__` etc. The fallback to (cloud) pickle should be avoided whenever possible. The following types are supported:

- basic Python types (int, float, str, etc.) (works out of the box)
- dataclasses consisting of fields that are serializable by default (json, hdf5, etc.)
  - such dataclasses are saved/loaded via their import path (-> works for dataclass definitions that are file-based, i.e. in a Python module)
  - works recursively, i.e. a dataclass field can be a dataclass
- Function nodes are stored by converting input and output into dictionaries (or dataclasses). The function of the node is stored/loaded via its import path (equivalent to dataclasses).
- Types that cannot be serialized by default
  - Callables/Objects
  - Workaround: store/load such data via a generator node, where the node only requires standard input.
    - Example: instead of storing an ASE instance, store the generator, e.g. atomistic.structure.built.bulk('Al'). The generator's input, such as the species ('Al'), has only standard types        

## Requirements for the user-facing interface

The pyironic storage interface should offer at least `save(obj: typing.Any, filename: pathlib.Path | str, **kwargs)`, `load(filename: pathlib.Path | str, **kwargs)`, and `delete(filename: pathlib.Path | str, **kwargs)` interfaces for (de)serializing objects.
Interfaces to particular back-ends may allow separate kwargs (e.g. HDF5 specifying a path to use inside a given file).

> `cloudpickle` idea: `cloudpickle_fallback: bool` is a kwarg for all interfaces? When `True`, the back-end must fall back on `cloudpickle` to package the object and store the bytes, but when `False` an un-pickleable object raises an exception?

> `back-ends` idea: The user-facing interface could let the user see (e.g. by tab completion) which different back-ends are currently available and easily choose between them?

## General requirements for back-ends

A pyironic storage back-end must offer at least `dump(obj: typing.Any, filename: pathlib.Path | str, **kwargs)`, `load(filename: pathlib.Path | str, **kwargs)`, and be capable of storing any object that could be succesfully pickled.
From its dumped file, it must be able to re-instantiate the python object that was saved.

> `cloudpickle` idea: back-ends must be able to fall-back on cloudpickling the object and storing the bytes if they would otherwise fail?

Back-ends must be able to secure the data they need for the dump/load cycle from the standard `pickle` methods of `__reduce__`, `__getstate__`, `__setstate__`, `__getnewargs__`, `__getnewargs_ex__`, and/or `__reduce_ex__`.

Examples of possible back-end formats:

- [HDF5](url_for_hdf)
- [JSON](url_for_json)
- [cloudpickle](url_for_cloudpickle)
  
### Our dream back-end

Our dream storage interface is designed to store input and output data for long-term storage and archiving.
It should allow loading partial objects, or saving to an existing file to partially update a saved object.
It should facilitate browsing/lazy loading -- i.e. we can see what is stored in it and metadata about the stored object without fully reinstating the (partial) object as a python object.
It stores as much versioning information as possible (module version, git hash if module is in a git repo, maybe even a hash of the raw source code?), and gives users some freedom for how strictly they want to enforce versioning at load time (ranging from "just go for it", to "look at the metadata of what is about to be loaded -- does my current environment match that metadata? If not throw an exception!").
It is fast (save/load cycle comparable to `pickle`).
It is memory efficient (storage footprint comparable to `pickle`).


# Tinybase Interface

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

## User facing Interface

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

## Dev facing Interface

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

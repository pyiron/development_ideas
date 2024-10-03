# Storage Interface 
The storage interface is designed to store input and output data for long-term storage and archiving. The storage interface offers three different storage modes:

- [HDF5](url_for_hdf)
- [JSON](url_for_json)
- [cloudpickle](url_for_cloudpickle)

The storage interface requires the data in the form of `dict` and the directory path to store the data. It stores and loads the data via the following two functions:

```python
def save(
    data: dict,
    path: str,
    mode: str = "hdf5"
)
```

```python
def load(
    path: str
    mode: str = "hdf5"
) -> dict
```

While all three modes are meant for long-term storage, the storage interface guarantees the validity of the stored data as long as the corresponding modules promise. For more information, take a look at their specs.


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

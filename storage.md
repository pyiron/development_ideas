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

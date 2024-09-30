# Storage Interface 
The storage interface is designed to store input and output data for long-term archiving. The storage interface offers three different storage modes:

- HDF5
- JSON
- cloudpickle

The storage interface requires the data in the form of `dict` and the directory path to store the data. It stores and loads the data via the following two functions:

```python
def save(
    data: dict,
    path: str,
    mode: str = "hdf5"
)

def load(
    path: str
    mode: str = "hdf5"
) -> dict
```

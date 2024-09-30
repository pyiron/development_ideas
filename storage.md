# Storage Interface 
The storage interface is planned to store input and output data for long-term archival, primarily to address the requirements of the funding agencies to archive our data. Ideally this interface would support multiple adapters to store in HDF5, JSON or as pickle / cloudpickle binary. 

At the same time the effort for the developer to add support for a new data type should be ideally minimal -- a good target is to have the same requirements as `pickle` (python's default storage).

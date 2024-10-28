# Software-specific resources

Software-specific resources should be handled underneath a specific node-hierarchy as determined by the node developers. Developers should not be expected to conform to a specific centralised way of handling software-specific resources.

pyiron_workflow should provide an as-shipped method of reading a "config" or "state" file which indicates to users how resources are preferentially set up, but this should be a non-binding approach to software-specific resources.

The layer of abstractions handling resources should be as minimal as possible, ideally a single layer, i.e. via the centralised config file only.

Resources between different software programs should not share abstractions, to lower the barrier of entry to node development. This also provides flexibility to support the different types of software configurations that can exist. A developer producing nodes should only need to understand the **workflow** and **node** parts of pyiron_workflow. 


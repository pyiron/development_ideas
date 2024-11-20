# Storage Interface 
## Group members
@XzzX @JNmpi @liamhuber @samwaseda @ligerzero-ai 
## What is it supposed to do?
Provide load and store functionality for:
- workflows
- output data

❓ Should data be storeable independent of the workflow? (FAIR?)
## How do we want to do it?
- Provide corresponding methods for the objects to be saved/loaded
    - `wf.save(filename, backend)`, `wf.load(filename, backend)`
    - `node.outputs.save(filename, backend)`, `node.outputs.load(filename, backend)`
- Introduce a `backend` argument that selects how things should be saved.
- ❓ Interface: `wf.save(filename, backend)` vs `save(wf, filename, backend)`? As nodes?
- ❓ Introduce a user supplied `metadata` argument for additional information?
## What backends do we provide?
### Our dream back-end
- Our dream storage interface is designed to store input and output data for long-term storage and archiving.
- It should allow loading partial objects, or saving to an existing file to partially update a saved object.
- It should facilitate browsing/lazy loading -- i.e. we can see what is stored in it and metadata about the stored object without fully reinstating the (partial) object as a python object.
- It stores as much versioning information as possible (module version, git hash if module is in a git repo, maybe even a hash of the raw source code?), and gives users some freedom for how strictly they want to enforce versioning at load time (ranging from "just go for it", to "look at the metadata of what is about to be loaded -- does my current environment match that metadata? If not throw an exception!").
- It is fast (save/load cycle comparable to `pickle`).
- It is memory efficient (storage footprint comparable to `pickle`).
- It should include options for semantically grouping together workflows in different storage locations which could be useful for a variety of purposes like publishing, combining smaller workflows into larger workflows etc.
### pickle
:+1: convenient on-click solution  
:+1: memory efficient (binary format)  
:-1: not everything can be stored  
:-1: source code of serialized objects needs to be available when restoring  
:-1: no versioning  
:-1: no metadata  
:-1: not human-readable (binary format)  
### cloudpickle
### HDF5
### JSON
### Complex one-fits-all backend for workflows from different users
See [Database](database.md)

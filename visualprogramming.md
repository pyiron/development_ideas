# Visual Programming Interface
The visual programmming interface `pyiron-xyflow` or `PyironFlow` is a gui skin based on [ReactFlow](https://reactflow.dev/) that currently works on top of `pyiron_workflow`. Theoretically, one could currently pack `pyiron_base` jobs into nodes for execution. The gui could also be extended to pack the workflow graph (extracted from the gui using `get_workflow()`) into a `pyiron_base` job for execution. An existing code-based workflow graph can be packed into the gui using `PyironFlow([wf])` where wf is the existing graph.

## Example of a multiscale simulation

### Problem definition:
![Gui1](https://github.com/user-attachments/assets/cce60750-6eb4-4e16-8fca-af192017da25)

### Constructing the workflow:
![Gui2](https://github.com/user-attachments/assets/3228734b-7ff5-4512-b965-ff8c6d10103a)

### Execution:
![Gui3](https://github.com/user-attachments/assets/34e43a83-c5bf-44ce-8aea-cd1d30f2b14d)

### Viewing source code:
![Gui4](https://github.com/user-attachments/assets/04c35f35-e850-4f5d-a511-ae35642e7d3c)

### Visualization of FEM meshes using PyVista:
![Gui5](https://github.com/user-attachments/assets/653d0bfd-92fe-4188-b8bd-e04a8f8fa8da)

### Atomistic sub-workflow:
![Gui6](https://github.com/user-attachments/assets/c4a85724-ce4e-4383-8c47-64c181281fe6)

### Continuum sub-workflow:
![Gui7](https://github.com/user-attachments/assets/42b617d4-303d-405a-b057-eebcc94089f4)

## Planned/desired/in-development features

- Dynamically add nodes from the notebook to the node library (currently being polished by Julius Mittler).
- Dynamically make macros from the gui using a box selection and add them to the node library (currently in development by Julius Mittler). The macro creaction function itself would probably need to be in the core `pyiron_workflow` module while the gui elements are handled here.
- Automatic positioning of nodes in a nice way on (re-)instantiation (current positioning is passable, but room for improvement exists). A feature for this exists in the [pro version](https://reactflow.dev/examples/layout/auto-layout), but is it worth buying considering resources like [this](https://gitlab.com/graphviz/graphviz/-/tree/main/lib/neatogen) exists.
- Toolbar for controlling the workflow (rather than nodes), ad other global graph functions (e.g., openBIS upload, hpc submission etc.).
- Toggle to visualize nodes inside macros. We might need to break reactflow open for this and make custom modifications or buy the [pro version](https://reactflow.dev/examples/layout/expand-collapse). Even with the pro version, there's no guarantee it would work the way we want it to.
- Enable creation and visualization of circular workflows.
- Edit the source code of nodes from within the gui. Maybe something that uses [this](https://uiwjs.github.io/react-codemirror/) or [this](https://stackoverflow.com/questions/51272255/how-to-use-filereader-in-react).
- Caching based on a hash-based storage. Currently caching does not work at all in the gui since the workflow graph is reinstantiated everytime there's a change in the gui. This could be theoretically fixed by comparing the dictionary of the existing workflow to the new dictionary from the gui. But this would be a painstaking process and easily broken. A better way would be to work with a hash-based storage which would reload outputs when the inputs of a node have not changed. However this would have to be implemented in `pyiron_workflow` and not in the gui side of things.

## Mid to long-term plans

The visual programming interface has the potential of breaking into the academic and possibly industrial mainstream with a few promisng prospects already. In this regard, it is perhaps worth questionning whether `pyironflow` continues to stay as a "educative and fun feature" in our notebooks or is developed into a full-fledged workflow manager with all the bells and whistles expected of a professional tool.

There are two obstacles for this: reactflow and jupyter notebooks. 

Reactflow is a general-purpose tool for node-based workflows and, at least the non-pro version, would eventually run out of features to represent our more-complicated workflows. Circular workflows which involve setting signals in addition to data connections, toggling the visualization of macros, and flooding the viewport with buttons to access various functions offered by `pyiron_wokflow` are some of the challenges that are already looming. Either we can keep trying to find "innovative" solutions, or we can just break open reactflow an shape it for our purposes.

Another issue is the lack of reproducibility with jupyter notebooks. The fact that users can execute cells in any arbitrary order, and the how hard it is find the cell in the notebook that does something interesting (e.g., to the structure) makes it unappealing for FAIR principles. It is no doubt very useful for "quick and dirty" tasks but is overall not suitable for completely encapsulating the workflow in a neat way. The `pyiron_workflow` module specifically addresses this but gives the user the freedom to continue their work outside the workflow if they desire by extracting outputs from the workflow nodes. Additionally, many users in the industry would prefer a streamlined standalone interface over a gui that has been shoe-horned into a jupyter notebook.

One way to address this would be to develop an interface that is independent of notebooks (but could still work in jupyterlab, just not in a notebook). Although it should be possible to maintain a persistant ssh connection to hpc from a desktop app the way [OVITO does](https://www.ovito.org/manual/advanced_topics/remote_file_access.html), it may make more sense to have a web-based service (would also be nice for advertising purposes). Ideally, this service should offer a toggle on the nodes which allows users to switch between the graphic node representation and the underlying code. Users should be able to add new nodes and program them directly in the web app. All the features mentioned in the previous section would naturally be included. Another possible development could be trying to make updating the nodes less cumbersome, i.e., no kernel restarts by importing the code behind the function, along with the function itself into the app. Eventually we could try to set up collaborations with e.g., OVITO, ParaView, Vesta etc. for powerful scientific visualization and analysis options in the webapp.  

# Visual Programming Interface
The visual programmming interface `pyiron-xyflow` or `PyironFlow` is a gui skin based on [ReactFlow](https://reactflow.dev/) that currently works on top of `pyiron_workflow`. Theoretically, one could currently pack `pyiron_base` jobs into nodes for execution. The gui could also be extended to pack the workflow graph (extracted from the gui using `get_workflow()`) into a `pyiron_base` job for execution. An existing code-based workflow graph can be packed into the gui using `PyironFlow([wf])` where wf is the existing graph.

## Milestones

![VisProgMilestones](https://github.com/user-attachments/assets/a1b02502-7d9a-4a3d-8591-c77601011aba)

Based on the above figure, the priority for the next months is to ensure that the main functionalities are available in the GUI and that the user has an intuitive and pleasant experience with the tool. For this 4 main categories can be defined:

### 1. Complex workflows:
- Creating macros (30.11.2024)
- Visualize components of macros (28.02.2025)
- Signals for if-statements and while-loops (30.04.2025)
- ...

### 2. Global workflow control
- A toolbar for running/saving/loading worflows (31.12.2024)
- Tabs in the accordian for uploading entire graph to openBIS, setting HPC parameters for entire graph submission etc. (30.06.2025)
- ...

### 3. Exexution and synchronization
- Hash-based caching or overhaul existing setup so that workflow is not reinitiated (which clears existing cache) (31.05.2025)
- Dynamically update node library (and to a certain extent reactflow) widget based on changes to function files (30.06.2025)
- Status of node execution (using hooks on node signals) (28.02.2025)
- ...

### 4. Cosmetics
- Expand on themes (30.06.2025)
- Give users the ability to set viewport size, layout etc. (30.06.2025)
- ...

The expected time and order of completion is dependent on the number of resources available and on the interests of said resources (e.g., directions students would like to take in their master's thesis).


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

# Graphical user interface
## An idea for an interface (definitely long-term plan) [^1]
The app can be web-based or desktop (different people seem to have preferences for different things). But many people seem to agree that existing third-party infrastructure are not streamlined for material scientists, which adds a lot of overhead to the already overloaded researchers. There always seems to be a trade-off in terms of time and effort between acheiving FAIR and concentrating on scientific understanding. 

Jupyter notebooks do not help in this regard since there is no fixed order of execution, and one always has to hope that the researcher has neatly structured and documented their notebooks. Even then, it is very difficult to automate data organizing efforts frm a jupyter notebook. The `pyiron_workflow` module addresses this to certain extent, but there is still a possibility for researchers to extract outputs from the Workflow and continue working on the outside. One could always justify this approach by saying that they just want to quickly try something out, but then completely forget to put the tried stuff into nodes due to time pressure. Then we are back at square-one dealing with lengthy notebooks. Another disadvantage is that a lot of analysis and visualization tools only offer surface level integration in a notebook. E.g., OVITO has strong python scripting available, but the viewport in a notebook is minimal. This takes away the crucial ability for researchers to interact in a detailed way with large structures. Thus the OVITO pipeline nodes would mostly be prepared after the user has homed in on the steps assuming the user remembers to do it.

A solution for this could be a custom interface that is streamlined for scientific work. It would borrow components from notebooks and node visualizers like reactflow but customize for complex scientific workflows. Dataclasses, datatypes, ontologies, HPC connection, knowledge-base connection, visualization and analysis tools etc. can be baked into the interface instead of writing cumbersome modules which users have to figure out how to use.

Below are some ideas for some of the specs described in other files:

### An overview:
![Picture1](https://github.com/user-attachments/assets/4b11518a-baa2-4135-9d4c-52f6231d3ead)

### The app toolbar:
- File: 
   - New workflow (in same session)
   - New workflow (in new session)
   - Save workflow
   - Load workflow (in same session)
   - Load workflow (in new session)
   - Export (in some human and/or machine-readable representation)
   - … other general options
- Database:
   - Move workflow
   - Find workflow
   - …
- Appearance: 
   - Themes
   - Layout: side by side, above and below
   - GUI direction: horizontal or vertical
   - …
- Configuration:
   - HPC details (defaults)
   - LLM co-pilot details (workflow assembly/node library suggestions/co-pilot for new nodes)
   - Knowledge-base details (defaults)
   - Database details (defaults)
- Help
  - Pyiron docs

### The GUI
<br clear="left"/>
<img align="left" src=https://github.com/user-attachments/assets/1679741f-acd1-4d46-8429-66f667f44d0c width="400">

- Changes in GUI immediately reflected in code and vice versa 
- Based on reactflow or similar but customized so that complex workflow concepts can be implemented:
   - Circular workflows
   - Signals vs data channels
   - Expand/collapse macros
   - Expand collapse node channels
   - Expand collapse dataclasses
   - Execution signals (connect with dashed-lines)
   - …
- LLMs to assist assembly of workflows (which leverage datatypes)
<br clear="left"/>

### Coding area
<br clear="left"/>
<img align="left" src=https://github.com/user-attachments/assets/6051f553-aa68-4716-8330-dfdaf3e61190 width="400">

- Changes in code immediately reflected in GUI and vice versa
- Users can work only with nodes – for FAIR!
- Cell block at the end for setting up and controlling workflow
- …
<br clear="left"/>

### HPC resource allocation
<br clear="left"/>
<img align="left" src=https://github.com/user-attachments/assets/2f95ea01-c9db-4d3e-be38-16e211570074 width="400">

- Intuitive interface
- HPC configs in app toolbar as mentioned previously – to enable and disable options in the interface
- Submit entire graph and auto-group options (which can be disabled from the app toolbar) populate the interface below with the determined resources, which the user can further modify if they are not happy with it

<br clear="left"/>

### KB (Knowledge-base) Map

<br clear="left"/>
<img align="left" src=https://github.com/user-attachments/assets/90336733-61bd-47a7-a32f-b726f6d5a8ba width="400">

- Maps to objects in the pyiron knowledge-base
- Uses builtin dataclasses and datatypes and uploads stored data (HDF5, JSON, pickle as backup)
- Knowledge base is customized openBIS with better nesting for macros, more levels of inventory categorization for ontological compatibility, ontology ids associated to properties etc.
- The knowledge base object types correspond to classes of the nodes in the node library, and the individual instantiated objects correspond to node instances 

<br clear="left"/>

### Output area

<br clear="right"/>
<img align="right" src=https://github.com/user-attachments/assets/e90979e6-4f81-4281-8eac-c2b29e7bfcbd width="400">

- Displays output of nodes (run, pull) and workflow
- Collaborations with OVITO, ParaView etc to have viewport plugins (or try to improve and use jupyter plugins) – with helpful toolbars
- Analysis pipelines of tools like OVITO as nodes in the workflow

<br clear="right"/>

### K-graph

<br clear="right"/>
<img align="right" src=https://github.com/user-attachments/assets/0e767070-4af5-43c3-aed1-4c19877e5931 width="400">

- Displays knowledge graphs of selected nodes based on existing ontologies
- For entire workflow, tries to connect node-based knowledge graphs into a large workflow graphs (would exclude data without ontology types)

<br clear="right"/>

### K-Base

<br clear="right"/>
<img align="right" src=https://github.com/user-attachments/assets/b0a0db0f-6f6d-40d2-8f9b-3d6fff8271b9 width="400">

- Knowledge base is customized openBIS with better nesting for macros, more levels of inventory categorization for ontological compatibility, ontology ids associated to properties etc.
- New object types for node classes in K-Base defined and controlled by sysadmins to avoid unintended overwrites
- Node classes without a defined object type in the K-Base will be uploaded as a generic node till a object type is defined.

<br clear="right"/>

### Node Lib

<br clear="right"/>
<img align="right" src=https://github.com/user-attachments/assets/cbabae17-d121-4a75-a317-5960415221ca width="400">

- Organized semantically 
- Users can drag and drop both in GUI and code
- The engines would draw from the same parsers but have different exposed  outputs based on the property

<br clear="right"/>



<br clear="left"/>
<br>
<br>



[^1]: (Tara): based on all the conversations I have had in the previous months with various expreimentalists, informatic exprts, industry reps, simulation scientists from other scales etc.

# Specs for interfacing architecture & infrastructure components
In this section, we aim to provide specifications on how pyiron interfaces with various IT-architecture and -infrastructure components. This includes (the ordering is rather random and more details to each point can be found in the respective subsection below):
- Running pyiron on HPC systems. This includes setup of various configs (so, a.t.m. running jupyter hub on a cluster node or setting up a "remote HPC" queue-interface). We should also make clear, what typical policies imposed by computing centers/ IT departments  must be considered in that context.
- A statement on which containerization technologies (Docker, podman, kubernetes, ...) are "officially" supported/ populated with base images by us. **This also interfaces with the channels/registries we want to use for distributing software (see branch "distribution")**
- Requirements and best practices for creating (Docker) containers based on the images provided by us. This might also include a recommendation on the runtime environment to be used

## Jupyter
- jupyter notebook are a.t.m. the preferred way to interface interactively with pyiron
- outline requirements of a jupyterhub deployment for running pyiron modules

## HPC systems
- typical computing-centre policies affected by a pyiron setup
- supported queuing systems (SLURM, SGE, ...)

## Containerization: creating images, distributing and defining specs for runtime environments
- which containerization technologies (Docker, podman, kubernetes, ...) we support with "official" base images. **This also interfaces with the channels/registries we want to use for distributing software (see branch "distribution")**
- Requirements and best practices for creating (Docker) containers based on the images provided by us

To make this aspect more tangible, we could refer to and make use of specifications defined be the Open Container Initiative:
- Acquire [OCI-conform](https://opencontainers.org/) images, their distribution and runtime envs: this ensures that any runtime (e.g. a jupyterhub deployment or "docker-like" backends like podman or kata containers) conform to OCI specifications can actually build and run containers from the images. This can increase interoperability significantly. Links: [OCI image specs](https://specs.opencontainers.org/image-spec/?v=v1.0.1), [OCI distribution specs](https://specs.opencontainers.org/distribution-spec/?v=v1.0.0), [OCI runtime specs](https://specs.opencontainers.org/runtime-spec/?v=v1.0.2). This whole point might also be very interesting in the FAIR context and for the usability of pyiron in digitization projects.

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

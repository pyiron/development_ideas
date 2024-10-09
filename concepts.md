# Definitions

- A node is an atomic unit of a workflow which executes an underlying function using one or more inputs to return one or more outputs.
- The pyiron core modules are the user interfaces `pyiron_xyflow`, `pyiron_workflow` and `pyiron_base` as well as the underlying modules like the newly defined storage module and database module plus `executorlib`.

# Concepts [^1]

- pyiron is simple
  - Any python function can be turned into a node in one line [^2]
- pyiron is interoperable
  - As long as the underlying function can run on different platforms, pyiron can run the node on any platform.
- pyiron is HPC-compatible
  - Any node can be submitted to the queueing system
- pyiron keeps data
  - The input and the output of any node can be stored in a file system and loaded after the session is closed
- pyiron allows for collaboration
  - Nodes can be imported via the conventional python `import`-statement
- pyiron is modular 
  - The users should have the option to select which of the pyiron core modules they want to use.
- pyiron tracks the source
  - Users can add the DOI of the underlying methods/software packages to the node
- pyiron understands physics
  - Nodes as well as their inputs and outputs can be provided with ontological types
- pyiron workflows are reproducible
  - workflows (classic & node-based) are reproducible via a well defined runtime environments.

# User Groups
While `pyiron` originally only addressed the atomistics community, the new generation of `pyiron` has a much wider range of potential users. They can be roughly defined into the following groups:

* **performance oriented expert user**: A user interested in maximum performance might just use `executorlib` and develop their own minimalistic storage solution to only store the material properties they calculated without storing the results of individual calculation. 
* **minimalist**: A user who previously used `snakemake` switches to use `pyiron_base` based on its minimalistic syntax which internally uses the newly defined storage module and database module plus `executorlib` and the auto-completion based user interface `pyiron_base` introduced. 
* **workflow expert**: A user interested in graph based workflows but without access to a Jupyter environment on their HPC might use `pyiron_workflow` which internally optionally uses `executorlib`, and in the future uses the forthcoming database module and optionally uses the forthcoming storage module.
* **beginner user**: A user who has less experience with python programming and enjoys the overview provided by an visual programming interface might use `pyiron_xyflow` which is based on `pyiron_workflow` which internally uses the newly defined storage module and database module plus `executorlib`.

The different user groups use a different selection of pyiron core modules. 


# Interoperability

Interoperability is a core concept of pyiron development. This point appears in all of the FAIR principles, but since workflows are considered here, this should be subdivided into different categories:

- Software interoperability: both classical pyiron and pyiron_workflow can be used to provide a generic or unified interface between software tools. Classical pyiron provides this with pyiron jobs while the pyiron_workflow provides it with nodes that encapsulate code execution.
- (Meta)Data interoperability: data generated with pyiron is lacking interoperability at the moment, see details I1 - FAIR in fair_assessment.md.
- Workflow interoperability: A workflow is interoperable with other platforms. For classical pyiron, this is somewhat complicated as there is no strict definition of the form and structure of a workflow. Operations can be performed outside of pyiron jobs. While this is great for rapid prototyping, it also means that a workflow cannot be fully reproduced. For pyiron_workflow, the complete provenance can be captured. If export to other formats such as CWL (or even a non code representation) is available, it can be considered interoperable with other platforms. Note that pickling is not a non-code representation.

Here there are several activities that needs to discussed to form a full strategy (collaborations with Aiida, NOMAD, CWL interoperability, Kadi4Mat, and Common Workflow Description for experiments, for more details see I1 - FAIR4RS in fair_assessment.md).

# Collaboration
At the moment, we can suggest a prototypical collaboration-setup [graphathon](https://github.com/mbruns91/graphathon) in the form of a github repository-template. We need some discussion on in which direction this should be further developed: What are our requirements for a collaboration platform? What are the use-cases? What backend should a more production-ready version use?

# Reproducibility
pyiron workflows can be published/deployed/shared/... in a reproducible way on two levels:
- **Conda environment**: by providing notebook(s) and an `environment.yml`, users can set up similar conda environments for executing a workflow.
- **Containerization**: based on the Conda environemnt approach, we can also go one step further and pack the workflow and the whole runtime environment in a container image. When Docker is the backend of our choice, such containers can be based on  the flavors we provide via [pyiron/docker-stacks](https://github.com/pyiron/docker-stacks). Then, when the container images are built, a simple `docker run` command is enough to be provided with jupyterhub, a readily-configured condaenvironment and an isolated file-system containing all neccessary files (notebooks, datafiles, ...). During the building process, docker allows for basically anything that can be done with a terminal (e.g. cloning of repositories or the execution of shell-scrips) for a workflow to be reproduced automatically.

[^1]: Currently the list does not follow a specific order.
[^2]: Currently not true for functions which might return very different types and has multiple return statements.

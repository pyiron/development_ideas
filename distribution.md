# Repository Structure, Management, and Frontend Integration for Nodes
This write-up is based on the strategy hackathon, and describes the repository structure, node management guidelines, and a frontend system to streamline interaction with the GitHub organization and node store. Additionally, it incorporates a review process for quality assurance and certification of nodes. The main aim of a node is to foster collaboration while ensuring ease of use and integration.

- Researchers worldwide are encouraged to contribute their scientific functions to the Pyiron community.
- These functions can be published as independent software packages, for example, releasing the package on platforms like PyPI.
- Accompanying the release with a publication in journals like the Journal of Open Source Software (JOSS).

At the moment, nodes will live as repositories under an umbrella organisation. The requirements of the repositories are below. This will be initial version of the node store. Ideally, node store would be a frontend, where all the nodes from the GitHub organisation are indexed, and there are searchable, and installable. An example is the [PMD Workflow store](https://workflows.material-digital.de/). We (ideally) have a lot of functionality to ensure good metadata for nodes. This should all be done with the help of CI. 

## Node Organization and Storage

### Repository Structure:
- Node Repositories:
  - A node is stored in its own repository.
  -  Multiple nodes can exist within a single repository if:
      - Dependencies are same/similar.
      - Nodes are scientifically connected (e.g., tied to a specific publication).
  - Complex nodes (e.g., involving C-bindings) are stored in a repository but published via conda-forge.

- Import and Storage Rules:
  - Imports:
    -  Nodes must contain their own imports. The mechanism for importing annotations remains unresolved.
  -  Storage Format:
    -  Nodes should use semantic ordering via entry points for structured storage and discovery.

### Interface and Data Classes
-  Node Interface:
    -  Each repository will include data classes required by the nodes.
    -  Enhancements to these data classes can be contributed via pull requests to the main data class repository.

## Repository Publication and Distribution

### Package Distribution

-  Conda Publication:
    -  Nodes are published to a dedicated Conda channel (later, we can start with conda-forge).
    -  Repositories are hosted in a dedicated GitHub organization.
-  Authorship and Maintenance:
    -  Ownership: The submitting author is the maintainer and is responsible for updates and security.
-  Versioning:
    -  Semantic versioning (e.g., v1.0.0) or date-based versioning (e.g., 2024.11) will be chosen.
-  Automation:
    -  GitHub bots will automate version updates and routine tasks.

## Frontend for Node Indexing and Management

## Core Features
-  Node Indexing:
    -  Integrates with the GitHub organization to fetch and index repositories.
    -  Metadata, scientific relationships, and dependencies are cataloged.
-  DOI Generation:
    -  Assigns DOIs to nodes using a DOI provider (e.g., DataCite, Zenodo) for traceability.
-  Search and Discovery:
    -  Search nodes by keywords, tags, dependencies, and publications.
-  Documentation Display:
    -  Automatically fetches README files, citation information, and associated publications.
-  Download and Install:
    -  Provides direct installation instructions (e.g., conda install <node>).

## Template Repository Setup
A template repository ensures consistent structure across nodes:

-  Metadata:
    -  codemeta.json (JSON-LD format) and CITATION.cff.
-  GitHub Config:
    -  .github directory for CI/CD workflows.
-  Code Organization:
    -  Directories for unittest, docs, and importable node files.
    -  README, environment.yml, and security guidelines.

## Security and Maintenance
-  Responsibilities:
    -  Maintainer Accountability: Maintainers are responsible for the node’s integrity, updates, and security.

---

From here, follows the discussion from pre-hackathon discussions:

# Distribution of Functions 
- Scientific functions should be contributed from researchers worldwide.
- The researchers should be able to publish their scientific functions as independent software package, for example by releasing it on [pypi.org](https://pypi.org) and writing a paper for the [journal of open source software](https://joss.theoj.org).
- The wrapper to maximize the utility in the pyiron context should be minimal.
- pyiron should be capable to track and ideally synchronize the functions which are available on different computing systems.
- more complex ecosystems of software (when not only pyiron and other python modules are used) might be best packed and distributed via container images (e.g. Docker or kubernetes). Technical hints, conventions and best practices belong to the section "architecture and infrastructure", so have a look at that branch for this. Here, it is more about making clear what *backends* we support "officially" (Docker, podman, kubenetes, ...) and what the publication *channels* are (continer registries like dockerhub or quay.io).
  - containerization also relates to solutions like binder (mybinder and most importantly the workshop-environments provided by mpcdf)

# Discussion in the spec meeting

A node should be in a repository.
Multiple nodes might be in one repository if
- the dependencies are individual
- the nodes are scientifically connected (related to a scientific publication)

macro nodes are also stored in a repo
more comlex code with C-bindings or the like need to go to conda-forge, the code needs to be **pure python**
import should be in the nodes
unsolved: import for annotations
node should be stored in a semantic ordering using entrypoints

The repository is published to `conda`
- own channel for our nodes
- our own github org for the repos
- the person submitting the code to the channel, is the author of the node and is maintained by the author
- semantic versioning vs dated version - not decided
- automation using github bots to update versions

How to build the interface for our nodes.
We distribute the datatclasses required for our nodes together with the nodes in the repo, good additions to dataclasses can be PRed into the main dataclass repo.

We need an easy template repo with the following:
- codemeta metadata json-ld baesed on a config yml file
- citation cff
- .github with CI
- unittest direcory
- docs
- README
- environment.yml
- file of importable nodes

Security:
- maintainer is responsible

- Lead Sarath

## Challenges [^1]

An idea could be that individual nodes can have their own environments (see [here](https://github.com/orgs/pyiron/discussions/244#discussioncomment-10645436)) with another environment for the underlying workflow itself. This would make solving dependecies much easier but it could pose challenges to github (tests, bots...) if researchers upload nodes too frequently to the repo. This would need a filtering mechanism to decide which node finds it's way to the repo, similar to the filtering mechanism to decide which nodes get their own unique object type in opeBIS. In the case of openBIS, the criterion could be that we need a well written docstring. Perhaps we would need a similar criterion for the pyiron_nodes repo as well.

How do we organize the nodes repo? It currently has all atomistic nodes in one folder. A [branch](https://github.com/pyiron/pyiron_nodes/tree/1ec7b903fa932a2f4e45e3dd9605e5f85298b070/pyiron_nodes) which also has folders for large scale atomistics and fem exists. Would it make sense to have a finer structure based on scale/application so that users can easily download whatever they require (similar to the different repos we have now)
- atomistic_generic: just the generic stuff
- atomistic_dpd: streamlined for defect phase diagrams
- atomistic_electrochem: streamlined for electrochemistry
- atomistic_solidstate: streamlined for band structure, magnetism etc.
- atomistic_potentialfit: for fitting potentials
- atomistic_large_scale: for millions of atoms
- continuum_crystal_plast: for crystal plasticity stuff
- continuum_phase_field: for phase field stuff
- continuum_fem_multiscale: for scale bridging stuff
- continuum_material_model: for geenrating model parameters
- continuum_coupled: for complex multiphysics simulations
- and so on...

But this has the problem that the node you need is in another folder. E.g., the node for rotating the elasticity tensor is currently in the atomistics_large_scale folder. But this node would also be super-useful for the continuum side of stuff. So a fem user would have to find and download from the folder for large scale atomistics unless we have duplicates everywhere. This raises the question: should we organize folders based primarily on properties with the scales as subfolders?

* Mechanical:
  - elasticity_atomistic
  - elasticity_continuum
  - elasticity_experimental
  - elasticity_utility (the function for rotating the elasticity matrix would belong here)
  - plasticity_atomistic
  - plasticity_continuum
  - plasticity_experimental
  - plasticity_meso (ddd stuff maybe)
  - plasticity_utility
  - and so on
* Electronic:
  - band_structure_atomistic
  - band_structure_experimental
  - band_structure_utility
* and so on


## Ease of use for the end user (NFDI-MatWerk perspective) [^2]

The current status of pyiron in MatWerk can be summarised in the following activities:

- IUC04: https://doi.org/10.5281/zenodo.13220179
- IUC07: https://doi.org/10.5281/zenodo.12687143
- IUC09: https://doi.org/10.5281/zenodo.12594062
- IUC10: https://nfdi-matwerk.de/project/structure/use-cases/iuc10
- IUC17: https://doi.org/10.5281/zenodo.12207313

The majority of these use cases deal with specific topics, for example, APT analysis, experimental workflows, atomistic simulations. I have identified some common requirements based on the feedback from the collaborators:

- Packages should be as lean as possible with minimum requirements. This brings questions related to distribution of nodes within pyiron_workflow. Would the strategy still be to include all nodes in a node library package? This would mean that nodes for APT analysis (paraprobe-toolbox) and nodes for calphy would have same dependencies. 
- Software tools should ideally have web based documentation, and not uniquely docstrings.
- Installation should be easy as possible, i.e. setting up of config files, data folders, executable scripts should be avoided as much as possible.
- Each node/job should create an execution folder, and store the produced data objects, and execution records within. This is currently in pyiron_workflow, and would be a very nice feature to have (even if optional).

These requirements should be met by all software tools developed within pyiron to facilitate the broader adoption in the MSE community.

## Further thoughts on the distribution of functions: Using GitHub repositories for nodes[^3]

In addition to the structure of the GUI representation mentioned in the [^1] section. It needs to be evaluated how to store the functions and macros. We need to store on a function level to allow other workflow solutions to pick up and use the functions.

We could store each of the function (packages) in an individual repository within a github organization. We develop a template repository for the node-containing function including

- a python package with all the functions to be wrapped as nodes included and exposed in the `__init__.py` to allow for automatic wrapping.

- The workflow definitions needed for the macro creation, probably simply as `pyiron_workflow` code for now. If there is a convertable workflow description  (WD) at some point, one could switch to that (prequires a pyiron_workflow <-> WD converter)

- An `environment.yml` to clarify the dependencies of the nodes

- A metadata containing file for additional information about the (macro) nodes
  
  

These repos could be converted into `conda-forge` feedstocks making all of the packages installable by `conda`. This alsow allows for dependency management on the installation level.

Probably, we will need (semi) atomated meta-packages to install a bunch of nodes in one go (e.g. add all nodes taged `atomistics` to the `pyiron_atomistics_nodes` meta-package). These meta-packages, could also be used to expose the nodes in at the correct semantic import locations, like discussed above concerning the structure of the node store.

To ease community contributions, it would be possible to collect all public repos on GitHub containing a specific tag (e.g. `workflow_node_repo`) and list them as a node-store. This would lift the 'all into one organization' restriction, however, malicious code could enter the framework, if one blindly uses nodes of untrusted sources. (Simply imagine a node in a nice, scientifically relevent and accurate macro package sending the content of the `.ssh` folder to an external server...)

[^1]: (Tara) I have added these points as open questions for discussion
[^2]: (Sarath) I have added these points in the context of NFDI-MatWerk and my concerns for the distribution.
[^3]: (Niklas) I added these after some discussions at the NFDI-MatWerk strategy meeting.


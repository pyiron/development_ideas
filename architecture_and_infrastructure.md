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
- Acquire [OCI-conform](https://opencontainers.org/) images, their distribution and runtime envs: this ensures that any runtime (e.g. a jupyterhub deployment or "docker-like" backends like podman or kata containers) conform to OCI specifications can actually build and run containers from the images. This can increase interoperability significantly. Links: [OCI image specs](https://specs.opencontainers.org/image-spec/?v=v1.0.1), [OCI distribution specs](https://specs.opencontainers.org/distribution-spec/?v=v1.0.0), [OCI runtime specs](https://specs.opencontainers.org/runtime-spec/?v=v1.0.2). This whole point might also be very interesting in the FAIR context and for the usability of pyiron in digitalization projects.

---

# Summary from the hackathon discussion
## Guidelines & recommendations for individual and group-level installations
**Examples for stable environments can be found in our docker-stacks repository.**

### individual (workstation) installations
See the guide(s) on our website(s).

### Group-level (cluster) installations
- Minimal supported requirements for `pyiron_workflow`:
  - user-specific miniconda installation:
    - jupyter**lab**
    - `pyiron_\*` packages
    - sqlite (optional)
  - Advanced setup
    - all from above
    - shared conda environments (**give hints/ guide how to do that**)
    - postgesql (optional)
  - Recommended setup: from cmti
    - all from above
    - Jupyter**hub**
    - postgresql
- Additional infrastructure
  - openBIS
  - CKAN
  - NOMAD
  - ...

## Installations for workshops/ student access /ready-to-use envs
- e.g. as alternative for working on a login-node
- easier to ensure authority of resp. institution compared to workstation/ laptop installation
- Docker based: docker-stacks
  - PMD-S deployment (jupyterhub (+ keycloak?))
  - JupyterHUB (MPCDF)
  - mybinder (of MPCDF)

---
## to do
### "casual" installation
- [ ] revise and write all related documentation
  - workstation/ laptop
  - group-level
    - user-specific
    - group-level
  - mention related conda best-practices
  - admin-oriented

### docker-based
- [ ] refactor existing images docker-stacks and create "missing" new ones
  - [ ] `pyiron`
  - [ ] `base`
  - [ ] `continuum`
  - [ ] `experimental`
  - [ ] `md`
  - [ ] `mpie_cmti`
  - [ ] `potentialworkshop`
  - [ ] `ontology`
  - [ ] `electrochemistry`
  - [ ] `workflow`
  - more required?
  - which ones can be archived?
  - clear statements, what each image's purpose is
- [ ] refactor `pyironhub` base-image (the one used in PMD)
- [ ] deployment-guides for setting up
  - [ ] docker-based `pyironhub`
    - [ ] connecting keycloak (or other user-management service)
  
## resources
- pyiron.org
- pyiron-readthedocs
- executorlib documentation
- cluster documentation (CM gitlab)
- PMD deployment-guide

Lead: Marian

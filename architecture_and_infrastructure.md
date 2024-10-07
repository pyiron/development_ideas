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
- Acquire [OCI-conform](https://opencontainers.org/) images, their distribution and runtime envs: this ensures that any runtime (e.g. a jupyterhub deployment) conform to OCI specifications can actually build and run containers from the images. This can increase interoperability significantly. Links: [OCI image specs](https://specs.opencontainers.org/image-spec/?v=v1.0.1), [OCI distribution specs](https://specs.opencontainers.org/distribution-spec/?v=v1.0.0), [OCI runtime specs](https://specs.opencontainers.org/runtime-spec/?v=v1.0.2). This whole point might also be very interesting in the FAIR context and for the usability of pyiron in digitization projects.

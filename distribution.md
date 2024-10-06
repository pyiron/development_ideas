# Distribution of Functions 
- Scientific functions should be contributed from researchers worldwide.
- The researchers should be able to publish their scientific functions as independent software package, for example by releasing it on [pypi.org](https://pypi.org) and writing a paper for the [journal of open source software](https://joss.theoj.org).
- The wrapper to maximize the utility in the pyiron context should be minimal.
- pyiron should be capable to track and ideally synchronize the functions which are available on different computing systems.
- more complex ecosystems of software (when not only pyiron and other python modules are used) might be best packed and distributed via container images (e.g. Docker or kubernetes). Technical hints, conventions and best practices belong to the section "architecture and infrastructure", so have a look at that branch for this. Here, it is more about making clear what *backends* we support "officially" (Docker, podman, kubenetes, ...) and what the publication *channels* are (continer registries like dockerhub or quay.io).
  - containerization also relates to solutions like binder (mybinder and most importantly the workshop-environments provided by mpcdf)

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

[^1]: (Tara) I have added these points as open questions for discussion
[^2]: (Sarath) I have added these points in the context of NFDI-MatWerk and my concerns for the distribution.
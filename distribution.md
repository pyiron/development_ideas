# Distribution of Functions 
- Scientific functions should be contributed from researchers worldwide.
- The researchers should be able to publish their scientific functions as independent software package, for example by releasing it on [pypi.org](https://pypi.org) and writing a paper for the [journal of open source software](https://joss.theoj.org).
- The wrapper to maximize the utility in the pyiron context should be minimal.
- pyiron should be capable to track and ideally synchronize the functions which are available on different computing systems.
- more complex ecosystems of software (when not only pyiron and other python modules are used) might be best packed and distributed via container images (e.g. Docker or kubernetes). Technical hints, conventions and best practices belong to the section "architecture and infrastructure", so have a look at that branch for this. Here, it is more about making clear what *backends* we support "officially" (Docker, podman, kubenetes, ...) and what the publication *channels* are (continer registries like dockerhub or quay.io).
  - containerization also relates to solutions like binder (mybinder and most importantly the workshop-environments provided by mpcdf)

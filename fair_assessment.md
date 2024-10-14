# FAIR assessment of pyiron

## pyiron as a software tool: [FAIR4RS](https://doi.org/10.15497/RDA00068)
  
### Findable: Software, and its associated metadata, is easy for both humans and machines to find

#### F1. Software is assigned a globally unique and persistent identifier.  

- F1.1. Components of the software representing levels of granularity are assigned distinct
Identifiers.  

    Distinct identifiers are needed for each package - all pyiron packages have the original pyiron paper as citation. CFF files can be added keeping the original paper as the preferred citation, while having distinct PIDs for each package. 

    *Action points:*
    * *Integration of each pyiron package with Zenodo to provide DOI*
    * *CFF files that correspond to the Zenodo integration*

    Classic pyiron ecosystem has different repos based on the granularity such as ‘base’, ‘atomistics’. In the case of workflow, node library would need to have more granularity depending on the topic.

    *Action points:*
    * *Granularity in node_library*

- F1.2. Different versions of the software are assigned distinct identifiers.

    Each release is semantically versioned and has a conda/pypi package, and GitHub url that corresponds to it. However, similar to the previous point, integration with Zenodo would provide PIDs for each release. This is for example, a necessary criteria for publishing software in JOSS.

    *Action points:*
    * *Integration of each pyiron package with Zenodo to provide DOI*

#### F2. Software is described with rich metadata.

Currently, pyiron has ‘pyproject.toml’ files as a source of human/machine readable metadata. For rich metadata, recognised community standards are CITATION.cff and codemeta.json files.

*Action points:*
* *Improve metadata with codemeta.json*

#### F3. Metadata clearly and explicitly include the identifier of the software they describe.

*Action points are the same as F1*

#### F4. Metadata are FAIR, searchable and indexable.  

Having codemeta.json files in combination with Zenodo releases would ensure that the metadata files itself are FAIR.

*Action points are the same as F1 and F2*

### Accessible: Software, and its metadata, is retrievable via standardized protocols.

#### A1. Software is retrievable by its identifier using a standardized communications protocol

- A1.1. The protocol is open, free, and universally implementable.  
    This requirement is fully met.

- A1.2. The protocol allows for an authentication and authorization procedure, where necessary.  
    This requirement is fully met.

#### A2. Metadata are accessible, even when the software is no longer available.
See F4

### Interoperable: Software interoperates with other software by exchanging data and/or metadata, and/or through interaction via application programming interfaces (APIs), described through standards.

#### I1. Software reads, writes and exchanges data in a way that meets domain-relevant community standards.

This point includes the use of data and metadata types, controlled vocabularies, and formats that are formally defined through standards to facilitate the exchange. In terms of data files, using hdf5, and other metadata formats as discussed above meets this point.
In terms of workflow concepts, this agreement is needed. This could be achieved by using provenance ontologies, or interoperability with other workflow definitions such as CWL; and domain relevant tools such as Aiida, Kadi4Mat etc.

*Action points:*
* *Strategy discussion is needed to define action plan*
* *Report from ADIS activities (Jan)*
* *Report from NOMAD initiatives (?)*
* *Report from CWL interoperability aspects (Sarath)*
* *Report from Common Workflow Description with experiments (FZJ, Brinckmann)*

#### I2. Software includes qualified references to other objects.

This is partially met through software dependencies, and by automated testing on different platforms. However, could be strengthened by using provenance concepts, leading to a complete (ontological) description. 

*Action points are the same as I1.*

### Reusable: Software is both usable (can be executed) and reusable (can be understood, modified, built upon, or incorporated into other software).

All pyiron developments fully meet the requirements. 

#### R1. Software is described with a plurality of accurate and relevant attributes.

- R1.1. Software is given a clear and accessible license.
- R1.2. Software is associated with detailed provenance.

#### R2. Software includes qualified references to other software.
#### R3. Software meets domain-relevant community standards.


## pyiron as a data generator - [FAIR data principles](https://www.go-fair.org/fair-principles/)

### Findable
#### F1. (Meta)data are assigned a globally unique and persistent identifier

Not necessarily relevant for pyiron itself, but is a relevant point to consider when discussing integration with OpenBis/CKAN.

#### F2. Data are described with rich metadata (defined by R1 below)

Pyiron could improve the metadata of the generated simulation data to a large extent. Specifically for simulation data, the consensus is that the metadata should be automatically generated. The metadata could be subdivided into *intrinsic* (captured by the machine), *contextual* (physical concepts), and *administrative* (who and when?)

* For classical pyiron the data is in hdf5 files, while some administrative metadata is stored in the project table. 
* For workflow, since there is no data model, this point is not met.
* Contextual metadata is limited to DOIs of publications, except units which could be well handled using ‘uniton’ (Sam)
* The physical concepts can be strengthened using the ‘conceptual dictionary’, and aligning to controlled vocabularies, i.e. ASMO, CMSO, etc, at least on the atomistic scale.
* Other scales can be fulfilled in the relevant projects (Marian and Sam) 

#### F3. Metadata clearly and explicitly include the identifier of the data they describe

At the moment, data and metadata are stored in separate places and not linked (for example, project table and job data). This could be linked through the creation of PIDs, or by storing in a common data model.

#### F4. (Meta)data are registered or indexed in a searchable resource

Only relevant if pyiron is creating a database of simulations. Otherwise it is the responsibility of the data owner.

### Accessible

Responsibility of data owner.

#### A1. (Meta)data are retrievable by their identifier using a standardised communications protocol

- A1.1 The protocol is open, free, and universally implementable
- A1.2 The protocol allows for an authentication and authorisation procedure, where necessary
A2. Metadata are accessible, even when the data are no longer available

### Interoperable

#### I1. (Meta)data use a formal, accessible, shared, and broadly applicable language for knowledge representation.

Main goal is to provide a “common understanding” of datasets by means of a language for knowledge representation (recommended are JSON LD, RDF, OWL) to be used to represent these objects. Here, one could: represent the administrative metadata, for example using Dublin Core Schema. Or, express the intrinsic metadata of the provenance using PROV-O. Another option is the conceptual dictionary of domain relevant contextual metadata using JSON LD.

#### I2. (Meta)data use vocabularies that follow FAIR principles

Responsibility of the end user, however, any controlled vocabulary for metadata that pyiron uses should follow the FAIR principles. This would exclude solutions that create terminology/vocabularies at code execution time. Furthermore, this makes it explicit that any data model that pyiron uses should be defined and accessible in places other than just the code base. 

#### I3. (Meta)data include qualified references to other (meta)data

See I1 and F3

### Reusable

#### R1. (Meta)data are richly described with a plurality of accurate and relevant attributes

- R1.1. (Meta)data are released with a clear and accessible data usage license  
    Responsibility of the data owner

- R1.2. (Meta)data are associated with detailed provenance  
    See F2, I1 and I1 (FAIR4RS)

- R1.3. (Meta)data meet domain-relevant community standards  
    See F2, I1 and I1 (FAIR4RS)


## pyiron as a generator of computational workflows - [FAIR for workflows](https://arxiv.org/pdf/2410.03490)

FAIR principles apply to workflows as a combination of both FAIR data and FAIR research software. This means that there is a distinction between the workflow as a digital object, and the data that it generates.

### Findable

#### F1. A workflow is assigned a globally unique and persistent identifier

- F1.1 Components of the workflow representing levels of granularity are assigned distinct identifiers.  
    
    Classical pyiron: there are two levels of granularity - one are Jobs which are workflows themselves, and shipped together with other pyiron packages. They benefit from fulfilling the requirements in F1 (FAIR4RS)
    The other option is using the pyiron workflow template. In this case, the requirement could be met by archiving this in Zenodo, workflowhub etc; and by versioning.

    pyiron_workflow: Granularity is captured in terms of nodes and macros. However, this point would indicate that having different repositories with nodes based on the given granularity (in terms of scientific themes, for example) would be a solution. This way, each of them could be versioned, and stored in a repository, and additionally in an archiving platform.

    *Action points:*
    * *Strategy discussion is needed to define action plan*
    * *for pyiron workflow template, recommend archiving on platforms*
    * *for pyiron workflow template, recommend versioning*
    * *for pyiron_workflow, separating nodes based on their utility and treating them as individual software packages would be a solution.*

- F1.2 Different versions of the workflow are assigned distinct identifiers.

    *Action points same as F1 above*

#### F2. A workflow and its components are described with rich metadata.

*Action points same as F2 (FAIR4RS)*

#### F3. Metadata clearly and explicitly include the identifier of the workflow, and workflow versions, that they describe.

*Action points same as F2 (FAIR4RS)*

#### F4. Metadata and workflow are registered or indexed in a searchable FAIR resource.

*Action points same as F1.1 above*

### Accessible

#### A1. Workflow and its components are retrievable by their identifiers using a standardized communications protocol

- A1.1 The protocol is open, free, and universally implementable.
- A1.2 The protocol allows for an authentication and authorization procedure, when necessary.

*Action points:*
    * *for classical pyiron: the workflow (imagine workflow template) itself should be available through pip/conda for this to be satisfied.*
    * *for pyiron_workflow, treating nodes and macros as software packages available through conda would address this point*

#### A2. Metadata are accessible, even when the workflow is no longer available.

*Action points:*
    * *In addition to above, archiving on Zenodo/WorkflowHub will adress this issue.*

### Interoperable

#### I1. Workflow and its metadata (including workflow run provenance) use a formal, accessible, shared, transparent, and broadly applicable language for knowledge representation.

* In classical pyiron, workflows are the jupyter notebooks itself, and processing of data that would be done outside of a pyiron job is not captured. This would be needed to capture full provenance.

* In pyiron_workflow,  workflows are well defined. If with the help of a data model, it can capture all operations performed on the data, it can lead to full provenance. This is indeed a major strength of pyiron_workflow. See I1 for FAIR4RS and FAIR.  
This also calls for a non-code based representation of workflows, for example in JSON, xml etc. Automated provenance collection can be too fine-grained and too detailed, therefore ontological abstractions can be helpful in reusability.

* atomRDF and conceptual dictionary (for both classical pyiron and pyiron_workflow) also partially tackles this point as they provide a knowledge representation.

*Action points:*
    * *strategy discussion needed*
    * *Standardised representation for workflows needed.*


#### I2. Metadata and workflow use vocabularies that follow FAIR principles.

* See above, once again, atomRDF, conceptual dictionary, unitton are all developments that can help towards this. 

#### I3. Workflow is specified in a way that allows its components to read, write, and exchange data (including intermediate data), in a way that meets domain-relevant standards.

* In classical pyiron, the data can be separated by the ‘pack’ methods into separate files. HDF5 files are interoperable format, supporting them with ontology based identifiers would be fully meet this point.

* In pyiron_workflow, we do not have a data model at the moment. Implementing this, along with data identifiers would address this point.

*Action points:*
    * *strategy discussion needed*
    * *Implementation of ontology based identifiers in data models*
    * *(Optional) data model in pyiron_workflow*

#### I4. Workflow and its metadata (including workflow run provenance) include qualified references to other objects and the workflow’s components.

*Action points same as I4 (FAIR4RS), additionally provenance representation needed*

### Reproducible

#### R1. Workflow is described with a plurality of accurate and relevant attributes.

- R1.1 Workflow is released with a clear and accessible license.
- R1.2 Components of the workflow representing levels of granularity are given clear and accessible licenses.
- R1.3 Workflow is associated with detailed provenance of the workflow and of the products of the workflow.

*Action points:*
    * *Same as F1*
    * *Additionally, provenance is needed, see above*

#### R2. Workflow includes qualified references to other workflows.

In pyiron_workflow, this would be automatically met in separation into nodes and macros.

#### R3. Workflow meets domain-relevant community standards.

Workflow reuse could mean rerun, replication, reproduction, repurpose. This calls for strong separation of workflow as digital objects, inputs of the workflow, and the generated data. Automated testing of workflow nodes are needed. 

## Self-assessment according to [FAIR checklist](https://fairsoftwarechecklist.net/)

Pyiron

[![FAIR checklist badge](https://fairsoftwarechecklist.net/badge.svg)](https://fairsoftwarechecklist.net/v0.2?f=30&a=31110&i=22300&r=133)

A workflow instance generated in pyiron

[![FAIR checklist badge](https://fairsoftwarechecklist.net/badge.svg)](https://fairsoftwarechecklist.net/v0.2?f=10&a=10010&i=20300&r=112)

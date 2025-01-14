Meeting weekly: Wednesdays 11AM, [link](https://gwdg.zoom.us/j/84400921753?pwd=VGtjTjFrNnF6OWZxc3UySWV4cS8yUT09), [repo](https://github.com/pyiron/pyiron_rdm)

# Publication of Scientific Data
A scientist who used pyiron to calculate material properties using standardized simulation protocols wants to publish the results of these calculation. Ideally, there should be no difference in the publishing process no matter if the scientist used `pyiron_base` or `pyiron_workflow`.

Potential platforms users from the (atomistic) community might be interested to use:
* https://zenodo.org
* A local installation of https://ckan.org
* A local installation of https://openbis.ch - primarily to share simulation results with collaborators
* https://nomad-lab.eu/nomad-lab/nomad-oasis.html
* https://www.aiidalab.net

## Strategy for publishing data

The current strategy for openBIS can be seen [here](https://github.com/pyiron/pyiron_rdm/blob/main/README.md). Examples of object types for openBIS can be seen [here](https://github.com/pyiron/pyiron_rdm/issues/9#issuecomment-2320749384). The ongoing work for the conceptual dictionaries can be seen [here](https://github.com/pyiron-dev/pyiron-conceptual-dict), with an example of a conceptual dictionary for a lammps job [here](https://github.com/pyiron-dev/pyiron-conceptual-dict/blob/main/lammps.ipynb). However, I think this figure is worth displaying here again since it forms the core of the current stratgey:
![image](https://github.com/user-attachments/assets/e57b4e2c-c34f-4456-9b1d-7ecde45ed4ae)

In the above figure, the ontologies could be replaced by other ontologies, but the strategy remains the same. The various platforms can also be replaced, but the strategy still remains the same with just different degrees of coarse-graining and different types of metadata filtering from the conceptual dictionary. In other words, the conceptual dictionary is the main "mapping" between pyiron and the platforms. This could mean different types of platforms like openBIS, ckan etc. or even different instances of the same platform (e.g., BAM-openBIS, SFB1394-openBIS etc.). An example of the data mode for a Murnaghan workflow in the BAM-openBIS instance is shown in the figure below.
![image](https://github.com/user-attachments/assets/e3f94662-cfec-4aa2-9a79-458cb2bc8f85)



For `pyiron_atomistics` jobs, the current method to extract the conceptual dictionary is done using the `to_dict()` method. It would be relatively straightforward to shift to the dataclasses. But `pyiron_workflow` is a whole different beast. The dataclass nodes would of course prove useful, but may lack contextual information on how they fit into the worklow graph. For this we may need a `wf.get_master_dataclass` or something that collects all the sub-dataclasses, inputs and outputs of nodes, and also maybe the functions themselves as strings or some such.

## Options for uploading data

Pyiron should be able to users with the following options to upload data to the platforms:
- Upload the data to the storage attached to the platform (probably some type of file system)
- Upload the data to a separate cloud storage (e.g., S3) and provide a link on the platform
- Upload only metadata which should at least be enough to reproduce calculations

## Challenges

* With the "open beta" release of `pyiron_workflow`, we would need to agree on a relatively concrete method to decide which nodes/graphs receive the conceptual dictionary treatment and a unique object type. One possible requirement could be that the node has to contain a proper docstring (which may also have useful literature references for the science behind the function/macro). Another requirement could be that the node is available on the *main* branch of the pyiron_nodes repo.
* Platforms like ckan have a limited number of datasets that they can handle, a few thousands at most. We would need features that can identify semantically similar workflows and pack them together. [^1]
* What is a material?? How do we let the publishing platform know in an automated manner that our simulations should be linked/tagged to nickel superalloy type 718, or steel grade 316. Such a feature would invaluable to connect our data with the experimentalists.

## FAIR principles [^2]

A major point in data publication is FAIR compliance. A detailed assessment is provided in fair_assessment.md, specifically in 'pyiron as a data generator'. The main points would be:
- metadata harmonisation with community standards, e.g. the implementation of an updated publication template.
- use of PIDs and interlinking data and metadata files.
- interoperability with the (meta)data formats in the aforementioned initiatives (DataCite, NOMAD, etc.)

## Milestones

1. Conceptual dictionaries available for a certain amount of basic job/node types
  - To be targeted first: Lammps, VASP, Sphinx, structure,  Murnaghan etc. The order will depend on user-demand.
  - Separate *server dataclass* may be needed in addition to the scientific one for a complete conceptual dictionary
  - Working concept exists (development [here](https://github.com/pyiron/pyiron_rdm/tree/pkruzikova_dev))
  - Development dependent on that of dataclasses, Uniton and nodes themselves
2. Set of functions available to map job/node conceptual dictionaries to the BAM(/SFB1394) OpenBIS data model and upload to OpenBIS
  - Working concept exists (development [here](https://github.com/pyiron/pyiron_rdm/tree/pkruzikova_dev))
  - Development dependent on that of 1. (conceptual dictionaries)
  - *To be decided:* Where do these functions live (if on the node-store thingy, then what should be the requirements?) and how are they structured (which information should appear in the main function and what can be imported?)
3. Upload of full workflow to OpenBIS
  - Option to upload the whole pyiron workflow to OpenBIS, from a code cell or the GUI, using 1. and 2.
  - First concept by March 2025
    
**Further possible milestones:**
- Set of functions available to export (data) to CKAN (metadata most likely provided by user directly)
- Set of functions available get information from the DB to upload to openBIS (if the database will allow for such)
- Interoperability with the NOMAD database (TBD)

[^1]: This conversation [here](https://github.com/pyiron/specs/pull/27#pullrequestreview-2350130002)
[^2]: [fair_assessment.md](fair_assessment.md)

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

In the above figure, the ontologies could be replaced by other ontologies, but the strategy remains the same. The various platforms can also be replaced, but the strategy still remains the same with just different degrees of coarse-graining and different types of metadata filtering from the conceptual dictionary. In other words, the conceptual dictionary is the main "mapping" between pyiron and the platforms. This could mean different types of platforms like openBIS, ckan etc. or even different instances of the same platform (e.g., BAM-openBIS, SFB1394-openBIS etc.).

For `pyiron_atomistics` jobs, the current method to extract the conceptual dictionary is done using the `to_dict()` method. It would be relatively straightforward to shift to the dataclasses. But `pyiron_workflow` is a whole different beast. The dataclass nodes would of course prove useful, but may lack contextual information on how they fit into the worklow graph. For this we may need a `wf.get_master_dataclass` or something that collects all the sub-dataclasses, inputs and outputs of nodes, and also maybe the functions themselves as strings or some such.

## Options for uploading data

Pyiron should be able to users with the following options to upload data to the platforms:
- Upload the data to the storage attached to the platform (probably some type of file system)
- Upload the data to a separate cloud storage (e.g., S3) and provide a link on the platform
- Upload only metadata which should at least be enough to reproduce calculations

## Challenges

* With the "open beta" release of `pyiron_workflow`, we would need to agree on a relatively concrete method to decide which nodes/graphs receive the conceptual dictionary treatment and a unique object type. One possible requirement could be that the node has to contain a proper docstring (which may also have useful literature references for the science behind the function/macro). Another requirement could be that the node is available on the *main* branch of the pyiron_nodes repo.
* Platforms like ckan have a limited number of datasets that they can handle, a few thousands at most. We would need features that can identify semnatically similar workflows and pack them together [^1]
* What is a material?? How do we let the publishing platform know in an automated manner that our simulations should be linked/tagged to nickel superalloy type 718, or steel grade 316. Such a feature would invaluable to connect our data with the experimentalists.

[^1]: This conversation [here](https://github.com/pyiron/specs/pull/27#pullrequestreview-2350130002)


# Data types

- Data types are used not only to indicate the numerical types, but also the ontological types.
- Users have a possibility to indicate the following information in the data types:
  - Numericla data type (`float`, `int` etc.)
  - Ontological type (optional) [^1]
  - Units (optional)
  - Shape (optional)
- Data types can be applied to functions and data classes
- If applied to the functions, users have the possibility to apply an interpreter, which checks the compatibility of the input data and the output data. [^1]
- Users can define ontological types based on existing an ontological database [^1]
- It may be sufficient that objects/variables have either a dataclass or ontology type (not both). If an object/variable has a dataclass, then the ontology typing can be done for the entries inside the dataclass. Ontology ids will mostly be reused multiple times in different dataclasses. Furthermore, many ontologies tend to be incomplete and a single dataclass may need to pull from different ontologies as needed. Hence, it may be beneficial to have ontology types defined as classes of existing ontologies in different files than the dataclass nodes. For example, we could have an asmo.py, cmso.py pmdco.py etc. which return the id of a specified entity from these ontologies when used in a dataclass node in pyiron_nodes. I mentioned these three ontologies specifically since there are plans to align them in the near future. [^2] 

## General points on using ontologies [^3]

- Level 1: As a controlled vocabulary for providing context: A possible solution would be simply to have an ontology id field that provides context (as Sam mentioned [^1]), in similar fashion to ELNs that support IRIs for their metadata fields. Benefits are limited to providing context to aid human understanding.
- Level 2: As a conceptual model: conceptual dictionary or metadata schemas should ideally be derived from controlled vocabularies (ontologies), as Tara mentioned [^2]. The benefits here are producing FAIR data and improving the interoperability of the data. See fair_assessment.md FAIR -F2, I1, I2.  A downside is the availability of data models. There is good coverage for atomistic simulations with ASMO, CMSO etc., but rather limited for other scales. The development of these data models would require significant efforts as they need community agreement, and should be separate from code development.
- Level 3: As a backbone for knowledge graph or database: This covers the instantiation of the data model as triples, as opposed to the key:value pairs in the previous point. For example, atomRDF for atomistic data which supports pyiron outputs and uses ASMO, CMSO, and PROV-O to create knowledge graphs. 
- Level 4: As part of larger system that allows reasoning, semantic decision making, with LLMs etc. Ideally, the benefit of the previous point would be this. On could think of constraining datatypes workflow inputs and outputs (for example Pressure and Stress would be different although they have same numerical datatypes, units, and shape), and providing workflow suggestions. To make full use of ontology typing in pyiron_workflow, one would need to develop a data model for pyiron_workflow, which also relies heavily on existing vocabularies.

[^1]: (Sam) I wrote these points to initiate a discussion, but in reality I don't really know how the actual implementation should look.
[^2]: (Tara) I wrote this point to continue the discussion. Useful references: [this](https://github.com/pyiron/uniton/issues/6#issue-2551361833) discussion, [asmo](https://github.com/OCDO/asmo/tree/8-asmo-term-additions-needed) ontology, [cmso](https://github.com/OCDO/cmso-ontology) ontology, [pmdco](https://github.com/materialdigital/core-ontology/tree/develop-3.0.0) ontology 
[^3]: (Sarath) I added different levels of interaction with ontologies in increasing complexity. I believe we should first establish which of the levels is beneficial for pyiron, which would help us in identifying the requirements.


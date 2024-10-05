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

[^1]: (Sam) I wrote these points to initiate a discussion, but in reality I don't really know how the actual implementation should look.
[^2]: (Tara) I wrote this point to continue the discussion. Useful references: [this](https://github.com/pyiron/uniton/issues/6#issue-2551361833) discussion, [asmo](https://github.com/OCDO/asmo/tree/8-asmo-term-additions-needed) ontology, [cmso](https://github.com/OCDO/cmso-ontology) ontology, [pmdco](https://github.com/materialdigital/core-ontology/tree/develop-3.0.0) ontology 

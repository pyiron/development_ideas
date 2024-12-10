# Development ideas for modules constituting next gen workflow manager

This repo is used for a brainstorm about the next generation workflow manager and their specs for pyiron. Feel free to take a look, but it is currently being actively written and therefore the content can change drastically at any moment.

In this repo, you find the main components of the next generation workflow manager and their specs. Feel free to open PR's to propose changes

We define specs to explain the structure of the pyiron project with a specific focus on how the individual components of pyiron interact with each other. pyiron in this case refers to both `pyiron_workflow` and `pyiron_base` as the two current interfaces we provide as part of the pyiron project. 

On an abstract level we see as specs as defining the interfaces between the building blocks which together form the pyiron project. Peter Naur wrote an article on [programming as theory building](https://pages.cs.wisc.edu/~remzi/Naur.pdf) which highlights that effective extension of an existing program requires more than just the access to the whole code, especially it requires an understanding of the scope of the program and the concepts it is based on.

## Overview 
* Abstract Definition
  * [Concepts](concepts.md)
  * [FAIR](fair_assessment.md)
* User Interface
  * [Function](function.md)
  * [Dataclasses](dataclasses.md)
  * [Datatypes](datatypes.md)
  * [Scientific Data](publication.md)
  * [Documentation](documentation.md)
  * [Backwards Compatibility](backwards.md)
* Backend
  * [Visual Programming](visualprogramming.md)  
  * [Storage](storage.md)
  * [Database](database.md)
  * [Executor](executor.md)
  * [Distribution](distribution.md)
  * [Architecture and Infrastructure](architecture_and_infrastructure.md)

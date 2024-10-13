# Specs for modules constituting next gen workflow manager

In this repo, you find the main components of the next generation workflow manager and their specs. Feel free to open PR's to propose changes

We define specs to explain the structure of the pyiron project with a specific focus on how the individual components of pyiron interact with each other. pyiron in this case refers to both `pyiron_workflow` and `pyiron_base` as the two current interfaces we provide as part of the pyiron project. 

On an abstract level we see as specs as defining the interfaces between the building blocks which together form the pyiron project. Peter Naur wrote an article on [programming as theory building](https://pages.cs.wisc.edu/~remzi/Naur.pdf) which highlights that effective extension of an existing program requires more than just the access to the whole code, especially it requires an understanding of the scope of the program and the concepts it is based on.

## What's spec?

Well written specs should:

- define terms used in the module
- specify technical requirements
- guarantee functionalities

Basically: "I promise to do ABC if you provide me XYZ"

## What do we need it for?

The single biggest advantage of specifying specs is it allows us to divide a huge chunk of code like pyiron into smaller parts. Taking an example of the current storage interface:

```mermaid
graph LR
  pyiron -- to_dict() --> storage
  storage -- from_dict() --> pyiron
```

Now if you want to make changes in `storage`, you don't need to know anything about pyiron - you just need to know that a `dict` must be saved and loaded, along with other conditions stated in the specs.

Therefore, it is crucial to state the requirements **explicitly**; it's not a place to merely state conceptual ideas in words.

## Examples

### Bad

"The storage interface takes the data from pyiron and stores it in the working directory"

NB: It doesn't mean that this phrase must not be written. It just needs more clarification.
### Good

"The storage interfaces stores the data via `save()`, which takes the arguments `data: dict` and `path: str`"

## Overview 
* Abstract Definition
  * [Concepts](concepts.md)
  * [FAIR](fair_assessment.md)
* User Interface
  * [Function](function.md)
  * [Dataclasses](dataclasses.md)
  * [Datatypes](datatypes.md)
  * [Scientific Data](publication.md)
* Backend
  * [Visual Programming](visualprogramming.md)  
  * [Storage](storage.md)
  * [Database](database.md)
  * [Executor](executor.md)
  * [Distribution](distribution.md)
  * [Architecture and Infrastructure](architecture_and_infrastructure.md)

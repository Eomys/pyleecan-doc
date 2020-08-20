##################
Code contribution
##################

Welcome to the PYLEECAN code contributing guidelines. On this page we will define the different steps you can follow to
contribute to the code:

- **First step: talk about it**
- **Second step: Contribute to the code**
- **Third step: Submit your contribution**


Talk about it
==============

As a first step to contribute to PYLEECAN code (to fix a bug, add a new feature, a new topology, etc.), we recommend to talk
about it to make sure that it doesn't already exist (for instance) or to be guided on how to do it. Thereare two ways to
talk about contributing to pyleecan:

- open an `issue <https://github.com/Eomys/pyleecan/issues>`__ on github
- send a message to the PYLEECAN mailing list pyleecan@framalistes.org to contact the maintainers.

Contribute to the code
=======================

Prerequisites
```````````````
Before contributing to the code, you may want to read about how we organize the classes and what is the PYLEECAN coding convention.

.. toctree::
    :maxdepth: 1

    object.organization
    coding.convention

How to add new topologies
``````````````````````````

The main steps to add a new topology (machine, slot, etc.) are:

- create a class that represents this topology by adapting an existing similar one
- develop the class methods
- validate that it works (and that it will always do) by adding tests

Create class
'''''''''''''

In PYLEECAN, classes are automatically generated. To know how to generate a class, we invite you to read the
documentation about it:

.. toctree::
    :maxdepth: 1

    class.generation

Develop class methods
''''''''''''''''''''''

The methods of the classes are developed in the folder **Methods/subfolder1/subfolder2**.

- **subfolder1** corresponds to the type of the topology (machine, lamination, slot, etc.)
- **subfolder2** is the name of the class.

These  methods  are  imported  and assigned to the correct class during the automated code generation according to the csv doc.
Note that you can also add a third layer of subfolder by naming a method **<folder_name>.<method_name>** in the csv file. For instance,
a method named "femm.draw" in the csv file of the Magnetics object would be stored in the folder Methods.Simulation.Magnetics.femm
(and the method is called with "draw")

Develop tests
''''''''''''''

To know more about tests development, please visit our :doc:`test contribution guideline </test.contribution>`.

Submit your contribution
=========================

Once the development step done, you will be able to share your work to the PYLEECAN community. To do that, Github provides the
**Pull requests**, which  is a method of submitting contributions to an open development project. You can visit this
`page <https://help.github.com/articles/creating-a-pull-request-from-a-fork/>`__ to know how to make pull requests from a
forked repository.

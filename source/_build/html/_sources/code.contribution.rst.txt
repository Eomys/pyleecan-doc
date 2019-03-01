##################
Code contribution
##################

Welcome to the PYLEECAN code contributing guidelines. On this page we will define the different steps you may follow to
contribute to the code:

- **First step: talk about it**
- **Second step: Contribute to the code**
- **Third step: Submit your contribution**


Talk about it
==============

As first step to contribute to PYLEECAN code (report a bug, new feature, new topology, etc.), we recommend our
community to open an `issue tracker <https://github.com/Eomys/pyleecan/issues>`__  or send message to the PYLEECAN mailing
list pyleecan@framalistes.org to  contact the maintainers. The main objective of communication process is giving to the
community as much information as necessary to let them understand what's going on and participate, without inundating them
with too much content to possibly digest.

Contribute to the code
=======================

Prerequisites
```````````````
Before contributing to the code, you may want to read how we organize the classes and what is the PYLEECAN coding convention.

.. toctree::
    :maxdepth: 1

    object.organization
    coding.convention

How to add new topologies
``````````````````````````

The main steps to add a new topology (machine, slot, etc.) are create a class, develop the class methods and their tests.

Create class
'''''''''''''

In PYLEECAN, classes are generated automatically, to know how to generate a class we invite you to read the
documentation about it:

.. toctree::
    :maxdepth: 1

    class.generation

Develop class methods
''''''''''''''''''''''

The methods of the class are developed in the folder **Methods/subfolder1/subfolder2**.

- **subfolder1** corresponds to the type of the topology( machine, lamination, slot, etc.)
- **subfolder2** is the name of the class.

These  methods  are  imported  and assigned to the correct class during the automated code generation.

Develop tests
''''''''''''''

To know more about tests development, please visit our test contribution guideline and see :ref:`test.contribution:How do we write unittest`

Submit your contribution
=========================

Once the development step done, you will be able to share your work to the PYLEECAN community. To do that, Github provides the
**Pull requests**, which  is a method of submitting contributions to an open development project. You can visit this
`page <https://help.github.com/articles/creating-a-pull-request-from-a-fork/>`__ to know how to make pull request from
forked repository.

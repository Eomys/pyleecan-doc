Code contribution
=================

Welcome to the PYLEECAN code contributing guidelines. On this page we
will define the different steps you can follow to contribute to the
code:

-   **First step: talk about it**
-   **Second step: Contribute to the code**
-   **Third step: Submit your contribution**

You can also watch the first part of this [webinar replay](webinar_3.md) for a step-by-step video tutorial.

Talk about it
-------------

As a first step to contribute to PYLEECAN code (to fix a bug, add a new
feature, a new topology, etc.), we recommend to talk about it to make
sure that it doesn't already exist (for instance) or to be guided on how
to do it. There are two ways to talk about contributing to pyleecan:

-   open an [issue](https://github.com/Eomys/pyleecan/issues) or a [discussion](https://github.com/Eomys/pyleecan/discussions) on github
-   send a message to the [PYLEECAN mailing list](mailto:pyleecan@framalistes.org) to contact the maintainers.

Contribute to the code
----------------------

### Prerequisites

Before contributing to the code, you may want to read about how we
organize the classes and what is the PYLEECAN coding convention.

* [Object Organization](object.organization.md)
* [Coding Convention](coding.convention.md)

### How to fork Pyleecan

In order to develop in PYLEECAN, you first need to fork and download the source code, following this [tutorial](fork.pyleecan.md).

### How to create your own branch

Before starting to develop, we recommend that you create your own branch
in order to cleanly track your modifications. To do so, please follow
this [tutorial](creating.branch.md).

### How to implement your modifications

The main steps to implement a new feature are:

-   create a class that represents a new object (e.g. machine, slot,
    model, etc., optional)
-   develop class methods and functions
-   validate that everything works (and that it will always do) by
    adding tests

This [tutorial](tuto.add.slot.md) on how to add a new slot gives an example
of code contribution. We recommend to read it before starting a
contribution.

This [tutorial](test.contribution.md) provides guidelines to add tests.

Submit your contribution
------------------------

Once the development steps done, you will be able to share your work to
the PYLEECAN community. To do that, please follow this
[tutorial](integrate.contribution.md).

####################
Project organization
####################

This page introduces the PYLEECAN file organization: you will find out how the modules are sorted and what are the folders
in the project. We will also talk about PYLEECAN repositories.

File organization
-----------------

The files of PYLEECAN 1.0 are sorted in several folders according to the following rules:

- **Classes** folder contains all the classes built by the :doc:`code generator </class.generation>`.

- **Methods** folder contains all the methods of the classes sorted in subfolder by types:

    + Machine
    + Slot
    + Simulation
    + Material
    + etc .

    These  methods  are  imported  and assigned to the correct class during the automated code generation. The method of a
    class can also be sorted in separated folders.

- **GUI** folder contains the code for the Graphical User Interface sorted by widget.

- **Function** folder  contains  general  functions  that  can  be  used  by  several methods (for instance FFT computation, interaction with other software...).

- **Tests** folder contains all the unittest and the validation cases. The subfolders follow the organization of the tested folders (Classes, Methods, GUI...).


PYLEECAN repositories
----------------------

PYLEECAN has currently two repositories:

- `pyleecan <https://github.com/Eomys/pyleecan>`__ for the python code
- `pyleecan-doc <https://github.com/Eomys/pyleecan-doc>`__ for PYLEECAN documentation (this website).

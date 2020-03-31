PYLEECAN
========

Presentation
------------

PYLEECAN™ objective is to provide a **user-friendly, unified, flexible simulation framework for the multiphysic design and optimization of electrical machines and drives** based on fully open-source software.

.. image:: _static/BPMSM.png
.. image:: _static/IPMSM.png
.. image:: _static/SyRM.png

It is meant to be used by researchers, R&D engineers and teachers in electrical engineering, both on standard topologies of electrical machines and on novel topologies (e.g. during a PhD work). 
An objective of PYLEECAN is that **every PhD student should start with PYLEECAN instead of implementing his own scripts (e.g. coupling Scilab or Matlab with Femm)**.

Scope
-----

The initial scope of the project is to simulate the electromagnetic performances of the following 2D radial flux machines:

- Interior, Surface and Surface Inset Permanent Magnet Synchronous Machines (IPMSM, SPMSM, SIPMSM) with inner or outer rotor
- Squirrel Cage Induction Machines (SCIM) and Doubly Fed Induction Machines (DFIM)
- Synchro Reluctant Machines (SyRM)
- Switched Reluctance Machines (SRM).

The project should then address 3D topologies (axial flux machines, claw-pole synchronous machines) and linear machines.
On a longer term, PYLEECAN should also include the following five physics with different model granularity (e.g. analytic, semi-analytic, finite element):

- Electrical
- Electromagnetics
- Heat Transfer
- Structural Mechanics
- Acoustics

Status of the project (28th March 2020)
---------------------------------------

EOMYS initiated in 2018 the open-source project named PYLEECAN (Python Library for Electrical Engineering Computational Analysis) under Apache license by releasing a part of `MANATEE <https://eomys.com/produits/manatee/article/logiciel-manatee?lang=en>`__ commercial software scripts. These initial scripts included a fully **object-oriented modelling** of main radial flux electrical machines, with parameterized geometry. However, PYLEECAN is not an EOMYS-only project, the initial maintainers includes other companies and universities and all contributors are welcome.

PYLEECAN is fully coupled to `FEMM <https://www.femm.info>`__ to carry **non-linear magnetostatic** analysis including sliding band and symmetries.
PYLEECAN includes a **Graphical User Interface** to define main 2D radial flux topologies parametrized geometries (PMSM, IM, SRM, SyRM) including material library.
PYLEECAN is coupled to `Gmsh <http://gmsh.info/>`__ **2D/3D finite element mesh generator** to run third-party multiphysic solvers. 
PYLEECAN is coupled to a **multiobjective optimization** library to carry design optimization of electrical machines.

Getting started
----------------

.. toctree::
    :maxdepth: 1

    prerequisite
    get.pyleecan
    project.organization
    tutorials
    development

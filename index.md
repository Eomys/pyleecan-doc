PYLEECAN
========

Presentation
------------

PYLEECAN™ objective is to provide a **user-friendly, unified, flexible
simulation framework for the multiphysic design and optimization of
electrical machines and drives** based on fully open-source software.

![](_static/BPMSM.png)
![](_static/IPMSM.png)
![](_static/SyRM.png)

It is meant to be used by researchers, R&D engineers and teachers in
electrical engineering, both on standard topologies of electrical
machines and on novel topologies (e.g. during a PhD work). An objective
of PYLEECAN is that **every PhD student should start with PYLEECAN
instead of implementing his own scripts (e.g. coupling Scilab or Matlab
with Femm)**.

Scope
-----

The initial scope of the project is to simulate the electromagnetic
performances of the following 2D radial flux machines:

-   Interior, Surface and Surface Inset Permanent Magnet Synchronous
    Machines (IPMSM, SPMSM, SIPMSM) with inner or outer rotor
-   Squirrel Cage Induction Machines (SCIM) and Doubly Fed Induction
    Machines (DFIM)
-   Synchro Reluctant Machines (SyRM)
-   Switched Reluctance Machines (SRM).

The project should then address 3D topologies (axial flux machines,
claw-pole synchronous machines) and linear machines. On a longer term,
PYLEECAN should also include the following five physics with different
model granularity (e.g. analytic, semi-analytic, finite element):

-   Electrical
-   Electromagnetics
-   Heat Transfer
-   Structural Mechanics
-   Acoustics

Status of the project (30th October 2020)
-----------------------------------------

EOMYS initiated in 2018 the open-source project named PYLEECAN (Python
Library for Electrical Engineering Computational Analysis) under Apache
license by releasing a part of [MANATEE](https://eomys.com/produits/manatee/article/logiciel-manatee?lang=en)
commercial software scripts. These initial scripts included a fully
**object-oriented modelling** of main radial flux electrical machines,
with parameterized geometry. However, PYLEECAN is not an EOMYS-only
project, the initial maintainers includes other companies and
universities and all contributors are welcome.

Main Features:

-   PYLEECAN is fully coupled to [FEMM](http://www.femm.info) to carry
    **non-linear magnetostatic** analysis including sliding band and
    symmetries. For now this coupling is available only on Windows.
-   PYLEECAN includes an electrical model to solve the equivalent
    circuit of PMSM machine by using the FEMM coupling
-   PYLEECAN includes a **Graphical User Interface** to define main 2D
    radial flux topologies parametrized geometries (PMSM, IM, SRM, SyRM)
    including material library.
-   PYLEECAN is coupled to [Gmsh](http://gmsh.info/) **2D/3D finite
    element mesh generator** to run third-party multiphysic solvers.
-   PYLEECAN is coupled to a **multiobjective optimization** library to
    carry design optimization of electrical machines.


Getting started
---------------

- [Prerequisites](prerequisite.md)
- [Install PYLEECAN](get.pyleecan.md)
- [Project Organization](project.organization.md)
- [Tutorials](tutorials.md)
- [Developement Guidelines](development.md)

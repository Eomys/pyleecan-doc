PYLEECAN
========

Presentation
------------

PYLEECAN’s objective is to provide a **user-friendly, unified, flexible simulation framework for the multiphysic design and optimization of electrical machines and drives** based on open-source software.

.. image:: _static/BPMSM.png
.. image:: _static/IPMSM.png
.. image:: _static/SyRM.png

It is meant to be used by researchers, R&D engineers and teachers in electrical engineering, both on standard topologies of electrical machines and on novel topologies (e.g. during a PhD work). An objective of PYLEECAN is that every PhD student should start with PYLEECAN instead of implementing (again) his own routines (e.g. coupling Scilab and Femm).

Scope
-----

The scope of the project is to simulate the following 2D radial flux
machines:

- Surface, Surface Inset or Interior Permanent Magnet Synchronous Machines (SPMSM, SIPMSM, IPMSM) with inner or outer rotor
- Squirrel Cage Induction Machines (SCIM) and Doubly Fed Induction Machines (DFIM)

Then the project should address the following topologies:

- Direct Current Machines (DCM).
- Synchro Reluctant Machines (SyRM).
- Switched Reluctance Machines (SRM).

In the long-term basis PYLEECAN will include 5 physics and several model
for each (analytical, numerical...):

- Electrical
- Electromagnetics
- Heat Transfer
- Structural Mechanics
- Acoustics

Status of the project
----------------------

EOMYS has started an open and non-commercial project named PYLEECAN (Python Library for Electrical Engineering Computational Analysis). The starting code of PYLEECAN is based on
`MANATEE <https://eomys.com/produits/manatee/article/logiciel-manatee?lang=en>`__ (EOMYS commercial software) but PYLEECAN is not an EOMYS only project: The initial maintainers includes other company and university. We are welcoming everyone that want to contribute.

All the architecture of the project is ready and we are progressively releasing the starting modules. If you are interested by a topology or a specific model, you can open an issue on this Github repository to talk
about it. We will gladly explain how to add it yourself or we will add it to the development list for further release.

Getting started
----------------

.. toctree::
    :maxdepth: 1

    prerequisite
    get.pyleecan
    project.organization
    tutorials
    development

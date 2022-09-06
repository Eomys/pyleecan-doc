Python
======

Prerequisites
-------------

PYLEECAN is developed and tested (for now) in Python 3.8.X. The executable is also generated with this version.
Other Python versions should be compatible but are not tested. If you must use another version of Python or of any package, please open an issue so we can investigate how to extend the compatibility.

Installation
------------

To install the latest version of Python, download the
[installer](https://www.python.org/downloads/) and run it.

PYLEECAN will use others python scientific packages like NumPy,
MatplotLib, etc. To install those packages we recommend you to use
**pip**, a python package manager which is already installed if your
python version is \>= 3.4.

You can find listed below all the packages used in PYLEECAN:

-   ddt
-   pytest
-   definitions
-   cloudpickle
-   numpy
-   scipy
-   matplotlib
-   pyfemm
-   mock
-   gmsh-sdk
-   pandas
-   PySide2
-   xlrd
-   deap
-   SciDataTool
-   pyvista
-   meshio
-   h5py

To install all these packages, copy them in a text file
(requirements.txt) and run :: python -m pip install -r requirements.txt

##############
Prerequisites
##############

Python
-------
PYLEECAN is developed (for now) in Python 3.5. Python 3 has been chosen to be up to date with the latest
development of Python language and its main packages. Python 3.5 or further versions, are used to be able to use the
package PyInstaller to generate an .exe file of the program. A compatibility with Python 2.7 can be considered if needed by
the community. In this case, we would need to use unittest to make sure that PYLEECAN runs on both versions.

Installation
`````````````
To install the latest version of Python, download the `installer <https://www.python.org/downloads/>`__ and run it.


PYLEECAN will use others python scientific packages like NumPy, MatplotLib, etc. To install those packages we recommend you
to use **pip** a python package manager which is already installed if your python version is >= 3.4.

You can find listed below all the packages used in PYLEECAN:

- ddt
- pytest
- definitions
- cloudpickle
- numpy
- scipy
- matplotlib
- pyfemm
- mock
- gmsh-sdk
- pandas

To install all these package, copy them in a text file (requirements.txt) and run 
::
        python -m pip install -r requirements.txt

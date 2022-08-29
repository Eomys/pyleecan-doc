Install PYLEECAN
================

Welcome to the PYLEECAN installation page. There are three ways to
install PYLEECAN:
- 1) Use the GUI installer (fastest and simpliest but limited)
- 2) Get the code from pypi (for scripting in python)
- 3) Get the code from Github (enables contributing to pyleecan)

Installing third party software (all three methods)
---------------------------------------------------

The principle of Pyleecan is to enable the user to choose between
several models to compute the same quantities. Some of them call other
software that you need to install as well (if you plan to use them). For
now, the following software have a coupling with pyleecan:

-   [FEMM](http://www.femm.info/wiki/Download) (Windows only for now)
-   [GMSH](http://gmsh.info/)
-   [Elmer](

GUI installer
-------------
For the first method, you will find here the installer of the different pyleecan versions:
- v1.4.0 released 29/08/22

This installer is available only for windows. It enables to define a machine and to run single speed magnetic current driven FEMM simulations. At the end of the simulations all the meaningful results are gathered in the result folder:
[add images]

This installer is the fastest way to define and run this kind of simulation but several important features (like variable speed simulations, parameter sweep or optimization) are not available.

Getting python
----------------------
For method two and three, you will need to install Python and preferably an **IDE** (Integrated Development Environment).

To be able to use PYLEECAN you will need to download
[Python](https://www.python.org/downloads/). We recommend to use python version 3.8.X
Support with older Python version haven't been tested and can be possible. If you experience difficulties with running
Pyleecan with any version of Python, please [open an issue on
Github](https://github.com/Eomys/pyleecan/issues) to talk about it.

The next needed tool is an **IDE** (Integrated Development Environment) to write and execute python code/scripts.
There are too many IDE to count and each developer has its own
preference. We recommend to install [Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial), this
is Microsoft’s IDE (accessible on all platform) that provides a [plugin
dedicated to Python](https://code.visualstudio.com/docs/python/python-tutorial) that you need to install as well. 

If you don't want to use VScode, You can take a look at [Spyder](https://docs.spyder-ide.org/index.html), [Anaconda](https://www.anaconda.com/distribution/), [PyCharm](https://www.jetbrains.com/fr-fr/pycharm/) or any results from “best IDE for
python” on your favorite web browser. 
We used to recommend Anaconda/Spyder since it's meant to be used in scientific environment, but we had several issues with pyleecan GUI (that may be corrected now).



Step 2: Getting PYLEECAN
------------------------

PYLEECAN is available on [pip](https://pypi.org/project/pip/). Simply
type in a shell the following command: :

    pip install pyleecan

This command line should download and install pyleecan on your computer,
along with all the needed dependencies (see [Prerequisites](prerequisites.md).

Step 4: Launch tests
--------------------

You can finally launch some tests to check that everything is working
correctly: :

    python -m pytest -m "not long"

More details on this command are available in the [tests contribution page](test.contribution.md).

Step 5: Launch the GUI
----------------------

Before launching the Graphical User Interface and if you are using Anaconda on Windows,
you will need to add a new variable to the system environment variables:
- variable name: `QT_QPA_PLATFORM_PLUGIN_PATH`
- value: **`path\to\anaconda3`**`\Lib\site-packages\PySide2\plugins\platforms`

You can have a look at this [forum](https://superuser.com/questions/949560/how-do-i-set-system-environment-variables-in-windows-10) 
if you don't know how to add an environment variable.

You should now be able to launch the GUI with the command:

`run -m pyleecan`

Note that this command launched from the IPython console of Spyder will probably crash. We advise
to launch it from a separate console.

Install PYLEECAN
================

Welcome to the PYLEECAN installation page. There are several ways to
install PYLEECAN, whether you want to simply use it, or to contribute.
In both cases, you will need to install Python and preferably an **IDE**
(Integrated Development Environment). To **simply use it**, please follow the 
following guidelines. If you want to **contribute** to PYLEECAN, you will 
have to download the source code and create a fork on your GitHub account: 
see this [tutorial](fork.pyleecan.md).

Step 1: Getting python
----------------------

To be able to use PYLEECAN you will need to download
[Python](https://www.python.org/downloads/). We recommend a python version
between 3.6 (to be able to use [black](https://pypi.org/project/black/) but
this is not mandatory) and 3.8 for the GUI (if you want to use it). Support with older Python version haven't been
checked and can be possible. If you experience difficulties with running
Pyleecan with any version of Python, please [open an issue on
Github](https://github.com/Eomys/pyleecan/issues) to talk about it.

The next needed tool is an **IDE** (Integrated Development Environment).
There are too many IDE to count and each developer has its own
preference (and we know that the following list will never be satisfying
enough for everyone). Here we will only detail few of the one that the
community use (without quality ordering):

-   [Spyder](https://docs.spyder-ide.org/index.html): this one is
    directly included in the
    [Anaconda](https://www.anaconda.com/distribution/) distribution
    which provides python with several useful scientific packages
-   [PyCharm](https://www.jetbrains.com/fr-fr/pycharm/): This IDE was
    specially designed for python. The free version is great, but you
    can have a look to the commercial one if you feel that you need it.

- [Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial): This
is Microsoft’s IDE (accessible on all platform) that provides a plugin
dedicated to Python. Last method to find an IDE: type “best IDE for
python” on your favorite web browser and you will be able to find the
latest debate on this topic.

Step 2: Getting PYLEECAN
------------------------

PYLEECAN is available on [pip](https://pypi.org/project/pip/). Simply
type in a shell the following command: :

    pip install pyleecan

This command line should download and install pyleecan on your computer,
along with all the needed dependencies (see [Prerequisites](prerequisites.md).

Step 3: Installing third party software
---------------------------------------

The principle of Pyleecan is to enable the user to choose between
several models to compute the same quantities. Some of them call other
software that you need to install as well (if you plan to use them). For
now, the following software have a coupling with pyleecan:

-   [FEMM](http://www.femm.info/wiki/Download) (Windows only for now)
-   [GMSH](http://gmsh.info/)

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

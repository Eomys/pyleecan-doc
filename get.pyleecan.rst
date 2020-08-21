#################
Install PYLEECAN
#################

Welcome to the PYLEECAN installation page. There are several ways to install PYLEECAN, whether you want to simply use it, or to contribute. In both cases, you will need to install Python and preferably an **IDE** (Integrated Development Environment).

Step 1: Getting python
======================

To be able to use PYLEECAN you will need to download `Python <https://www.python.org/downloads/>`__. We recommend a version >3.6 to be able to use `black <https://pypi.org/project/black/>`__ but this is not mandatory. Support with older Python version haven't been checked and can be possible. If you experience difficulties with running Pyleecan with any version of Python, please `open an issue on Github <https://github.com/Eomys/pyleecan/issues>`__ to talk about it.

The next needed tool is an **IDE** (Integrated Development Environment). There are too many IDE to count and each developer has its own preference (and we know that the following list will never be satisfying enough for everyone). Here we will only detail few of the one that the community use (without quality ordering):  

-	`Spyder <https://docs.spyder-ide.org/index.html>`__: this one is directly included in the `Anaconda <https://www.anaconda.com/distribution/>`__ distribution which provides python with several useful scientific packages
-	`PyCharm <https://www.jetbrains.com/fr-fr/pycharm/>`__: This IDE was specially designed for python. The free version is great, but you can have a look to the commercial one if you feel that you need it.   
-	`Visual Studio Code <https://code.visualstudio.com/docs/python/python-tutorial>`__: This is Microsoft’s IDE (accessible on all platform) that provides a plugin dedicated to Python.
Last method to find an IDE: type “best IDE for python” on your favorite web browser and you will be able to find the latest debate on this topic.

Step 2: Getting PYLEECAN
========================

There are two ways of getting the code: using **pip install to simply use it**, or downloading it **from GitHub to develop in it and contribute**.

The "I want to use it" way (from pip install)
---------------------------------------------

PYLEECAN is available on `pip <https://pypi.org/project/pip/>`__. Simply type in a shell the following command:
::

        pip install pyleecan
        
This command line should download and install pyleecan on your computer, along with all the needed dependencies (see :doc:`Prerequisites <prerequisite>`). You can then jump to Step 3.



The "I want to develop" way (from GitHub)
-----------------------------------------

**Step 2.1: Download PYLEECAN**

The first step is to get the code from `Github <https://github.com/Eomys/pyleecan/>`__:

.. image:: _static/github_get_code.PNG

There are three ways of getting the code:

- The "quick and dirty" way: 

Although we don't recommend it, you can download an archive of the code by clicking on the green button "Clone or download" then **"Download Zip"** (orange square on the image). Nothing else to install, you can directly go to Step 2.2.

- The "I want to develop on my side" way: 

For this method you will first need to install `git <https://git-scm.com/>`__ or `github <https://desktop.github.com/>`__. For Windows users, you may also want to install `Tortoisegit <https://tortoisegit.org/download/>`__ (or any equivalent) for a more convenient use of git.
Once git is installed you can **"clone"** the repository (either though command line or the tool you have installed with git) with the following command (green square on the image):

::

        git clone https://github.com/Eomys/pyleecan

This method will enable you to get the upcoming modifications of the Pyleecan's code in a more convenient way. 

- The "I want to contribute" way:

You will need a Github account to `fork <https://help.github.com/en/articles/fork-a-repo>`__ (bleu square on the image) the `pyleecan repository <https://github.com/Eomys/pyleecan>`__ which will create a copy of the repository in your Github projects. The forked repository will allow you to freely experiment with changes without affecting the original project. Later you will be able to send modifications from your fork to the main project on Github with :ref:`code.contribution:"pull-request"` (see also this :doc:`tutorial<integrate.contribution>`).
You can now **clone** the forked version of Pyleecan in your Github projects but following the "I want to use it" way above. 
You can also click on "Watch" (red square on the image) to choose how you want to be notified of the activities of the community. 
As a reminder, before contributing, please read the :ref:`project.organization:Project organization` and the :ref:`development:Development guidelines`.

**Step 2.2: Install dependencies**

Now that you downloaded PYLEECAN, you need to install the dependencies. All of them are gathered in the file `requirements.txt <https://github.com/Eomys/pyleecan/blob/master/requirements.txt>`__. You can install them with one single command with `pip <https://pypi.org/project/pip/>`__ (which should have been installed with python):
::

        pip install -r requirements.txt

If you want to contribute, we recommend to set a pre-commit hook to automatically apply `black <https://pypi.org/project/black/>`__ before every commit (if you are using python >=3.6).
First install the package pre-commit with the command:
::

        pip install pre-commit
Then in the top folder of pyleecan run the command:
::

        pre-commit install
The file "pyleecan/.pre-commit-config.yaml" is used for the configuration. You can edit it to set a particular python version if your default one is not >=3.6 (for instance).

Step 3: Installing third party software
=======================================

The principle of Pyleecan is to enable the user to choose between several models to compute the same quantities. Some of them call other software that you need to install as well (if you plan to use them). For now, the following software have a coupling with pyleecan:

* `FEMM <http://www.femm.info/wiki/Download>`__ (Windows only for now)
* `GMSH <http://gmsh.info/>`__

Step 4: Launch tests
====================

You can finally launch some tests to check that everything is working correctly:
::

        python -m pytest -m "not long"

More details on this command are available in the :doc:`tests contribution page </test.contribution>`.

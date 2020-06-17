#################
Install PYLEECAN
#################

Step 1: Getting the code
------------------------

Welcome to the PYLEECAN installation page. As PYLEECAN is not (yet) available on `pip <https://pypi.org/project/pip/>`__, to use PYLEECAN, the first step is to get the code from `Github <https://github.com/Eomys/pyleecan/>`__:

.. image:: _static/github_get_code.PNG

There are three ways of getting the code:

**The "quick and dirty" way**: 

Although we don't recommend it, you can download an archive of the code by clicking on the green button "Clone or download" then **"Download Zip"** (orange square on the image). Nothing else to install, you can directly go to step 2.

**The "I want to use it" way**: 

For this method you will first need to install `git <https://git-scm.com/>`__ or `github <https://desktop.github.com/>`__. For Windows users, you may also want to install `Tortoisegit <https://tortoisegit.org/download/>`__ (or any equivalent) for a more convenient use of git.
Once git is installed you can **"clone"** the repository (either though command line or the tool you have installed with git) with the following command (green square on the image):

::

        git clone https://github.com/Eomys/pyleecan

This method will enable you to get the upcoming modifications of the Pyleecan's code in a more convenient way. 

**The "I want to contribute" way**:

You will need a Github account to `fork <https://help.github.com/en/articles/fork-a-repo>`__ (bleu square on the image) the `pyleecan repository <https://github.com/Eomys/pyleecan>`__ which will create a copy of the repository in your Github projects. The forked repository will allow you to freely experiment with changes without affecting the original project. Later you will be able to send modifications from your fork to the main project on Github with :ref:`code.contribution:"pull-request"`.
You can now **clone** the forked version of Pyleecan in your Github projects but following the "I want to use it" way above. 
You can also click on "Watch" (red square on the image) to choose how you want to be notified of the activities of the community. 
As a reminder, before contributing, please read the :ref:`project.organization:Project organization` and the :ref:`development:Development guidelines`.


Step 2: Getting python and the dependencies
-------------------------------------------
Now to be able to use the code you will need to download `Python <https://www.python.org/downloads/>`__. We recommend a version >3.6 to be able to use `black <https://pypi.org/project/black/>`__ but this is not mandatory. Support with older Python version haven't been checked and can be possible. If you experience difficulties with running Pyleecan with any version of Python, please `open an issue on Github <https://github.com/Eomys/pyleecan/issues>`__ to talk about it.

Now that python is installed, you can download all the packages that are required to use pyleecan. All of them are gathered in the file `requirements.txt <https://github.com/Eomys/pyleecan/blob/master/requirements.txt>`__. You can install them with one single command with `pip <https://pypi.org/project/pip/>`__ (which should have been installed with python):
::

        pip install -r requirements.txt

The principal of Pyleecan is to enable the user to choose between several model to compute the same quantities. Some of them call other software that you need to install as well (if you plan to use them). For now, the following software have a coupling with pyleecan:

* `FEMM <http://www.femm.info/wiki/Download>`__
* `GMSH <http://gmsh.info/>`__

If you want to contribute, we recommend to set a pre-commit hook to automatically apply `black <https://pypi.org/project/black/>`__ before every commit (if you are using python >=3.6).
First install the package pre-commit with the command:
::

        pip install pre-commit
Then in the top folder of pyleecan run the command:
::

        pre-commit install
The file "pyleecan/.pre-commit-config.yaml" is used for the configuration. You can edit it to set a particular python version if your default one is not >=3.6 (for instance).
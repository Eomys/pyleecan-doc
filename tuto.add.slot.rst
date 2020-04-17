##################################
How to add a new slot in Pyleecan?
##################################
Introduction
============
Thanks to the OOP architecture of Pyleecan, adding a new topology (slot, ventilation, holes…)in Pyleecan is rather simple. We will take the opportunity of this simple example to explore in depth all the know-how and hint required to do a great contribution for all the Pyleecan community. We will present every single step from installing python to doing your first “pull-request” on Github. To do so, we will guide you through several other pages of this website in the correct order and with the proper introduction.

Step 1: Installing Pyleecan
===========================
To contribute to Pyleecan the first required tool is… Pyleecan. In particular, please follow :ref:`code.contribution: the procedure` to “fork” Pyleecan with git (the “I want to contribute way”), it will be required for the next part of this tutorial. 

Step 2: Getting an IDE
======================
Now that you have the code and that you are able to run it, you need a tool to properly edit it. For that you will need to download an IDE (Integrated Development Environment). There are too many IDE to count and each developer has its own preference (and we know that the following list will never be satisfying enough for everyone). Here we will only detail few of the one that the community use (without quality ordering):
-	`Spyder <https://docs.spyder-ide.org/index.html>`__: this one is directly included in the `Anaconda <https://www.anaconda.com/distribution/>`__ distribution which provide python with several useful scientific packages
-	`PyCharm <https://www.jetbrains.com/fr-fr/pycharm/>`__: This IDE was specially designed for python. The free version is great but you can have a look to the commercial one if you feel that you need it. 
-	`Visual Studio Code <https://code.visualstudio.com/docs/python/python-tutorial>`__: This is Microsoft’s IDE (accessible on all platform) that provide a plugin dedicated to Python. 
Last method to find an IDE: type “best IDE for python” on your favorite web browser and you will be able to find the latest debate on this topic. 

Step 3: Defining the Slot
=========================
You have now all the tools you need to start contributing to pyleecan. The next step is: What are you going to contribute to? For that you can begin by going to the `Github issue page<https://github.com/Eomys/pyleecan/issues>`__. This page gathers all the open topic from bug to correct, to new feature to add or anything to improve… Basically it is the place where the pyleecan community (users and contributors) gather to discuss about what to develop next.
For your new slot, first take a look at the issues to see if you can help the community by developing a slot that was requested in an issue. If you don’t find any (or if you just want to do your own) you can open an issue to describe the slot you are about to add. By opening this issue, the community will be able to help you during your development and maybe tell you that the slot is already available somewhere. We can also help you to improve the slot definition so that other people will be able to use it.
For these discussions (and for your development) you will need a schematic: an image that shows the shape of your slot and what each parameter corresponds to. To draw your schematics, we recommend to use `Geogebra <https://www.geogebra.org/>`__ (or any equivalent tool), it will help you to be precise in the definition of each parameter which is very important for other to understand and use your slot.
In this tutorial we will define a simple circular slot (to be used for notches) named “SlotCirc” and we will use the following schematics:

.. image:: _static/SlotCirc.png

Step 4: Creating a branch with git
==================================
Now that you are ready to start coding, let’s take some time to introduce git: the tool that will enable you to share your work with the community. There are lots of tutorial and video on the web that can get you started, the more you will learn the most efficient you will be if things get complicated. You can start with `this tutorial from Github <https://try.github.io/>`__. Here we will focus only on the very basis that you need to know to contribute to Pyleecan as quickly as possible. We recommend to take some time to explore some more detailed tutorials to better understand this powerful tool. In general, if you are not familiar with git, we strongly recommend to use it for any valuable piece of code that you produce. It will significantly improve your work (and opening a repository on Github is free if you want to share your work).

In this tutorial we will use TortoiseGit on Windows but there are lots of other way of using git. 
What is Git?
------------
Git is a tool that will track every single modification of your project. It will keep in memory the history of all these modifications so that you can easily see:
-	Who has done a modification?
-	What is the history of a single file?
As all the project is seen as a list of modifications (a file is edited, deleted, created…) you can easily do the following:
-	Get modifications from other users and merge it with your
-	Cancel some modifications to come back to an older version of the code. 

What are branches?
------------------
To contribute to pyleecan, the first thing you need to do is to create a "branch" on your repository. In the following example we create a branch named “new_slot” by right clicking on the pyleecan folder then selecting “TortoiseGit” and “Create Branch”:

.. image:: _static/tuto_slot_branch_1.PNG

A branch is a parallel version of the code that will cleanly isolate your modifications from the “official version”. It indicates from where “your version” of the code started and what you have done step by step (or version by version) to get to your current state.

You then need to select your branch to start working on it. For that you need to right click on the pyleecan folder then selecting “TortoiseGit” and “Swith/Checkout” and select the “new_slot” branch:

.. image:: _static/tuto_slot_branch_2.PNG

Now all further modifications will be added to your branch.

Step 5: Creating your class
===========================
Now that you have all the tools, let’s start creating your slot. The first thing you will need to do is to create a new Slot class. In Pyleecan all the class files are created with a tool known as the class generator. A dedicated documentation on this tool is available :doc:` on the website </class.generation>`
In pyleecan\Generator\ClassesRef you will find one csv file for each class of Pyleecan (organized by type/folder). To create your own, you can copy/paste an existing csv file (for instance SlotCirc), rename it and edit it. The name of your file will be the name of your class and it must be unique within pyleecan. In our example we copy the csv file pyleecan\Generator\ClassesRef\Slot\SlotW10.csv rename it to "SlotCirc.csv" and here is the content that we have edited:

.. image:: _static/tuto_slot_csv_1.PNG

The “left part” corresponds to the properties of the class. We have defined two lines or two properties: one for H0 and one for W0. Both are defined to be “float” value greater than 0 and a proper documentation text has been set.

.. image:: _static/tuto_slot_csv_2.PNG

The “right part” corresponds to global information on your class. We have updated the method list (cf next chapter of this tutorial) and changed the class description.  
Now that your csv file is properly edited, you need to run the code generator to create the corresponding python code. For that you need to run the script:
::

        python pyleecan/Generator/run_generate_classes.py

The class code is now available in the pyleecan/Classes folder. You can have a look at the resulting code to see what method and feature are automatically available but you shouldn’t edit this file as it is erased every time the code generator is called. To change a class in pyleecan, one must change its csv file and run the code generator.

Step 6: Defining your methods
=============================
Choosing which method to implement
----------------------------------
In this part, we will finally write the first lines of code! And we have a great news: you don’t have to write most of them! With Object Oriented Programming to add a new slot (with winding), you need to define the following list of method:
-	build_geometry: define the edge of the slot
-	build_geometry_wind: define the surface for winding
-	check: Check the slot constraints
-	comp_angle_opening: Compute the opening angle of the slot
-	comp_height: Compute the height of the slot
-	comp_height_wind: Compute the height of the winding part
-	comp_surface: Compute the surface of the Slot
-	comp_surface_wind: Compute the surface of the winding part

The convention linked to these methods are defined in the :doc:` following article</slot.convention>`

In this list, build_geometry and build_geometry_wind are the only two mandatory methods to define. All the other can be computed numerically according to the result of these two methods. The numerical computation code is available as method of the “Slot” or “SlotWind” classes if you are interested. You can still define the other methods (comp_surface, comp_height,...) to provide a faster analytical way of computing these values. We recommend to define all the methods, but the fastest way to add a new slot is just to define build_geometry and build_geometry_wind. Once you know which method you want to define, you can update the “Methods” column in the csv file if needed (and run the code generator).

Creating the Method folder
--------------------------
In pyleecan, all the methods are stored in a dedicated folder that follow this generic path pyleecan/Methods/<package>/<class_name>/<method_name>.py. In our example we need to create the folder pyleecan/Methods/Slot/SlotCirc. 
You will also need to add an empty file named “__init__.py” so that your methods can be imported in other part of pyleecan. You can also copy/paste/edit the full folder of SlotCirc. 
You need to create a file for each of the method that you listed in the csv file (build_geometry.py, comp_surface.py…). Note that if a method is present in the Methods folder but not referred in the csv file, **it won’t be available in the class**. The csv file is the exhaustive description of the class.

Defining the build_geometry method
---------------------
For the build_geometry method, you will need to compute the complex coordinates of each point on the edges of your slot. Note that for some slot we encapsulate the computation of the coordinates in a method named "_comp_point_coordinate". Once you have all the coordinates you need to create a list of “Line” object that describe your slot centered on the 0x axis in trigonometrical way and with both ends on the bore radius (abs(Z)=Rbo). Line is an abstract class so you need to use the following daughter objects:
-	Segment: A straight line between two points. 
-	Arc1: An arc of circle defined by two points and a radius.
-	Arc2: An arc of circle defined by a starting point, a center and an angle.
-	Arc3: Half a circle defined by 2 points and a direction. 

You can take inspiration from the code of other slot to see how to define your own list. 

Defining the build_geometry_wind method
--------------------------
For build_geometry_wind, you will need to define the “Winding area”. You will need to create several "SurfLine" object according to the number of requested surface. Each surface must be labeled:
Wind_<S or R>_R<Radial id>_T<Tangential id>_S0
With <S or R> for Stator or Rotor and the radial and tangential id are defined as follow (left image):

.. image:: _static/winding_convention_1.PNG

To create a SurfLine object, you will need to define the list of lines on the edges of this surface and to provide a “point_ref”: A point in the surface where we can apply the property for FEA software. Again, take inspiration from other slot to understand how the surfaces are defined and remember that you can ask for help on Github if needed. 

Defining the other methods
--------------------------
All the other method should be more straight forward if you follow the indication from the :doc:`slot conventions</slot.convention>`. Otherwise, you can `open an issue <https://github.com/Eomys/pyleecan/issues page>`__.

Docstring and documentation
---------------------------
Each method has a dedicated docstring that you can copy/paste/edit for your own:

.. image:: _static/tuto_slot_docstring.PNG

These docstrings are important since they are automatically scanned to generate :doc:` this website</pyleecan.Classes.SlotW10>`. So please always provide some well defined docstring, it will help others to use your object.

Step 7: Sending your modification to the project
================================================
Writting in process
(Merge, Black, Test, Commit, Pull Request)

Step 8: Adding some test
========================
Writting in process

Step 9: Adding your Slot to the GUI
===================================
Writting in process
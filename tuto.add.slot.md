
How to add a new slot in Pyleecan
=================================

Introduction
------------

Thanks to the Object-Oriented Programming architecture of Pyleecan,
adding a new topology (slot, ventilation, holes...) in Pyleecan is
rather simple. We will take the opportunity of this simple example to
**explore in depth all the know-how and hint required to do a great
contribution for the entire Pyleecan community**. We will present every
single step from installing python to doing your first "pull-request" on
Github. To do so, we will guide you through several other pages of this
website in the correct order and with the proper introduction.

Step 1: Installing Pyleecan
---------------------------

To contribute to Pyleecan the first required tool is... Pyleecan. ou
will first need to install **Python** and an **IDE** (Integrated
Development Environment), then to **download Pyleecan**. Please follow
[the procedure](get.pyleecan.md) to **"fork"
Pyleecan with git** (the "I want to contribute way"), it will be
required for the next part of this tutorial.

Step 2: Defining the Slot
-------------------------

Now that all the tools are installed, the next step is: **What are you
going to contribute to?** The first place to go is the [Github issue
page](https://github.com/Eomys/pyleecan/issues). This page gathers all
the open topic from bug to correct, to new feature to add or anything to
improve... Basically it is the place where the pyleecan community (users
and contributors) gather to discuss about what to develop next. For your
new slot, first look at the issues to see if you can help the community
by developing a slot that was requested in an issue. If you do not find
any (or if you just want to do your own) you can open an issue to
describe the slot you are about to add. By opening this issue, the
community will be able to help you during your development and maybe
tell you that the slot is already available somewhere. We can also help
you to improve the slot definition so that other people will be able to
use it. For these discussions (and for your development) you will need a
**schematic**: an image that shows the shape of your slot and what each
parameter corresponds to. To draw your schematics, we recommend to use
[Geogebra](https://www.geogebra.org/) (or any equivalent tool), it will
help you to be accurate in the definition of each parameter, which is
very important for others to understand and use your slot. In this
tutorial we will define a simple circular slot (to be used for notches)
named "SlotCirc" and we will use the following schematics:

![](_static/SlotCirc.png)

Step 3: Creating a branch with git
----------------------------------

The first step to work on a contribution is to create a "branch on
git". This [tutorial](creating.branch.md) explains all the steps required to create your own branch.

Step 4: Creating the class
--------------------------

The first modification needed is to create a new Slot class. In Pyleecan
all the class files are created with a tool known as the **class
generator**. A dedicated documentation on this tool is available
[on the website](class.generation.md). In
pyleecan/Generator/ClassesRef there is **one csv file for each class**
of Pyleecan (organized by type/folder). To create a new one, copy/paste
an existing csv file (for instance *SlotCirc*), rename it and edit it.
The name of your file will be the name of the class and it must be
unique within pyleecan. In our example we copy the csv file
"pyleecan/Generator/ClassesRef/Slot/SlotW10.csv", rename it to
"SlotCirc.csv" and here is the edited content:

![](_static/tuto_slot_csv_1.PNG)

The "left part" corresponds to the properties of the class. Two lines or
two properties has been defined: one for H0 and one for W0. Both are
defined to be "float" values greater than 0 and a proper documentation
text has been set.

![](_static/tuto_slot_csv_2.PNG)

The "right part" corresponds to global information on the class. We have
updated the method list (cf next chapter of this tutorial) and changed
the class description. Now that your csv file is properly edited, you
need to **run the code generator** to create the corresponding python
code. The corresponding command is: :

    python pyleecan/Generator/run_generate_classes.py

The class code is now available in the pyleecan/Classes folder. You can
have a look at the resulting code to see what method and feature are
automatically available but this file mustn\'t be edited as it is erased
every time the code generator is called. To change a class in pyleecan,
one must change its csv file and run the code generator.

Step 5: Defining your methods
-----------------------------

### Choosing which method to implement


In this part, we will finally write the first lines of code! And we have
a great news: you do not have to write most of them! With Object
Oriented Programming to add a new slot (with winding), the following
list of method are needed:

-   build_geometry: define the edges of the slot
-   build_geometry_wind: define the surfaces for winding
-   check: Check the slot constraints
-   comp_angle_opening: Compute the opening angle of the slot
-   comp_height: Compute the height of the slot
-   comp_height_wind: Compute the height of the winding part
-   comp_surface: Compute the surface of the Slot
-   comp_surface_wind: Compute the surface of the winding part

The conventions linked to these methods are defined in the
[following article](slot.convention.md).

In this list, build_geometry and build_geometry_wind are the only two
mandatory methods to define. All the other can be computed numerically
according to the result of these two methods. The numerical computation
code is defined as methods of the *Slot* or *SlotWind* classes. The
other methods (comp_surface, comp_height,...) can be defined to
provide a faster analytical way of computing these values. We recommend
defining all the methods, but the fastest way to add a new slot is just
to define build_geometry and build_geometry_wind. Once you know which
method you want to define, the "Methods" column in the csv file can be
edited if needed (don\'t forget to run the code generator).

### Creating the Method folder

In pyleecan, all the methods are stored in a dedicated folder that
follow this generic path
pyleecan/Methods/\<package\>/\<class_name\>/\<method_name\>.py. In our
example we need to create the folder pyleecan/Methods/Slot/SlotCirc. You
will also need to add an empty file named "__init__.py" so that the
methods can be imported in other part of pyleecan. The full folder of
*SlotCirc* can also copy/paste/edit as a template. A file for each of
the method listed in the csv file (build_geometry.py,
comp_surface.py...) is needed. Note that if a method is present in the
Methods folder but not referred in the csv file, **it will not be
available in the class**. The csv file is the exhaustive description of
the class.

### Defining the build_geometry method

For the build_geometry method, the first step is to compute the complex
coordinates of each point on the edges of the slot. Note that for some
slot we encapsulate the computation of the coordinates in a method named
"_comp_point_coordinate". Then a list of "Line" objects is created
to describe the slot edges with the following constraints:

-   The slot is centered on the 0x axis
-   The lines are ordered in trigonometrical way
-   Each Line begins with the end point of the previous one
-   Both the first and last point are on the bore radius (abs(Z)=Rbo)".

"Line" is an abstract class with the following daughter objects:

-   Segment: A straight line between two points.
-   Arc1: An arc of circle defined by two points and a radius.
-   Arc2: An arc of circle defined by a starting point, a center and an
    angle.
-   Arc3: Half a circle defined by 2 points and a direction.

The code of the other slots can be used as inspiration to define this
list of Lines.

For *SlotCirc*, the build_geometry method returns a list with only one
Line object. We compute the Z1 and Z2 coordinates to match W0 then we
compute the radius of the circle according to H0 to define the
corresponding Arc1.

### Defining the build_geometry_wind method

build_geometry_wind defines the "Winding area". It creates several
*SurfLine* objects according to the number of requested surface/layers.
Each surface must be labeled: [Wind]()\<S or R\>_R\<Radial
id\>_T\<Tangential id\>_S0 With \<S or R\> for Stator or Rotor and the
radial and tangential id are defined as follow (left image):

![](_static/winding_convention_1.PNG)

To create a *SurfLine* object, the list of lines on the edges of this
surface and a "point_ref" (a point in the surface where we can apply
the property for FEA software) are needed. The code from other slots can
again be used to understand how the surfaces are defined. There is also
the possibility to ask for help in the issue related to the slot.

For *SlotCirc*, the slot is intended to be used for notches so it
shouldn\'t contain winding. But maybe someone want to add winding in
such slot, so we defined the winding related methods anyway. We define
the "Winding area" to be the complete slot surface. To define the
original surface, we just need to add an *Arc1* between Z2 and Z1. Then
we proceed to "cut" this surface according to Nrad, Ntan.

### Defining the other methods

All the other method should be more straight forward with the indication
from the [slot conventions](slot.convention.md). Otherwise, ask for help in the issue related to the slot.

For *SlotCirc*, as the winding area matches the complete slot,
comp_surface and comp_surface_wind as well as comp_height and
comp_height_wind return the same values.

### Docstring and documentation

Each method has a dedicated docstring that can be copy/paste/edit:

![](_static/tuto_slot_docstring.PNG)

These docstrings are important since they are automatically scanned to
generate [this website](pyleecan.Classes.SlotW10.md). So please always provide some well-defined docstring, it
will help others to use the slot.

## Step 6: Integrating your contribution into PYLEECAN

Now that your slot is ready, you need to share it with the community.
Please follow this [tutorial](integrate.contribution.md) to submit your modifications to PYLEECAN.

## Step 7: Adding some tests

Any new feature should come with a test script. Please visit our
[test contribution guideline](test.contribution.md) page for advice on how to write good tests.

Step 8: Adding your Slot to the GUI
-----------------------------------

Writing in process...

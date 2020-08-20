####################
Object organization
####################

This page introduces the object-oriented code of PYLEECAN and explains how it is organized.
Object-Oriented  Programming (OOP) is a programming paradigm based on the concept of objects. The point of using OOP is to
gain in abstraction by using high level concepts  (a.k.a.  the  objects). These  objects  are  defined  by  their
attributes (characteristic values for instance the number of slots of a stator) and  their methods (functions which
interact with the object attributes, for instance a method to compute the surface of a stator lamination). There is no
need to be familiar with OOP to add your models to pyleecan: you basically "just" need to copy and adapt existing objects.

Object-oriented code of PYLEECAN
---------------------------------

The  object-oriented  code  of  PYLEECAN  is  split  into  three  main  objects  which  are composed of several sub-objects.
To add a feature/model in pyleecan, you need to create an alternative version of theses objects. Then to launch a simulation,
you will select the correct version of each object to use (for instance, a MachineSCIM with SlotType10 and a WindingDW1L).
The whole point is to see the object as concepts or black boxes (a machine, a slot, a winding...) to gain in abstraction.
For instance all alternative versions of the machine object will provide its own comp_mass method (that compute the mass of
the machine) and in the program we will just refer to machine.comp_mass() regardless of the current version of the machine
object that we are using.

The three main objects are:

- **Machine**: stores all the electrical machine topology data and enables to compute “intrinsic” quantities. You can think of it as a physical object: the electrical machine with its lamination, winding, specific material properties etc.
- **Simulation**: defines what to simulate when associating the electric machine and drive objects (Drive objects stores the electrical drive hardware and software parameters), and how to simulate it (modeling assumptions).
- **Output**:  stores  all  the  computation  results  and  provides  post-processing methods.

This separation into 3 objects is the key of PYLEECAN’s flexibility: all versions of every object are designed to be compatible
with every combination of the others. For instance if a mechanical model needs to know the machine mass, the value will be returned
by the comp_mass method regardless of how it is computed (some machines have magnet mass to add, or winding...). That way,
if you make sure that your new object/model provides all the methods as the other equivalent model, it will be useable in
all pyleecan without further work.

For further information about the main objects:

    .. toctree::
        :maxdepth: 1

        machine.object
        output.object

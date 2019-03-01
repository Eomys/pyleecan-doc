####################
Object organization
####################

According to Wikipedia, in the class-based object-oriented programming paradigm, `object <https://en.wikipedia.org/wiki/Object_(computer_science)>`__
refers to a particular instance of a class, where the object can be a combination of variables, functions, and data structures.
On this page we introduce object-oriented code of PYLEECAN and we explain how the objects are organized.

Object-oriented code of PYLEECAN
---------------------------------

The  object-oriented  code  of  PYLEECAN  is  split  in  three  main  objects  which  are composed of several sub-objects.
These objects are:

- **Machine**: stores all the electrical machine topology data and enables to compute “intrinsic” quantities. You can think of it as a physical object: the electrical machine with its lamination, winding, specific material properties etc.
- **Simulation**: defines what to simulate when associating the electric machine and drive objects (Drive objects stores the electrical drive hardware and software parameters), and how to simulate it (modeling assumptions)
- **Output**:  stores  all  the  computation  results  and  provides  post-processing methods this separation in 3 objects is the key of PYLEECAN’s flexibility.

For further informations about the main objects:

    .. toctree::
        :maxdepth: 2

        machine.object
        output.object

Multi simulation organization
-------------------------------

The objective of this tutorial is to explain the definition, the postprocessings and the storage of a multi-simulation in Pyleecan.

The following organization aims to enable:

-  multisimulation of a single model or a complete workflow
-  multisimulation of multisimulation
-  parallelization to speed up calculations
-  errors management
-  built-in postprocessings
-  efficient storage 

*MultiSimulation* class
~~~~~~~~~~~~~~~~~~~~~~~

A simulation corresponds to the computation of machine quantities at a single speed. 
*MultiSimulation* is designed to perform different simulations based on a default simulation and variations of its parameters.
To do so, *MultiSimulation* is devised to be an attribute of *Simulation* which is the default simulation. 
*MultiSimulation* contains :

=============== ============== ===============================
Attribute       Type           Description
=============== ============== ===============================
parent          *Simulation*   default simulation
dict_variable   *dict*         simulation variables to variate
list_datakeeper *[DataKeeper]* output data to keep
nb_proc         *int*          number of processes used
no_fail         *bool*         error tolerance
=============== ============== ===============================

Default simulation
^^^^^^^^^^^^^^^^^^

The default simulation is mandatory, *MultiSimulation* must be contained in a *Simulation* object.

Input variables
^^^^^^^^^^^^^^^

These variables will variate according to a dictionnary containing : 

- accessors (list): contains the accessors to the default simulation parameters to variate, e.g. *"simu.machine.stator.slot.W0"* 
- values (list): contains the values of variables stored in a list

*MultiSimulation* creates every simulation by making the cartesian product of every values.

To link some variables accessors and list in values contains tuple, e.g.:

.. code:: python

   {
       "accessors": [
           "simu.machine.stator.slot.W0",
           ("simu.input.time.value", "simu.input.Is.value")
       ],
       "values": [
           [0.001, 0.0011, 0.0012, 0.0013, 0.0014],
           [
               (array_time1, array_current1),
               (array_time2, array_current2),
               (array_time3, array_current3),
           ]
       ]
   }

This dict creates 5×3=15 simulations.

Variables to keep: *DataKeeper*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The *MultiSimulation* contains a list
of *DataKeeper* to specify which data to keep after each simulation by defining post-processing on *Output* object. 
A *DataKeeper* is a class whith five attributes: 

+--------------+------------+----------------------------------------+
| Attribute    | Type       | Description                            |
+==============+============+========================================+
| name         | *str*      | name of the data                       |
+--------------+------------+----------------------------------------+
| symbol       | *str*      | short name to access in *XOutput*      |
+--------------+------------+----------------------------------------+
| unit         | *str*      | data unit                              |
+--------------+------------+----------------------------------------+
| keeper       | *function* | function that takes an *Output* in     |
|              |            | argument and return a value            |
+--------------+------------+----------------------------------------+
| error_keeper | *function* | function that takes a *Simulation* in  |
|              |            | argument and returns a value, this     |
|              |            | attribute permits to handle errors and |
|              |            | to put NaN values in the result        |
|              |            | matrices                               |
+--------------+------------+----------------------------------------+


This is an example of datakeepers:

.. code:: python

   keepers = [
       DataKeeper(
           name = "Average Torque",
           unit = "N.m", 
           symbole = "Tem_av",
           keeper = lambda output: output.mag.Tem_av,
           error_keeper = lambda simu: np.nan
       ),
       DataKeeper(
           name = "Radial Magnetic Flux",
           unit = "H",
           symbol = "Br",
           keeper = lambda output: output.mag.Br,
           error_keeper = lambda simu: np.nan * np.zeros(
               len(simu.machine.time.value), len(simu.machine.angle.value)
           )
       )
   ]

Results from DataKeepers are stored in a dict containing ndarray with
DataKeeper.keeper(output) or DataKeeper.error_keeper(simu) results. Each ndarray has the shape of the
multisimulation.

Running *MultiSimulation*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``Simulation.run`` method enables to run the MultiSimulation. When the method is called, it checks if a MultiSimulation is defined and runs it if needed.
If a multisimulation is defined, the method returns the new object *XOutput* else it
returns an *Output*.

If the simulation has no *Output* defined as a parent, it is now created in the
method.

*XOutput* class
~~~~~~~~~~~~~~~

*XOutput* is a daughter of *Output* that enables to store *MultiSimulation* results:

+----------------+--------------+------------------------------------+
| Attribute      | Type         | Description                        |
+================+==============+====================================+
| simu           | *Simulation* | Default *Simulation*               |
+----------------+--------------+------------------------------------+
| geo            | *OutGeo*     | Default *Simulation* geometry      |
|                |              | output                             |
+----------------+--------------+------------------------------------+
| elec           | *OutElec*    | Default *Simulation* electrical    |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| mag            | *OutMag*     | Default *Simulation* magnetic      |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| force          | *OutForce*   | Default *Simulation* force module  |
|                |              | output                             |
+----------------+--------------+------------------------------------+
| struct         | *OutGeo*     | Default *Simulation* structural    |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| post           | *OutPost*    | Default *Simulation*               |
|                |              | post-processing settings           |
+----------------+--------------+------------------------------------+
| input_variable | *ndarray*    | Variables values for each          |
|                |              | simulation                         |
+----------------+--------------+------------------------------------+
| output_list    | *list*       | List containing each *Output*      |
+----------------+--------------+------------------------------------+
| xout_dict      | *dict*       | Dictionnary containing             |
|                |              | *MultiSimulation* *DataKeeper*     |
|                |              | results in ndarray                 |
+----------------+--------------+------------------------------------+

Default simulation results are stored in the properties inherited from Output and other simulation
results are stored in a list of *Output* and/or in a dict containing
ndarray, according to *MultiSimulation* parameters. Variables that vary are stored in a specific
dictionnary.

If the *MultiSimulation* has no *DataKeeper*, each output is stored in the
output_list. This mod should be avoided for memory reasons. 

The class has some getters to gather results: *ndarray* slices can be extracted according to some input values
e.g. extract average torque for simulations with a specific value of slot angle or a specific
speed.

.. code:: python

   xouput['Tem_av'][0,:]


Multi simulation organization
-------------------------------

The objective of this tutorial is to explain the definition, the postprocessings and the storage of a multi-simulation in Pyleecan.

The following organization aims to enable:

-  multi-simulation of a single model or a complete workflow
-  multi-simulation of multi-simulation
-  parallelization to speed up calculations
-  errors management
-  built-in postprocessings
-  efficient storage 

*VarParam* class
~~~~~~~~~~~~~~~

A simulation, as defined by the *Simulation* object, corresponds to the computation of machine quantities on a single operating point. 
A multi-simulation is defined as a list of simulations, all based on a reference simulation with variations of its parameters.

To define a multi-simulation in pyleecan, first the reference simulation must be defined as a Simulation object. Then an optional *VarParam* object (that inherit from an abstract VarSimu class) is set as a property of the reference simulation object. The *VarParam* object defines how to generate the simulation list, how to parallelize (or not) the computation and which data to gather. To do so, the *VarParam* class contains :

================== ============== ===============================
Attribute           Type           Description
================== ============== ===============================
dict_variable       *dict*         simulation parameters to variate
list_datakeeper     *[DataKeeper]* output data to keep
nb_proc             *int*          number of processes used
is_keep_all_output  *bool*         True to keep all the outputs
no_fail             *bool*         error tolerance
================== ============== ===============================

On a side note, as all pyleecan object, *VarParam* also have a parent property that links to the reference simulation. *VarParam* must be defined as a property of a *Simulation*. 

Input variables
^^^^^^^^^^^^^^^

These variables will variate according to a dictionnary containing : 

- accessors (list): contains the accessors to the default simulation parameters to variate, e.g. *"simu.machine.stator.slot.W0"* 
- values (list): contains the values of variables stored in a list

*VarParam* creates every simulation by making the cartesian product of every values.

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

The *VarParam** contains a list of *DataKeeper* to specify which data to keep after each simulation by defining post-processing on *Output* object. 
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

Results from DataKeepers are stored in a dict containing ndarray with DataKeeper.keeper(output) or DataKeeper.error_keeper(simu) results. Each ndarray has the shape of the multi-simulation.

Running *VarParam*
^^^^^^^^^^^^^^^^^^

When the method ``Simulation.run`` is called, we first run the reference simulation. Then, if a VarParam is defined, the corresponding list of simulation is generated and run. If a VarParam is defined, ``Simulation.run`` returns a *XOutput* object else it returns an *Output*.

If the simulation has no *Output* defined as a parent, it is now created in the method.

*XOutput* class
~~~~~~~~~~~~~~~

*XOutput* is a daughter of *Output* that enables to store *MultiSimulation* results:

+----------------+--------------+------------------------------------+
| Attribute      | Type         | Description                        |
+================+==============+====================================+
| simu           | *Simulation* | Reference *Simulation*               |
+----------------+--------------+------------------------------------+
| geo            | *OutGeo*     | Reference *Simulation* geometry      |
|                |              | output                             |
+----------------+--------------+------------------------------------+
| elec           | *OutElec*    | Reference *Simulation* electrical    |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| mag            | *OutMag*     | Reference *Simulation* magnetic      |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| force          | *OutForce*   | Reference *Simulation* force module  |
|                |              | output                             |
+----------------+--------------+------------------------------------+
| struct         | *OutGeo*     | Reference *Simulation* structural    |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| post           | *OutPost*    | Reference *Simulation*               |
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

Reference simulation results are stored in the properties inherited from Output and other simulation results are stored in a list of *Output* and/or in a dict containing ndarray, according to *MultiSimulation* parameters. Variables that vary are stored in a specific dictionnary.

If the VarParam.is_keep_all_output is True, each output of each simulation is stored in the output_list. This option is by default at False to avoid memory issues. 

The class has some getters to gather results: *ndarray* slices can be extracted according to some input values
e.g. extract average torque for simulations with a specific value of slot angle or a specific
speed.

.. code:: python

   xouput['Tem_av'][0,:]


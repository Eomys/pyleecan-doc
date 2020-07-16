Multi-simulation organization
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

To define a multi-simulation in pyleecan, first the reference simulation must be defined as a *Simulation* object. Then an optional *VarParam* object (that inherit from an abstract *VarSimu* class) is set as a property of the reference simulation object. *VarParam* object defines how to generate the simulation list, how to parallelize (or not) the computation and which data to gather. To do so, *VarParam* class contains :

+--------------------+-----------------+------------------------+
| Attribute          | Type            | Description            |
+====================+=================+========================+
| paramsetter_list   | *[ParamSetter]* |    simulation          |
|                    |                 |    parameters to       |
|                    |                 |    variate             |
+--------------------+-----------------+------------------------+
| datakeeper_list    |                 |    output data to keep |
|                    |  *[DataKeeper]* |                        |
+--------------------+-----------------+------------------------+
| nb_proc            |    *int*        |    number of processes |
|                    |                 |    used                |
+--------------------+-----------------+------------------------+
| is_keep_all_output |    *bool*       |    True to keep every  |
|                    |                 |    output              |
+--------------------+-----------------+------------------------+
| stop_if_error      |    *bool*       |    error tolerance     |
+--------------------+-----------------+------------------------+

On a side note, as all pyleecan object, *VarParam* also have a parent property that links to the reference simulation. *VarParam* must be defined as a property of a *Simulation*. 

Input parameters: *ParamSetter*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*ParamSetter* enables to set the parameters variations by defining a function that takes a *Simulation* and a value in argument. Using such function enable to link some input parameters together. This setter can also be defined as a string to target directly a parameter. 
When generating the list of simulation to run, the function is executed before running the simulation to set the parameters as expected. 
The object has five attributes:

+--------------+------------+----------------------------------------+
| Attribute    | Type       | Description                            |
+==============+============+========================================+
| name         | *str*      | name of the data                       |
+--------------+------------+----------------------------------------+
| symbol       | *str*      | short name to access in *XOutput*      |
+--------------+------------+----------------------------------------+
| unit         | *str*      | data unit                              |
+--------------+------------+----------------------------------------+
| setter       | *function* | function that takes a *Simulation* and |
|              |            | a value in argument and modifies the   |
|              |            | simulation                             |
+--------------+------------+----------------------------------------+
| value_list   | *list*     | list that contains the different       |
|              |            | parameters values to explore           |
+--------------+------------+----------------------------------------+

*VarParam* creates every simulation by making the cartesian product of *ParamSetter* values:

.. code:: python

   def slot_scale(simu, scale_factor):
      """Edit stator slot size according to a percentage
      
      Parameters
      ----------
      simu: Simulation
        simulation to modify
      scale_factor: float
        stator slot scale factor
      """
      simu.machine.stator.slot.W0 *= scale_factor
      simu.machine.stator.slot.W1 *= scale_factor
      simu.machine.stator.slot.W2 *= scale_factor
      simu.machine.stator.slot.H0 *= scale_factor
      simu.machine.stator.slot.H1 *= scale_factor

   paramsetter_list=[
       ParamSetter(
           name = "Stator slot scale factor",
           symbol = "stat_slot",
           unit="",
           setter=slot_scale,
           value_list = [0.99, 0.101]
       ),
       ParamSetter(
           name = "Current",
           symbol = "I",
           unit="A",
           setter="simu.input.Is.value",
           value_list = [array_current1, array_current2, array_current3]
       ),
   ]

``slot_scale`` function variate every slot parameters in one function. The list above creates the six following simulations:

+-------------------+-----------------------------+----------------------+
| simulation number | Stator slot scale factor    | Stator current       |
+===================+=============================+======================+
| 1                 | 0.99                        | array_current1       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+
| 2                 | 0.99                        | array_current2       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+
| 3                 | 0.99                        | array_current3       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+
| 4                 | 0.101                       | array_current1       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+
| 5                 | 0.101                       | array_current2       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+
| 6                 | 0.101                       | array_current3       |
|                   |                             |                      |
+-------------------+-----------------------------+----------------------+

Variables to keep: *DataKeeper*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*VarParam* contains a list of *DataKeeper* to specify which data to keep after each simulation by defining post-processing on *Output* object. 
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


This following datakeepers enable to store the average torque and the radial magnetic flux for each of the six simulations:

.. code:: python

   datakeeper_list = [
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

Results from DataKeepers are stored in a dict whose keys are the data symbol and values are ndarray containing results from DataKeeper.keeper(output) or DataKeeper.error_keeper(simu). Each ndarray has the shape of the multi-simulation, in this case, the matrix shape 2×3 where the rows corresponds to the stator scale factor and the columns represent the current.

Running *VarParam*
^^^^^^^^^^^^^^^^^^

When the method ``Simulation.run`` is called, we first run the reference simulation. Then, if a VarParam is defined, the corresponding list of simulation is generated and run. If a VarParam is defined, ``Simulation.run`` returns a *XOutput* object else it returns an *Output*.

If the simulation has no *Output* defined as a parent, it is now created in the method.

*XOutput* class
~~~~~~~~~~~~~~~

*XOutput* is a daughter of *Output* that enables to store *VarParam* results:

+----------------+--------------+------------------------------------+
| Attribute      | Type         | Description                        |
+================+==============+====================================+
| simu           | *Simulation* | Reference *Simulation*             |
+----------------+--------------+------------------------------------+
| geo            | *OutGeo*     | Reference *Simulation* geometry    |
|                |              | output                             |
+----------------+--------------+------------------------------------+
| elec           | *OutElec*    | Reference *Simulation* electrical  |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| mag            | *OutMag*     | Reference *Simulation* magnetic    |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| force          | *OutForce*   | Reference *Simulation* force       |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| struct         | *OutStruct*  | Reference *Simulation* structural  |
|                |              | module output                      |
+----------------+--------------+------------------------------------+
| post           | *OutPost*    | Reference *Simulation*             |
|                |              | post-processing settings           |
+----------------+--------------+------------------------------------+
| input_param    | *ndarray*    | Parameters values for each         |
|                |              | simulation                         |
+----------------+--------------+------------------------------------+
| output_list    | *list*       | List containing each *Output*      |
+----------------+--------------+------------------------------------+
| xoutput_dict   | *dict*       | Dictionnary containing             |
|                |              | *VarParam* *DataKeeper*            |
|                |              | results in ndarray                 |
+----------------+--------------+------------------------------------+

Reference simulation results are stored in the properties inherited from Output and other simulation results are stored in a list of *Output* and/or in a dict containing ndarray, according to *VarParam* parameters. Paramaters variation are stored in a specific dictionnary.

If ``VarParam.is_keep_all_output`` is True, each output of each simulation is stored in the output_list. This option is set as False by default to avoid memory issues. 

The class has some getters to gather results: *ndarray* slices can be extracted according to some input values
e.g. extract average torque for simulations with a specific value of slot angle or a specific
speed. To ease the access to the results, *XOutput* behave like a dictionary to access directly to ``XOutput.xout_dict`` and like a list to access directly to ``XOuput.output_list``. Furthermore, ``len(XOutput)`` returns the number of simulation, which is 6 in this case. For this example, the following call returns a 1×3 matrix containing the average torque for each simulation with the stator scale factor set to 0.99. 

.. code:: python

   xouput['Tem_av'][0,:]


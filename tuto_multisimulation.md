
How to run a multi-simulation
=============================

The objective of this tutorial is to explain the definition, the
postprocessings and the storage of a multi-simulation in Pyleecan.

The following organization aims to enable:

-   multi-simulation of a single model or a complete workflow
-   multi-simulation of multi-simulations
-   parallelization to speed up calculations
-   errors management
-   built-in postprocessings
-   efficient storage

*VarParam* class
----------------

A simulation, as defined by the *Simulation* object, corresponds to the
computation of machine quantities on a single operating point. A
multi-simulation is defined as a list of simulations, all based on a
reference simulation with variations of its parameters.

To define a multi-simulation in pyleecan, first the reference simulation
must be defined as a *Simulation* object. Then an optional *VarParam*
object (that inherits from an abstract *VarSimu* class) is set as a
property of the reference simulation object. *VarParam* object defines
how to generate the simulation list, how to parallelize (or not) the
computation and which data to gather. To do so, *VarParam* class
contains :


| Attribute          | Type                | Description                                                                                               |
| ------------------ | ------------------- | --------------------------------------------------------------------------------------------------------- |
| paramexplorer_list | *\[ParamExplorer\]* | simulation parameters to variate                                                                          |
| datakeeper_list    | *\[DataKeeper\]*    | output data to keep                                                                                       |
| nb_proc            | *int*               | number of processes used                                                                                  |
| is_keep_all_output | *bool*              | True to keep every output                                                                                 |
| stop_if_error      | *bool*              | error tolerance                                                                                           |
| ref_simu_index     | *int*               | Index of the reference  simulation, if None the </br> reference simulation is not in the multi-simulation |
| nb_simu            | *int*               | number of simulations                                                                                     |


On a side note, as all pyleecan object, *VarParam* also has a parent
property that links to the reference simulation.

Input parameters: *ParamExplorerSet*
------------------------------------

The parameter to change in the reference simulation and how are defined
with *ParamExplorer* objects. *ParamExplorerSet* is a *ParamExplorer*
that enables to set the parameters variations from a list of values. The
parameters to change are defined with a function (setter) which enables
to do complex operation with the value as argument (cf example below).
This setter can also be defined as a string to target directly a
parameter. When generating the list of simulations to run, the function
is executed before running the simulation to set the parameters as
expected. The *ParamExplorerSet* has five attributes:

| Attribute | Type       | Description                                                                                        |
| --------- | ---------- | -------------------------------------------------------------------------------------------------- |
| name      | *str*      | name of the data                                                                                   |
| symbol    | *str*      | short name to access in *XOutput*                                                                  |
| unit      | *str*      | data unit                                                                                          |
| setter    | *function* | function that takes a *Simulation* and a </br> value in argument and modifies the </br> simulation |
| value     | *list*     | list that contains the different </br> parameter values to explore                                 |


When a *VarParam* is defined with several *ParamExplorer*, it will
creates every simulation by making the cartesian product of every
*ParamExplorerSet* values.

In the following example, two *ParamExplorer* are defined, the first one
scales every parameter of the slot according to a single value, the
second directly update the current matrix:

``` {.python}
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

paramexplorer_list=[
    ParamExplorerSet(
        name = "Stator slot scale factor",
        symbol = "stat_slot",
        unit="",
        setter=slot_scale,
        value = [0.99, 1.01]
    ),
    ParamExplorerSet(
        name = "Current",
        symbol = "I",
        unit="A",
        setter="simu.input.Is.value",
        value = [array_current1, array_current2, array_current3]
    ),
]
```

A *VarParam* with both the *ParamExplorer* above creates the six
following simulations:

| simulation number | Stator slot scale factor | Stator current |
| ----------------- | ------------------------ | -------------- |
| 1                 | 0.99                     | array_current1 |
| 2                 | 0.99                     | array_current2 |
| 3                 | 0.99                     | array_current3 |
| 4                 | 1.01                     | array_current1 |
| 5                 | 1.01                     | array_current2 |
| 6                 | 1.01                     | array_current3 |

Variables to keep: *DataKeeper*
-------------------------------

*VarParam* contains a list of *DataKeeper* to specify which data to keep
after each simulation by defining post-processing on *Output* object. A
*DataKeeper* is a class with six attributes:

| Attribute    | Type       | Description                                                                                                                                                                                                         |
| ------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name         | *str*      | name of the data                                                                                                                                                                                                    |
| symbol       | *str*      | short name to access in *XOutput*                                                                                                                                                                                   |
| unit         | *str*      | data unit                                                                                                                                                                                                           |
| keeper       | *function* | function that takes an *Output* in argument and  </br> returns a value                                                                                                                                              |
| error_keeper | *function* | function that takes a *Simulation* in argument and </br> returns a value, this attribute permits to handle </br> errors and  attribute permits to handle errors and </br>  to put NaN values in the result matrices |
| result       | *list*     | list containing DataKeeper results for each simulation                                                                                                                                                              |

This following datakeepers enable to store the average torque and the
radial magnetic flux for each of the six simulations:

``` {.python}
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
```

DataKeepers with their results are stored in a dict whose keys are the
data symbol. DataKeepers results contain results from
*DataKeeper.keeper(output)* (or *DataKeeper.error_keeper(simu)* when
the simulation raise an error).

Running *VarParam*
------------------

When the method `Simulation.run` is called, the reference simulation is
executed first. Then, if a VarParam is defined, the corresponding list
of simulations is generated and run. If a VarParam is defined,
`Simulation.run` returns an *XOutput* object else it returns an
*Output*.

*XOutput* class
---------------

*XOutput* is a daughter of *Output* that enables to store *VarParam*
results:

| Attribute    | Type         | Description                                                            |
| ------------ | ------------ | ---------------------------------------------------------------------- |
| simu         | *Simulation* | Reference *Simulation*                                                 |
| geo          | *OutGeo*     | Reference *Simulation* geometry output                                 |
| elec         | *OutElec*    | Reference *Simulation* electrical module output                        |
| mag          | *OutMag*     | Reference *Simulation* magnetic module output                          |
| force        | *OutForce*   | Reference *Simulation* force module output                             |
| struct       | *OutStruct*  | Reference *Simulation* structural module output                        |
| post         | *OutPost*    | Reference *Simulation* post-processing settings                        |
| input_param  | *list*       | List of *ParamExplorerSet* containing </br> values for each simulation |
| output_list  | *list*       | List containing each *Output*                                          |
| xoutput_dict | *dict*       | Dictionnary containing *VarParam* *DataKeeper*                         |

Reference simulation results are stored in the properties inherited from
Output and other simulation results are stored in a list of *Output*
and/or in a dict containing *DataKeeper*, according to *VarParam*
parameters. Paramater variations are stored in a specific list of
*ParamExplorerSet* created at the beginning of the simulation.

If `VarParam.is_keep_all_output` is True, then each output of each
simulation is stored in the output_list. This option is set as False by
default to avoid memory issues.

The class has some getters to gather results: list slices can be
extracted according to some input values e.g. extract average torque for
simulations with a specific value of slot angle or a specific speed. To
ease the access to the results, *XOutput* behaves like a dictionary to
access directly to `XOutput.xout_dict` and like a list to access
directly to `XOuput.output_list`. Furthermore, `len(XOutput)` returns
the number of simulations, which is 6 in this case. For this example,
the following call returns a list containing the average torque for each
simulation with the stator scale factor set to 0.99.

``` {.python}
xouput['Tem_av'].result[0:3]
```

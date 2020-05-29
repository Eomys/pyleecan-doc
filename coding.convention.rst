##################
Coding convention
##################
.. role:: red
.. role:: green

PEP8
----

PYLEECAN will be developed in Python with an oriented object approach to provide a « simple » code that
anyone can contribute to. Python is a language that was thought to be readable. The Python community has written
some rules about how a good readable Python code should be written. These rules are known as the Python Enhancement
Proposal 8 (PEP8): https://www.python.org/dev/peps/pep-0008/.


The **PYLEECAN project follows the PEP8 rules** to unify the coding style of all the contributors. The code will be
more homogenous and understandable this way. Some tools are used to « correct » any Python script to follow the PEP8 rules.
For PYLEECAN modules, we will use `Black <https://black.readthedocs.io/en/stable/>`__.


Naming convention
------------------

The PEP8 conversion scripts can’t correct all the PEP8 rules. For instance, the PEP8 provides some naming
convention. In this paragraph some naming conventions of PYLEECAN project are detailed, as well as global
coding rules to improve the code quality.

- According to PEP8: Class names must follow CamelCase naming (Each new word is capitalized without underscore)


    +------------------------+------------------------------+-------------------------------------------------------+
    | :red:`NOT TO DO`       |    Lam_squirrel_Cage         |  Name of squirrel cage rotor class                    |
    +------------------------+------------------------------+-------------------------------------------------------+
    | :green:`TO DO`         |    LamSquirrelCage           |                                                       |
    +------------------------+------------------------------+-------------------------------------------------------+

- According to PEP8, function and variable names should be lowercase with words separated by underscores. In PYLEECAN, small exceptions are made to this rule: a method or a variable name can have capitalized letter if and only if it follows physical quantities (radius=R, number=N or Z, length=L etc). For instance, Zs is used for the number of slots or Rbo for the lamination bore radius.

    +------------------------+-----------------------------------+--------------------------------------------------+
    | :red:`NOT TO DO`       |   S=compSurfaceRadiation()        |  Calculates the radiation surface                |
    +------------------------+-----------------------------------+--------------------------------------------------+
    | :green:`TO DO`         |   S=comp_surface_radiation()      |                                                  |
    +------------------------+-----------------------------------+--------------------------------------------------+


- For functions that compute some numerical quantities, called “computation functions” (in the sense that mathematical operations are carried on numerical data, even a simple multiplication), the convention is to start the name by “comp\_” like “computation”. By default, a “comp\_” function will return the corresponding numerical value (a series of scalar, vector, matrices) and the calling function stores it in the Output object. That way the user can find all the available computation by typing “.comp_”.

    +------------------------+--------------------------------+-----------------------------------------------------+
    | :red:`NOT TO DO`       |   B=flux_airgap_SDM()          |  Calculates airgap flux with SubDomain Model        |
    +------------------------+--------------------------------+-----------------------------------------------------+
    | :green:`TO DO`         |   B=comp_flux_airgap_SDM()     |                                                     |
    +------------------------+--------------------------------+-----------------------------------------------------+

- When a computation function computes and returns several numerical values, these values are stored in a dictionary, and the keys are the same as the names used in the Output object to be easily set (e.g. Output.Geometry.Smag stores the surface of magnets calculated with Smag=comp_surface_magnet()).

    +------------------------+----------------------------------------------------------+---------------------------+
    | :red:`NOT TO DO`       |   - S=comp_surface_magnet()                              |                           |
    |                        |   - Output.Geometry.Smag=S                               |                           |
    +------------------------+----------------------------------------------------------+---------------------------+
    | :green:`TO DO`         |   - Smag=comp_surface_magnet()                           |                           |
    |                        |   - Output.Geomtry.Smag=Smag                             |                           |
    +------------------------+----------------------------------------------------------+---------------------------+

- For computation functions, the physical type of computation should be used first in the name of the function (surface, resistance, inductance…). This way, the user can more easily access to unknown methods by listing all the computation commands with tab completion key (to find the methods calculating torque, comp_torque+tab key gives comp_torque_Maxwell(), comp_torque_equivalent_circuit() etc.)

    +------------------------+----------------------------------------------------------+---------------------------+
    | :red:`NOT TO DO`       |   - comp_radiation_surface()                             |                           |
    |                        |   - comp_Maxwell_torque()                                |                           |
    +------------------------+----------------------------------------------------------+---------------------------+
    | :green:`TO DO`         |   - comp_surface_radiation()                             |                           |
    |                        |   - comp_torque_Maxwell()                                |                           |
    +------------------------+----------------------------------------------------------+---------------------------+

- Computation function names can be abbreviated with predefined abbreviations (see CSV sheet PYLEECAN scripting conventions table.csv), for instance comp_ind_leak_ew_ANL() to compute analytically (ANL) the end-winding (ew) leakage (leak) inductance (ind).

    +------------------------+----------------------------------------------------------+--------------------------+
    | :red:`NOT TO DO`       |   comp_inductance_leakage_endwinding_analytic            |                          |
    +------------------------+----------------------------------------------------------+--------------------------+
    | :green:`TO DO`         |   comp_ind_leak_ew_ANL()                                 |                          |
    +------------------------+----------------------------------------------------------+--------------------------+

- By default, plural “s” should not be used the function names (comp_force and not comp_forces).

    +------------------------+------------------------------+------------------------------------------------------+
    | :red:`NOT TO DO`       |   comp_forces_magnetic()     |                                                      |
    +------------------------+------------------------------+------------------------------------------------------+
    | :green:`TO DO`         |   comp_force_magnet()        |                                                      |
    +------------------------+------------------------------+------------------------------------------------------+

- As much as possible, computation functions should not use the Output object as an input argument (except for the most complex ones such as comp_displacement). The argument is explicitly listed and fetched in the Output object by the calling function. This way, it is easier to know the input data necessary to call a given computation function, optional arguments can be added with explicit docstrings, and the call is easier because it doesn’t require to create an Output object.

    +------------------------+--------------------------------+----------------------------------------------------+
    | :red:`NOT TO DO`       |   comp_vol_mag(Output)         | Calculates the volume of magnets with              |
    |                        |                                | Output.Geometry.Smag, Output.Geometry.Lmag etc     |
    +------------------------+--------------------------------+----------------------------------------------------+
    | :green:`TO DO`         |   comp_vol_mag(Smag, Lmag)     | Calculates directly the volume of magnets          |
    |                        |                                | with Smag, Lmag, etc                               |
    +------------------------+--------------------------------+----------------------------------------------------+

-  All the hidden internal methods (or properties) that the user shouldn’t use should start with underscore “_” for instance: _comp_point_coordinates is a private method

    +------------------------+------------------------------+------------------------------------------------------+
    | :red:`NOT TO DO`       |   comp_point_coordinates()   |  Private method calculating a point coordinates      |
    +------------------------+------------------------------+------------------------------------------------------+
    | :green:`TO DO`         |   _comp_point_coordinates()  |                                                      |
    +------------------------+------------------------------+------------------------------------------------------+


- As much as possible, getters should be used to ease further evolutions. For instance, instead of directly accessing the  Frame.Wfra property, a get_width() method of the Frame returning Wfra should be implemented. PYLEECAN 1.0 contains a single Frame object (circular frame), but there might be new ones. If the Simulation objects use the get_width() method, creating a new Frame object (rectangular for instance) would “only” require to have the get_width() method implemented to be compatible with all the code.

    +------------------------+------------------------------+-------------------------------------------------------+
    | :red:`NOT TO DO`       |   Frame.width                |  Direct access to an object attribute                 |
    +------------------------+------------------------------+-------------------------------------------------------+
    | :green:`TO DO`         |   Frame.get_width()          |  Access through a getter                              |
    +------------------------+------------------------------+-------------------------------------------------------+


- The set of functions that computes a physical quantity based on some another physical quantity (e.g. calculation of magnetic flux based on electrical currents) is called a “module” – it can be seen as a transfer function from one physics to another. The modelling methodology used to run the calculations in this module is called a “model”. This model may be only valid when fulfilling some specific conditions – additional model parameters are called “assumptions”. As an example, the Structural “module” contains an Analytical “model” with different boundary conditions “assumptions”.

Commit message convention
-------------------------
When committing modifications, we add a tag at the beginning of the commit message to indicate the nature of the commit:
- [VI]: (Version Information) Main development step achieved or meaningful version of the software
- [NF]: (New Feature) Something important have been added and works (a new function, a new post-treatment…)
- [BC]: (Bug Correction) A bug was corrected (the message should contain description of the bug or the corresponding issue number)
- [WP]: (Work in Progress) Some intermediate changes leading to BC / NF
- [CO]: (Code Optimization) No new functionality but the scripting is improved in terms of clarity, CPU time, etc
- [CC]: (Code Cleaning) Nearly “passive” development (comments added, split of function in several functions, typo correction...)
- [VR]: (Variable Renaming) Change of variable or class name, important to be tagged separately

When possible, please refer to the corresponding issue in your commit message with #<issue number>, Github will automatically add a link to the commit in the issue.

Geometrical convention
----------------------
Here is a list of article about some global geometrical conventions in pyleecan:

.. toctree::
    :maxdepth: 1

    slot.convention
    winding.convention

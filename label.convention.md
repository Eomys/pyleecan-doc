################
Geometry modeler and label Conventions
################

Pyleecan uses Object Oriented programming to provide a generical geometrical modeler. It converts any geometry pyleecan can handle into a list of generical surfaces and lines that can then be drawn more easily in a FEA software. Then when adding a new topology in pyleecan, it becomes autmatically available in all the FEA software pyleecan has a coupling with (FEMM, gmsh,...). A publication in ICEM 2020 further introduces this concept: https://pyleecan.org/icem.2020.html

When drawing the machine in a FEA software, first the lines are drawn then the surfaces and boundary condition need to be defined. This article introduces the lamination, surfaces and lines label conventions that enable to link a generical surface/line to the property to set. 

Lamination label conventions:
#############################
In pyleecan not all machines have two laminations. All laminations are refered with a unique label like "Stator-<id>" or "Rotor-<id>" with <id> the number of the lamination of this kind (rotor or stator) from in to out. Exemple:
.. image:: _static/Lamination_label.png

For a lamination with a rotor and a stator the corresponding labels would be "Stator-0" and "Rotor-0".

Surface label conventions:
Surface label must be unique within the machine to enable defining different property in any surface. The surface label is organized as follow: <lamination_label>_<surface_type>_<surface_index> with:
- <lamination_label> as described above ("Stator-0" for instance)
- <surface_type> one from the following list: "Winding", "Magnet", "HoleMag", "HoleVoid", "Ventilation", "Lamination"
- <surface_index> RX-TY-SZ with "R" for Radial, "T" for Tangential and "S" for Slot. This coordinate system is further details in the corresponding article (winding: https://pyleecan.org/winding.convention.html)
For instance the label of the 1st radial layer, 2nd tangential layer of the winding in the 8th slot of the first rotor would be: "Rotor-0_Winding_R0-T1_S7"

How to handle labels:
####################
All labels definition and important functions are gathered in pyleecan/Functions/labels.py. All label definition (in build_geometry methods for instance) must use the variables defined in this file to make sure that the label are consistant and to enable renaming them (Stator => stat for instance).

    | <span style="color:red">NOT TO DO</span> | if "Stator" in label:     |
    | ---------------------------------------- | --------------------------------------------------------- |
    | <span style="color:green">TO DO</span>   | if STATOR_LAB in label: |

################
Slot Conventions
################

For  the  definition  of  the  slot  geometry  with build_geometry()  the  following conventions are used: 
- Start  and  end  points  of  the  curve  defining  a  slot  shape  should  be  on  the lamination bore radius 
- Line objects are defined in the trigonometric direction (therefore the imaginary part of the first point is negative and the one of the last point is positive)
- All the coordinates are by default returned with the Ox axis as the symmetries axis of the slot (or in the middle of the slot opening if the slot is not symmetrical)
- Points are named Px, their complex coordinate is Zx. The arc center points are named Pcx in schematics

.. image:: _static/slot_convention_1.png

- The top of the slot is the closest part to the bore radius, the bottom of the slot is the closest part to the yoke (for both outer and inner rotor)
- The slot bottom radius is the radius of the point which is the furthest from the bore
- The slot height is defined as the difference between the bore radius and the slot bottom radius
- The  top  of  the  slot  surface  is  always  defined  by  the  lamination  bore  radius (therefore,  the  small  arc  surface  needs  to  be  taken  into  account  in  the  slot surface calculation)
- The yoke surface is defined as the cylinder between the slot top radius and the radius  of  the  lamination  which  is  not  the  bore  radius  (Rext  for  external lamination). 
- The teeth surface (and therefore the teeth) is defined as the remaining surface between the bore radius and the slot bottom radius

Slot geometry convention (external lamination):
.. image:: _static/slot_convention_2.png

 Slot geometry convention (internal lamination):
.. image:: _static/slot_convention_3.png

- The opening angle of the slot is the angle of the arc fromed by the first and last points of the slot along  the lamination bore radius, with O=(0,0) as the center.
- The equivalent winding angle is the angle of the equivalent polar geometry of the slot winding surface. This equivalent polar geometry is defined assuming the same height as the real winding, the angle being such as the equivalent polar surface equals the real winding surface.
- The winding height is define as the difference between the slot bottom radius and the radius of the closest winding point from the lamination bore
- The opening height is defined as the difference between the radius of the closest winding point from the lamination bore and the lamination bore radius
- The mid-winding radius is defined as the slot top radius minus half the winding height for external lamination or as the slot top radius plus half the winding height for internal lamination.

Several of these methods use polar definitions for the height or for the angle to be able to easily define a polar equivalent of the machine (for instance for subdomain model – Magnetic module). 

All the SlotWind objects have the following methods: 
- _comp_coordinates(): (optional) Returns a list of the complex point coordinates in the order (P1, P2, P3...). If there are some arc in the points (for instance an arc between P2, P3 of center Pc1), the convention is to return the center coordinates too (P1, P2, Pc1, P3, P4 ...)
- build_geometry(): Returns a list of Line objects that enables to draw in 2D one slot in trigonometric direction (no bore arc, the beginning of each Line is the end of the previous one).
- build_geometry_wind(): Returns a list of Surface that enables to draw in 2D the winding zones (taking 2 integer, Nrad and Ntan, as arguments to set the number of winding zones to be drawn).
- check():  Checks  that  all  the  geometrical  constraints  are  respected,  throw exceptions otherwise.
- comp_angle_opening(): Returns the opening angle of the slot.
- comp_angle_wind_eq(): Returns the equivalent angle of the winding part of the slot (method of SlotWind).
- comp_height(): Returns the slot’s height.
- comp_height_wind(): Computes the winding’s height.
- comp_surface(): Computes the surface of a single slot.
- comp_surface_wind():  Computes  the  winding’s  maximum  theoretical  active surface.
- comp_radius_mid_wind():  Computes  the  mid  winding  radius  (method  of SlotWind).


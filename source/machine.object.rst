#############################
Machine & Simulation objects
#############################

In the Machine class, the machine methods are only meant to provide “intrinsic” quantities of the machine
(e.g.  geometrical  quantities,  weights). This  constraint  helps differentiating  the  methods  between  the Machine
and  the  Simulation  objects.  When adding a new machine topology, one must be sure that it works with all
the simulation possibilities of PYLEECAN.

Methods organization
---------------------

The object-oriented approach helps to integrate a new topology by listing all the methods that need to be implemented.
Therefore, to ease the creation of new topologies, the number of methods in the machine object must be limited.
As  an  example,  the  Machine  methods  include  a  comp_coefficient_Carter()(calculation of the Carter slotting
coefficient) and a comp_resistance_winding() method (calculation of  the total  electric  resistance  per  phase),
but  no comp_flux_airgap() nor comp_circuit_equivalent() method in the Machine. The methods comp_flux_airgap() and
comp_circuit_equivalent() are defined within the Simulation object and sorted by physics. This way, the Machine object
provides a unified and simplified framework to handle the machine topology and the Simulation project splits and
organizes the different models.

Constraints
''''''''''''
This  philosophy  implies  several  constraints  on  the code  organization.  When coupling  PYLEECAN  with  FEMM
magnetostatics  solver  to  compute  the  airgap  flux distribution, one simple possibility would be to add a draw_femm()
method in Machine and all  its  subclasses to  define the  geometry  in  FEMM  in  one  call. The  problem  with this method
is that the code for FEMM coupling would then be split between Machine and Simulation objects, and adding a new machine
topology would then require to  define another draw_femm()  method,  even  if  the machine  was  not  intended  for
FEMM simulation.  Another  issue  is  that  the  Machine  objects  would need  a  dedicated  draw method for each software
(e.g. Ansys, Flux, Code_Aster) that would replicate part of the code of draw_femm().

Solution
'''''''''

The  solution  (that  is  detailed  later  in  this  document)  is  to  create  a build_geometry() method in Machine
class that returns a unified geometry representation of the machine (though a list of Surface) which is intrinsic of the
machine and not model-dependent. Then, each software coupling should call this method to draw the machine accordingly.
With this coding principle, all the model-related code is in the corresponding Simulation  part,  and  when  adding  a
new topology,  implementing the build_geometry() method should make it compatible with all the existing models.

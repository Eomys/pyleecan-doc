##############
Output object
##############

The  Output  class  is  composed  of  several  subclasses  corresponding  to physical  domain (Output.Electrical,
Output.Structural...).  The  Output  object  is instantiated  by  the  Simulation  method run(). Each  Output
sub-object  (i.e.  physics) contains mostly the same data, organized in the same way whatever model was used to compute it.
Besides, each model can add its own dedicated output and post-processing if needed. It enables to use all the existing
post-processing on any new model, as long as all the  class templates  are  respected. PYLEECAN  includes options to
select  which  level  of output data should be saved in the Output object (for example, airgap flux waveform or only
fundamental  airgap  flux).

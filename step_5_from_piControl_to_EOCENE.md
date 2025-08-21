In this section, we try to repoduce the Eocene configuration from piControl configuration step by step.

My start point is the piControl suite **u-cm430**, and the destination is the final product run from Seb (**u-di625**).


## First step: Make sure the original suite can work with all standard boundary/initial conditions, before making it Eocene.
### Necessary sub-steps are shown below:
1. copy the u-cm430 from rosie, to get a new suite (for me it is u-ds179).
2. Edit this suite by `rose config-edit u-cm430`:  
  -   at `suite conf >> Project accounting` set the `Account group for HPC tasks` as `'n02-ncas'`  
  -   at `suite conf >> Machine Options` set the `USERNAME` as `Your HPC USERNAME`
  -   at `suite conf >> Machine Options` set the `Science Configuration Module Name` as `GC3-PrgEnv/v1`
  -   at `fcm_make_ocean >> env >> NEMO Source` set the `nemo_sources` as `/home/n02/n02/ros/nemo/branches/dev_r5518_GO6_package`
3. confirm the restart/initial file of UM (`ainitial at um >> file >> namelist >> recon_technical`), CICE and NEMO (NEMO start dump, NEMO icebergs start dump and CICE start dump at suite conf >> Run Initialisation and Cycling) exist.
4. Edit `host` under `$suite_DIR/site/archer2.rc [[HPC]][[remote]]` as `$(rose host-select archer2)`
5. Edit `host` under `$suite_DIR/suite.rc [runtime][[PPTRANSFER]][[[remote]]]` as `$(rose host-select archer2)`
6. Run the rose suite by `rose suite-run` at the directory of the suite.

### some tips:
- $UMDIR is set in `$suite_DIR/site/archer2.rc [[HPC]][[enviroment]]` 
- the initial/restart file of Unified model can be set by `ainitial` under `um >> file >> namelist >> recon_technical`


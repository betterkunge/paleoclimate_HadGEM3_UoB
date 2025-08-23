In this section, we try to repoduce the Eocene configuration from piControl configuration step by step.

My start point is the piControl suite **u-ch089**, and the destination is the final product run from Seb (**u-di625**).


## First step: Make sure the original suite can work with all standard boundary/initial conditions, before making it Eocene.
### Necessary sub-steps are shown below:
1. copy the u-ch089 from rosie, to get a new suite (for me it is u-ds203).
2. Edit this suite by `rose config-edit u-cm430`:  
  -   at `suite conf >> Project accounting` set the `Account group for HPC tasks` as `'n02-ncas'`  
  -   at `suite conf >> Machine Options` set the `USERNAME` as `Your HPC USERNAME`
  -   at `suite conf >> Machine Options` set the `Science Configuration Module Name` as `GC3-PrgEnv/v1`
  -   at `fcm_make_ocean >> env >> NEMO Source` set the `nemo_sources` as `/home/n02/n02/ros/nemo/branches/dev_r5518_GO6_package`
3. Edit `host` under `$suite_DIR/site/archer2.rc [[HPC]][[remote]]` as `$(rose host-select archer2)`
4. Edit `host` under `$suite_DIR/suite.rc [runtime][[PPTRANSFER]][[[remote]]]` as `$(rose host-select archer2)`
5. Edit `$suite_DIR/site/archer2.rc`:
  -   set `[[HPC]][[[remote]]]` as `$(rose host-select archer2)`
  -   set `[[EXTRACT_RESOURCE]][[[remote]]]` as `$(rose host-select {{ EXTRACT_HOST }})`
  -   at [[POSTPROC_RESOURCE]]pre-script delete:
      ```
      module load um
      module swap craype-network-ofi craype-network-ucx
      module swap cray-mpich cray-mpich-ucx
      ```
      add:
      ```
      module load cce/15.0.0
      module load atp
      ```
6. confirm the restart/initial file of UM (`ainitial at um >> file >> namelist >> recon_technical`), CICE and NEMO (`NEMO start dump`, `NEMO icebergs start dump` and `CICE start dump` at `suite conf >> Run Initialisation and Cycling`) exist
7. at `$suite_DIR/app/ocean_passive_tracers/rose-app.conf` set `meta` as `/home/n02/n02/ros/meta/ocean_passive_tracers`
8. at `$suite_DIR/app/fcm_make_pp/rose-app.conf` set `install_host` as `remote.cfg`
9. at `$suite_DIR/app/fcm_make_um/rose-app.conf`:  
   - change `config_root_path=/home/ros/um/branches/vn10.7_archer2_compile`
     by `config_root_path=fcm:um.xm-br/dev/simonwilson/vn10.7_archer2_compile`
   - add `branches/dev/jeffcole/vn10.7_archer2_fixes` after `um_sources=branches/pkg/Share/vn10.7_CMIP6_production_mods@43043` 

10. Run the rose suite by `rose suite-run` at the directory of the suite.

### some tips:
- $UMDIR is set in `$suite_DIR/site/archer2.rc [[HPC]][[enviroment]]` 
- the initial/restart file of Unified model can be set by `ainitial` under `um >> file >> namelist >> recon_technical`
- use `fcm ls fcm:um.xm_br/pkg/Share` to show the 


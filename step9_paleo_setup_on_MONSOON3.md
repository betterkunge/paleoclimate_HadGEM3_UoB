## Quick introduction
MONSOON3 is another HPC that supporting the PAST2FUTURE projects.

### Official documentation about MONSOON3:
[What is MONSOON](https://code.metoffice.gov.uk/doc/monsoon3/whatIsMonsoon.html)     
[The job scheduler (PBS;Portable Batch System) on monsoon](https://code.metoffice.gov.uk/doc/monsoon3/pbs.html#scheduling-work-with-pbs)    
[MASS and MOOSE](https://code.metoffice.gov.uk/doc/monsoon3/firstHourOnMass.html#your-first-hour-on-mass)    
[use quota to know your amount limitation](https://code.metoffice.gov.uk/doc/monsoon3/file_systems.html#filesystems-datadir-and-quotas)

## start-point GC5 suites on MONSOON3
There is a new suites u-dv308 (GC5-central N96 ORCA1 UM13.8 piControl), which is hosted by EXC and EXD. Here we make a copy of u-dv308 (u-dv344), then revise its rose configuration with the configuration of **u-du021***(MONSOON3 coupling suite) and **u-do322**(ARCGER2 GC5 suite) as a reference.     
Then we take the revised u-dv344 (GC5-central N96 ORCA1 UM13.8 piControl on monsoon3) as a start-point.
Note that there is some difference in the configuration of `instal ancil` task between GC3 and GC5.    

### The configuration of GC5
Note that there is some difference in the configuration of `instal ancil` task between GC3 and GC5.    
The NEMO and SI output variables can be changed through 



### Debug during the setup of u-dv344
#### Mismatch of calendar and outputstream
The default calendar for u-dv344 is **gregorian**, it is different with that for the GC3 Eocene and LGM suites.
For the gregorian calendar, all the time unit for reinitialisation of the outputstreams (reinit_unit) should be `Real Month(4)`. Or the outputstream with other units can not be generated.
Then you may get a error in the <your work directory>/cylc-run/u-dv344/runN/log/job/25001201T0000Z/coupled/NN/job.err as below:
```
????????????????????????????????????????????????????????????????????????????????
???!!!???!!!???!!!???!!!???!!!       ERROR        ???!!!???!!!???!!!???!!!???!!!
?  Error code: 1
?  Error from routine: io:file_open
?  Error message: Failed to open file
?  Error from processor: 0
?  Error number: 17
????????????????????????????????????????????????????????????????????????????????
```
#### ***mismatch between file_def_nemo_ice.xml and field_def_nemo_ice.xml***
 <your work directory>/cylc-run/u-dv344/runN/log/job/25001201T0000Z/coupled/NN/job.err as below:
```
> Error [CField::solveGridReference(void)] : In file '/home/users/harry.shepherd/cylc-run/infrastructure_suite_deploy_login.exa.sc/share/xios/src/node/field.cpp', line 1302 -> A grid must be defined for field 'sisnconc' .

MPICH ERROR [Rank 616] [job id e8932228-88a0-4a1c-8873-793d37b63163] [Fri Dec 12 23:04:41 2025] [nidd1411] - Abort(-1) (rank 616 in comm 0): application called MPI_Abort(MPI_COMM_WORLD, -1) - process 616
```

The `file_def_nemo_ice.xml` determines the list and frequencies of the variables to be outputed. The variables claimed in the `file_def_nemo_ice.xml` must be defined in the `field_def_nemo-oce.xml`, which is linked from `$CYLC_SUITE_SHARE_DIR/fcm_make_ocean/build-ocean/etc/field_def_nemo-oce.xml` as documented in the `./app/si3/rose-app.conf`.    
In our suite, we resolve this bug by replace our `domain_def_ice.xml` with that in u-do322, a GC5 suite owned by Xu. In the future, if the internal suite is fully ported to MONSOON3, we may not get this.

#### ***no sofwficb in NEMO ancil***   

 <your work directory>/cylc-run/u-dv344/runN/log/job/25001201T0000Z/coupled/NN/job.log as below:
 ```
          ===========

 iom_varid, file: ./runoff_1m_nomask.nc, var: sofwficb not found


  ===>>> : E R R O R

          ===========

 STOP
 No iceberg runoff data read in for Greenland. Check input file or set rn_greenland_calving_fraction=0.0


 huge E-R-R-O-R : immediate stop
```
This bug should be attributed to the Incomplete runoff ancillary file (/common/share/monsoon_ancils_ocean/hadgem3/forcing/ocean/eORCA1v2.2x/), which dont't have the file eORCA1_runoff_GO6_icb.nc which is needed here.     
eORCA1_runoff_GO6_icb.nc can be found on archer2 (/work/y07/shared/umshared/hadgem3/forcing/ocean/eORCA1v2.2x), so we port it from archer2 to monsoon3. **report the imcomplete file to the ncas people**

#### Explosion of the NEMO sea suraface height and sailinity
 <your work directory>/cylc-run/u-dv344/runN/log/job/25001201T0000Z/coupled/NN/job.log as below:
```
  ===>>> : E R R O R

          ===========


     ==>>>   nemo_gcm: a total of  1  errors have been found

             Look for "E R R O R" messages in all existing ocean_output* files

                     iom_close ~~~ close file: ./geothermal_heating.nc ok
                     iom_close ~~~ close file: ./runoff_1m_nomask.nc ok
                     iom_close ~~~ close file: ./runoff_1m_nomask.nc ok
[INFO] Nemo output file solver.stat not avaliable
[INFO] Ocean output from file run.stat
 it :       1    |ssh|_max:  0.2121747942899510E+01 |U|_max:  0.1916948210547619E+01 S_min:  0.3686317946397103E+00 S_max:  0.4341922647751776E+02
 it :       2    |ssh|_max:  0.2122421174448312E+01 |U|_max:  0.1918937899674541E+01 S_min:  0.3686122685981999E+00 S_max:  0.4341970512130442E+02
 it :       3    |ssh|_max:  0.2122247217306716E+01 |U|_max:  0.1918040254011808E+01 S_min:  0.3686296032653921E+00 S_max:  0.4342016674559835E+02
 it :       4    |ssh|_max:                     NaN |U|_max:  0.0000000000000000E+00 S_min:  0.1797693134862316+309 S_max: -0.1797693134862316+309
```
if you wanna check the output.abort_*.nc you may need to rebuild it. by the script `rebuild_nemo`. On MONSOON3 it is located at `/data/users/moci.mon/bin/REBUILD_NEMO/nemo_br_r10277_cpe2305_cce15`. For convenience, we copy it to the ~/bin/rebuild_nemo_zikun. A example for it: `rebuild_nemo_zikun output.abort 108`.

This error also shows in WORK/coupled/ocean.output, which is the log of NEMO. However, in this case this explosion seems to stem from the CFL question in the UM. It disappear after we uplift the ATMOS_TIME_STEPS_PER_DAY from 48 to 72.   

**note that** after increase ATMOS_TIME_STEPS_PER_DAY from 72 to 96 this error appear again. That may be caused by the influence of the restart file. The original ATMOS_TIME_STEPS_PER_DAY of u-do332, from which the restart file of our suite is taken from, is 72.


#### Explosion of the atmospheric windspeed 

## From piControl to Eocene on MONSOON3    
We copy the GC5-central piControl suite u-dv344 as a new suite u-dv769.    
There are three parts of parameters demand being changed:
- {path to your suite}/app/install_ancil/rose-app.conf
- 
### ancil_list
Based on the ancil_version scripts of Seb, I try to setup the Eocene ancil_files in u-dv769. However, there is some significant differences in NEMO ancil set bettween the GC3 and GC5 configurations: 
#### NEMO
##### Bathmetry and domain_CFG (configuration change)
In GC3 the bathmetry is controlled by the ancil_file `BATHY_METER`, while in GC5 it is included in the `domain_cfg.nc`. We may need to build a Eocene `domain_cfg.nc` based on the bathymetry file, so we can make it consistency.   
I am trying to generate it with the `DOMAINcfg` tools.    
Some repository for reference：
[A example for regional model](https://github.com/NOC-MSM/Regional-NEMO-Medusa)
See its [Wiki](https://github.com/NOC-MSM/Regional-NEMO-Medusa/wiki/) for usage.

My attemption:
1. [On Monsoon3] git clone https://github.com/NOC-MSM/Regional-NEMO-Medusa.git.
2. [On Archer2: under the work directory of di-622] rsync -avg /work/n02/n02/ssteinig/ancils/DeepMIP-Eocene/gc31/ancils_seb/ocean/eORCA_R1_bathy_meter_v2.0_eocene_chiselled.min30m.nc /work/y07/shared/umshared/hadgem3/grids/ocean/eORCA1v2.2x//eORCA1_coordinates_nc4.nc_from_MR clogin:/data/users/zikun.ren.ext/ancil/NEMO_tool/bathmetry_coordinate_from_di-622/
3. 

##### viscosity coefficient (configuration change)
In GC3 the viscosity coefficient in tropical area is modified by the `AHMCOEF` file, while in GC5 it is turned into a 3d distribution documented in `eddy_viscosity_3D.nc`. Therefore a new script should be build to generate the 3D distribution of viscosity which is adapted to Eocene mask.    
In the GC3 version, the viscosity file is generated by the script of Chalie (which is documented in the idiot's guidance). Refer to `/home/users/cwilliams2011/analysis.d/projects.d/sweet.d/gc31n96orca1_eo.d/build.d/ocean.d/ahmcoef.py.` on JASMIN for further detail.    
##### runoff or river (new ancil file)
In the GC5 model, except for the `runoff_1m_nomask.nc`, there is another file named `river_number_nemo.nc` required. I am not sure whether it is neccessary to make change on it.


#### UM
##### Chlorophyll qrclim.sea (potential element)
This is the same for piControl and eocene in the GC3 suite. In GC5, it is updated. Not sure whether it will significantly impact the results.
##### easyareosol (potential element)
There is some ancil seems to be fixed impcat on radiation of the areosol. Will it influence the model? Where to set it?
##### river_number_um (new ancil file)
For UM, there are two ancils requreid: `qrparm.rivseq` and `river_number_um`. The Eocene `qrparm.rivseq` have been applied in GC3, but no `river_number_um` yet.
##### qrclim.rivstor riverstorage (new ancil file)
Only for GC5. Switch it off in the configuration?
##### Ozone (potential element)
The dealing of ozone is different in the set of Charlie and Seb. In the Charlie's configuraation it is taken from UKESM1 4xCO2 monthly climatology (85 levels), and zonally palaeotise at every month/level. In contrast, in the Seb's suite, it is calculated by interactive ozone scheme.     
Note that, the ozone scheme is not correctly set in our GC5 suite. Here we simply set it as a PI zonal-mean field. In the future, examine the ozone output to check whether it is used (07/01/2026).    
##### Soil roughness (new ancil file)
There is one new ancil file in GC5 named soil_roughness
##### orog_radiation_parameters, orog_radiation_parameter, qrparm.orog_wavedrag, qrparm.ash (new ancil files)
Some GC5 ancils relevant to the paramterization of the orography. None of them are supplied in GC3. Swich them off?
##### qrclim.aerosols${LEVELS}.nc (new ancil files)
These ancils seem to document fixed areosols by different vertical levels. We may have need to switch the relevant parameterizations off.
##### hydrosheds qrparm.hydtop (new ancil files)
This ancil file domucment the Mean Topographic Index and STANDARD DEVN IN TOPOGRAPHIC INDEX, which influence the average wetness tendency of a grid cell and the sub-grid heterogeneity of soil moisture, respectively. They may be used in the JULES.

### Switch off some scheme based on information about present-day bathymetry (Copy from [Idiot's guide to setting up GC3.1 Eocene suite on NEXCS v3.docx](https://uob-my.sharepoint.com/:w:/g/personal/wb19586_bristol_ac_uk/EcIwjv71mMFDgCKJqzcjB_oBGmRss2O40IxQPvVG6BF4xw?e=vgAmjN)):
- Ocean ancils used by the standard model are listed below - first 4 are based on information about present-day bathymetry which can’t be replicated for the Eocene, so just switch off spatially varying parts of these schemes:
    - Tidal mixing (M2, K1 files): Switch it off tidal mixing scheme  (i.e. remove key_zdftmx in ~/roses/<SUITEID>/app/fcm_make_ocean/rose-app.conf)
    - Geothermal heating: Switch it off (i.e. set ln_trabbc= .false. in /home/d05/<USERNAME>/roses/<SUITEID>/app/nemo_cice/rose-app.conf)
    - Indonesian throughflow mixing: Switch it off (i.e. set ln_tmx_itf=.false. in /home/d05/<USERNAME>/roses/<SUITEID>/app/nemo_cice/rose-app.conf)
    - 2D bottom friction (bfr_coef.nc): Switch it off (i.e. set ln_bfr2d=.false. in /home/d05/<USERNAME>/roses/<SUITEID>/app/nemo_cice/rose-app.conf)
    - Viscosity (ahmcoef.nc) - need to make new file, but using Eocene mask.  Purpose of this file is to reduce viscosity between 20° N/S to increase equatorial current speed and vertical shear.  This can't be applied next to western boundaries for stability reasons, or to ensure the WBCs remained sensible.  So it needs to be 0 between +/- 20°, except for a 10° buffer to the east of any land mass, followed by a 5° ramp from 100 to 0.  To do this, need to take modern version (at /projects/ocean/hadgem3/ancil/ocean/eORCA1v2.2x) then, using this, create new version using code on JASMIN at /home/users/cwilliams2011/analysis.d/projects.d/sweet.d/gc31n96orca1_eo.d/build.d/ocean.d/ahmcoef.py.  Final version needs to be transferred to NEXCS, e.g. at /home/d05/cwilliams/gc31/final_ancils_round2/ocean
    - Iceberg calving & ice sheet basal melt flux need to be switched off (as no ice sheets).  To do this, set in /home/d05/<USERNAME>/roses/<SUITEID>/app/
nemo_cice/rose-app.conf:
        -	ln_icebergs=.false.
        -	nn_isf=0
        -	rn_antarctica_total_fw_flux=0
        -	rn_greenland_total_fw_flux=0
Note: Last two may not be necessary as first two should switch off fluxes and hence their magnitude is irrelevant, but just in case…
    - Postprocessing/archiving may fall over if it tries to archive iceberg files but none exist (because iceberg advection scheme is off), so set in /home/d05/<USERNAME>/roses/<SUITEID>/app/postproc/rose-app.conf:
        - archive_iceberg_trajectory=true
        - nemo_icebergs_rst=true





## MOOSE    

echo "moo projinfo --members --long project-ukesm" | qsub -q collabmass -j oe

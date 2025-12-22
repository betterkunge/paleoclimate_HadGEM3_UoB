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
This error also shows in WORK/coupled/ocean.output, which is the log of NEMO. However, in this case this explosion seems to stem from the CFL question in the UM. It disappear after we uplift the ATMOS_TIME_STEPS_PER_DAY from 48 to 72.


## Quick introduction
MONSOON3 is another HPC that supporting the PAST2FUTURE projects.

### Official documentation about MONSOON3:
[What is MONSOON](https://code.metoffice.gov.uk/doc/monsoon3/whatIsMonsoon.html)     
[The job scheduler (PBS;Portable Batch System) on monsoon](https://code.metoffice.gov.uk/doc/monsoon3/pbs.html#scheduling-work-with-pbs)    
[MASS and MOOSE](https://code.metoffice.gov.uk/doc/monsoon3/firstHourOnMass.html#your-first-hour-on-mass)    
[use quota to know your amount limitation](https://code.metoffice.gov.uk/doc/monsoon3/file_systems.html#filesystems-datadir-and-quotas)

## start-point GC5 suites on MONSOON3
There is a new suites u-dv308 (GC5-central N96 ORCA1 UM13.8 piControl), which is hosted by EXC and EXD. Here we have a copy of u-dv308 (u-dv344), then revised its rose configuration with the site configuration of u-du021.     
Then we take the revised u-dv344 (GC5-central N96 ORCA1 UM13.8 piControl on monsoon3) as a start-point.

Note that there is some difference in the configuration of `instal ancil` task
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
#### ***mismatch between CICE3 restart file and SI3***
 <your work directory>/cylc-run/u-dv344/runN/log/job/25001201T0000Z/coupled/NN/job.err as below:
```
> Error [CField::solveGridReference(void)] : In file '/home/users/harry.shepherd/cylc-run/infrastructure_suite_deploy_login.exa.sc/share/xios/src/node/field.cpp', line 1302 -> A grid must be defined for field 'sisnconc' .

MPICH ERROR [Rank 616] [job id e8932228-88a0-4a1c-8873-793d37b63163] [Fri Dec 12 23:04:41 2025] [nidd1411] - Abort(-1) (rank 616 in comm 0): application called MPI_Abort(MPI_COMM_WORLD, -1) - process 616
```
That may be caused by the mismatch of restart file and the `domain_def_ice.xml`. Our restart file is taken from the archer2 suite u-do322 supplied by Xu. Therefore, we referenced the `domain_def_ice.xml` of Xu. In the future, if the internal suite is fully ported to MONSOON3, this bug can be better fixed.

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
eORCA1_runoff_GO6_icb.nc can be found on archer2 (/work/y07/shared/umshared/hadgem3/forcing/ocean/eORCA1v2.2x), so we port it from archer2 to monsoon3. ***report the imcomplete file to the ncas people***



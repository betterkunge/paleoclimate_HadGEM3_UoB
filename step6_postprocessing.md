Through the above steps, we have successfully run this model for three years. The next issue to be focused on is how to set the correct post-processing.



$ROSE_DATAC： where to archive the outputs    
default:cylc_run/{your suite}/share/cycle/   

$ROSE_DATA： where to store all the outputs
default:/cylc-run/u-ds206/share/data    

$DATAM: Directory path relative to $ROSE_DATA.    
default:History_data

where to set: Suite conf > Output Paths
## STASH 
`postproc > Atmosphere > process_all_streams`
find the `STASHmaster` file for the detail of STASH:
The path of STASHmaster file: ~/cylc-run/{your suite}/app/um/file/STASHmaster

finish the [further exercise](https://ncas-cms.github.io/um-training/further-exercises-1.html)

[Unified Model Output Post Processing](https://code.metoffice.gov.uk/doc/um/vn13.9/papers/umdp_Y01.pdf)

### structure of UM Outputs

- UM outputs:
  - [file formats](https://code.metoffice.gov.uk/doc/um/vn13.9/papers/umdp_F03.pdf)
  - [Unified Model Output Post Processing](https://code.metoffice.gov.uk/doc/um/vn13.9/papers/umdp_Y01.pdf)
    - Marker files: ~/cylc-run/{your suite}/work/{cycle point}/coupled/*.done    
      Rose UM suites which are set up to launch subjobs when particular output is produced may ’poll’ for the existence of a .done file which indicates that a file has been closed.
      The UM may be configured to produce a zero-length file with the filename of the original file plus an extension of `.done` when **closing a writable file**. For the same files any pre-existing `.done` file will be deleted whenever the UM opens the file.
      Switches:
      ```
      namelist
      --> IO System Settings
          --> General IO Control
              --> io_external_control
      ```
    - Marker files: ~/cylc-run/{your suite}/work/{cycle point}/coupled/*.arch    
      The UM may be configured to produce zero length files with a .done or .arch file extension to indicate when particular files are finished with. These markers may then be used by post processing tasks such as archiving, housekeeping, external meaning procedures, or to launch dependent tasks within a Rose suite.
      The UM may be configured to produce a zero length file with the original filename plus an extension of .arch in the run directory (typically $CYLC_TASK_WORK_DIR), which will indicate that the UM will not need the file again in future cycles.
      Switches:
      ```
      namelist
      --> IO System Settings
          --> General IO Control
              --> l_postp
      ```
    - MOOSE: Met Office Archive Storage System: long-term data archive system of Met Office
    - Archiving and Deletion:
      ```
      postproc
      --> Atmosphere
          --> Archiving
              --> archive_switch and del_switch
      ```
      Selecting these items will trigger the appropriate options to control which files are to be archived and deleted, and the corresponding frequencies and offsets. The app can also be run in a ‘debug’ mode, in which a log file will be produced indicating which files would be archived and/or deleted, **but without performing the actions**. This can be triggered using
      ```
      postproc
      --> Atmosphere
          --> debug
      ```
  - [STASH (Storage Handling and Diagnostic System)](https://code.metoffice.gov.uk/doc/um/vn13.9/papers/umdp_Y01.pdf)
- NEMO outputs:

## Debugging instance
### number of fields exceeds reserved headers ###
for a error like this: find the solution at: [https://cms-helpdesk.ncas.ac.uk/t/cycle-point-for-restarting-suites/476/19]
???????????????????????????????????????????????????????????????????????????????? 
???!!!???!!!???!!!???!!!???!!! ERROR ???!!!???!!!???!!!???!!!???!!! 
? Error code: 4 
? Error from routine: STWORK 
? Error message: STWORK: Number of fields exceeds reserved headers for unit 28 
? Error from processor: 18 
? Error number: 17 
????????????????????????????????????????????????????????????????????????????????

Where to find the outputs from specific processor:
archer2:~/cylc-run/{your suite}/work/{the cycle}/atmos_main/pe_output/{your suite}.fort6.pe0000

### Validity Time Mismatch ###
```
[ERROR]  Validity time mismatch in file /work/n02/n02/an25872/cylc-run/u-ds215/share/data/History_Data/ds215a.ps1850son to be archived
        --> Expected [1850, 12, 1, 0, 0, 0] and got [1850, 9, 1]
```
See (https://cms-helpdesk.ncas.ac.uk/t/validity-time-mismatch/1773)

The key logic here is making your climate meaning settings consistent with your STASH setting.
check your stash stream by switching off `postproc > Atmosphere > process_all_streams` and `postproc > Atmosphere > process_all_streams`. Try to make them match with each other.




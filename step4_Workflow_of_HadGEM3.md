

## The workflow of HadGEM3

Every successfully operated simulation of HadGEM3 should be a well-organized cylc workflow combined with a bunch of interdependent tasks. These tasks are scheduled in sequence and grouped to archive different target.

### cycle point
The time cycle point is the biggest unit of the HadGEM3 workflow. And the cycle point is usuualy regulated by the model time (set at rose_suite.conf[jinja2:suite.rc] by CALENDAR).  Visit **step3.1** to know more detail. The workflow of HadGEM3 will push forward with the cycle containing modeling and post-process of initial output. That means the tasks can always be repeatedly submited and running during a workflow.

The frequency and the total number (the length of the modeling) of the cycle points can be set by `RESUB`(Cycling frequency in GUI), and `RUNLEN` （Total Run Length） at `suite conf >> Run Initialisation and cycle`, respectively.


### Tasks
A cycle point of HadGEM3 typically contains the `couled` task and the `postproc` tasks of different components. At the initial cycle point, there will be additional tasks (such as `install_ancil` and `fcm_make_*`) to build the executable program.    
Note that, All the tasks above is not neccessary in the workflow. They can be canceled in the schedule at ``suite conf >> Build and Run`

## [Jobs acountting](https://ncas-cms.github.io/um-training/running.html#um-standard-output)
For a suite in running, the tasks documented in a suite will be built into some specific jobs.
The `sacct` command displays accounting data for all jobs that are run on ARCHER2. sacct can be used to find out about the resources used by a job. For example; Nodes used, Length of time the job ran for, etc. This information is useful for working out how much resource your runs are using. You should have some idea of the resource requirements for your runs and how that relates to the annual CU budget for your project. Information on resource requirements is also needed when applying for time on the HPC.    
Let’s take a look at the resources used by your copy of u-cc519 run.
- Locate the SLURM Job Id for your run. This is a 6 digit number and can be found in the `job.status` file in the cylc task log directory. Look for the line `CYLC_BATCH_SYS_JOB_ID=` and take note of the number after the `=` sign.
Run the following command:
```
sacct --job=<slurm-job-id> --format="JobID,JobName,Elapsed,Timelimit,NNodes"
```
Where `<slurm-job-id>` is the number you just noted above. You should get output similar to the following:
```
ARCHER2-ex> sacct --job=204175 --format="JobID,JobName,Elapsed,Timelimit,NNodes"
       JobID    JobName    Elapsed  Timelimit   NNodes
------------ ---------- ---------- ---------- --------
204175       u-cc519.a+   00:00:23   00:20:00        1
204175.batch      batch   00:00:23                   1
204175.exte+     extern   00:00:23                   1
204175.0     um-atmos.+   00:00:14                   1
```
The important line is the first line.
- How much walltime did the run consume?
- How much time did you request for the task?
- How many CUs (Accounting Units) did the job cost?
1 node hour currently = 1 CU. See the ARCHER2 website for information about the CU.
There are many other fields that can be output for a job. For more information see the Man page (`man sacct`). You can see a list of all the fields that can be specified in the `--format` option by running sacct `--helpformat`.

## The Unified model
[Possible the newest user guide for UM](https://code.metoffice.gov.uk/doc/um/latest/um-training/index.html)

The Met Office Unified Model (UM) is the name given to the suite of numerical modelling software developed and used at the Met Office for forecast prediction across a range of timescales; from weather forecasting to climate change.

Prior to UM9.0 the UM had its own gui, the **UMUI**, which was able to control the configuration and submission of UM jobs. As of UM9.0 the UM is configured, built, jobs submitted and run by **Rose**. **Thus a prerequisite of this UM course is that trainees are familiar with Rose**. 

## [Unified Model Documentation Papers](https://code.metoffice.gov.uk/doc/um/latest/umdp.html)
The link above supplied many useful documents about the configuration of UM, such as:
- C02 Coupled Model Technical Overview
- C04 Storage Handling and Diagnostic System (STASH)

## [FCM](https://ncas-cms.github.io/um-training/fcm-tutorial.html#documenting-your-change)
If you want to know more about the fcm sources used in the rose suite, for instance:`fcm_make –> env –> sources`. You will need this page.

Subversion (SVN) is a centralized [version control system](https://svnbook.red-bean.com/en/1.8/svn.basic.version-control-basics.html) that records changes to files and directories, enabling collaboration, revision tracking, and recovery of earlier versions. It relies on a central repository that all users connect to.

[FCM (Flexible Configuration Management)](https://metomi.github.io/fcm/doc/user_guide/getting_started.html) is a toolset built on top of SVN, developed by the UK Met Office to simplify scientific software development. It provides an easier interface for creating and managing branches, merging changes, and includes a build system (fcm make) to compile large models like the Unified Model or NEMO. In short, SVN is the underlying version control system, while FCM adds extra tools and user-friendly features tailored for scientists and complex model workflows.

### Trac System
Trac system is where you can find the revision from you and others. It supplied convinient tool to compare different suites and codes.
All your revision commited for the suites or the fcm_resources can be viewed and found in the following two links.   
[Trac system for um](https://code.metoffice.gov.uk/trac/um)    
[Trac system for rose suites](https://code.metoffice.gov.uk/trac/roses-u/browser/)    

### Some useful command
- fcm ls fcm:um.x-br/
   List directory entries in the repository fcm:um.x-br
- fcm branch-info
  show the detail of present working copy
- fcm branch-list  fcm:um.x
  show your revision
- fcm branch-delete
  delete the working copy
  


## [Solving Common UM Problems](https://ncas-cms.github.io/um-training/solving-problems.html)

## [Useful Hints & Tips for running with Rose/Cylc on the Archer2](https://cms.ncas.ac.uk/rose-cylc-hints/#passing-arguments-to-fcm_make)



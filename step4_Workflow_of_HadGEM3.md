

## The workflow of HadGEM3

Every successfully operated simulation of HadGEM3 should be a well-organized cylc workflow combined with a bunch of interdependent tasks. These tasks are scheduled in sequence and grouped to archive different target.

### cycle point
The time cycle point is the biggest unit of the HadGEM3 workflow. And the cycle point is usuualy regulated by the model time (set at rose_suite.conf[jinja2:suite.rc] by CALENDAR).  Visit **step3.1** to know more detail. The workflow of HadGEM3 will push forward with the cycle containing modeling and post-process of initial output. That means the tasks can always be repeatedly submited and running during a workflow.

The frequency and the total number (the length of the modeling) of the cycle points can be set by `RESUB`(Cycling frequency in GUI), and `RUNLEN` （Total Run Length） at `suite conf >> Run Initialisation and cycle`, respectively.


### Tasks
A cycle point of HadGEM3 typically contains the `couled` task and the `postproc` tasks of different components. At the initial cycle point, there will be additional tasks (such as `install_ancil` and `fcm_make_*`) to build the executable program.    
Note that, All the tasks above is not neccessary in the workflow. They can be canceled in the schedule at ``suite conf >> Build and Run`



## The Unified model
[Possible the newest user guide for UM](https://code.metoffice.gov.uk/doc/um/latest/um-training/index.html)

The Met Office Unified Model (UM) is the name given to the suite of numerical modelling software developed and used at the Met Office for forecast prediction across a range of timescales; from weather forecasting to climate change.

Prior to UM9.0 the UM had its own gui, the **UMUI**, which was able to control the configuration and submission of UM jobs. As of UM9.0 the UM is configured, built, jobs submitted and run by **Rose**. Thus a prerequisite of this UM course is that trainees are familiar with **Rose**. 

## [Unified Model Documentation Papers](https://code.metoffice.gov.uk/doc/um/latest/umdp.html)
The link above supplied many useful documents about the configuration of UM, such as:
- C02 Coupled Model Technical Overview
- C04 Storage Handling and Diagnostic System (STASH)

## [FCM](https://ncas-cms.github.io/um-training/fcm-tutorial.html#documenting-your-change)
If you want to know more about the fcm sources used in the rose suite, for instance:`fcm_make –> env –> sources`. You will need this page.

## [Solving Common UM Problems](https://ncas-cms.github.io/um-training/solving-problems.html)



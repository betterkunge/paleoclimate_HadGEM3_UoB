### Issues on the schedule (update on 15/01/2026)
- (1) GC3 PI (on MONSOON3):
    - look for a existing suite as the starting point
      - **status**: waiting for the response of MONSOON3 community Ext.
- (2) GC3 LGM (on archer2):
    - The coupling weights are required so that we can run the LGM correctly.    
      - **status**: waiting for the responce from Metoffice
    - Zikun are trying to refine the domain decomposition and make it run more efficienctly.
      - **status**: 1152 cores are used for running on ARCHER2. (Speed: six model years per day)
      - **to do**: output more information by uplift the level of `$NLOGPRT`
- (3) GC5 PI:
    - The ozone post-processing bug
      - **status**: waiting for the response of NCAS helpdesk and the update of MetOffice
      - **to do**: ask the Monsoon Community Ext Group for help.
    - model explosion after five months' running
      - **status**: waiting for the response of NCAS helpdesk and the update of MetOffice
- (4) GC5 Eocene: The workflow to produce the ancillaries (in pariticular the missing files highlighted by Zikun)
    - The missing of ancillary files for GC5 Eocene.
      - **status**: waiting for Duncan's responce. Try to get the suites to generate the ancillary files.
      - **to do**: Try running the model without these ancils, and making some paleotization based on the PI version.
- (5) The "slab/nudged" ocean.
    -  find or develope a "slab/nudged" ocean component (Duncan)
      -  **status**: waiting for the update of Duncan.

### Done
- (1) GC5 PI:
    - The post-processing doesn't work
      - **status**: run successfully after switching off the postproc_integrity.

### Optional (can be engaged in when blocked in P2F)
#### HadGEM3-GC3 work
- Based on the GC3.1, tune the model (relevant to P2F).
- Apply the ITCZ energetics framework on the outputs of HadGEM3-GC3.(Eocene piControl)
- MONSOON?
- ENSO?
#### DeepMIP work
- Help Seb with APRP
- ITCZ analysis
#### HadCM3
- Contribute to Dan's paper
- Orbital -CO2 coupling  
  - Eocene
  - Pheebe Ross's Thesis
    - Phoebe PhD thesis tuning HadCM3 + orbital ocean circulation: (Eocene):    
      https://spiral.imperial.ac.uk/entities/publication/694a0346-7cea-4acd-afec-31fb330c8f84
  - New simulation
  - ENSO
  - MONSOON


### Issues on the schedule (update on 16/01/2026)
- (1) GC3 PI (on MONSOON3):
    - look for a existing suite as the starting point
      - **status**: waiting for the response of MONSOON3 community Ext.
- (2) GC3 LGM (on archer2):
    - The coupling weights are required so that we can run the LGM correctly.    
      - **status**: learning to generate the mesh file (a example for Eocene: /home/users/an25872/pmip4_vol2/users/ssteinig/gc31/ancils/Eocene/final_ancils_charlie_round2/ocean/bathy；    
      Kenji's resources for LGM:/home/users/an25872/pmip4_vol2/users/kizumi/HadGEM3_boundary_data/projects/peltier6g/N96_eORCA1_v2.2x)
      - **to do**: Zikun are going to generate mesh and pass it to Duncan as soon as possible.
    - Zikun is trying to refine the domain decomposition and make it run more efficienctly.
      - **status**: 1152 cores are used for running on ARCHER2. (Speed: six model years per day)
      - **to do**: output more information by uplift the level of `$NLOGPRT`
- (3) GC5 PI:
    - (MONSOON3 GC5-central) model explosion after five months' running
      - **done**: Zikun attempted to fix it (failed)
      - **status**: waiting for the response of NCAS helpdesk and the update of MetOffice
     
    - (ARCHER2 GC5-unknown version) try to restart u-do332
      - **status**: in processing
      - **to do**: use it as a start point of the Eocene suite setup.
- (4) GC5 Eocene: The workflow to produce the ancillaries (in pariticular the missing files highlighted by Zikun)
    - The missing of ancillary files for GC5 Eocene.
      - **status**: waiting for Duncan's responce. Try to get the suites to generate the ancillary files.
      - **to do**: Try running the model without these ancils, and making some paleotization based on the PI version.
- (5) The "slab/nudged" ocean.
    -  find or develope a "slab/nudged" ocean component (Duncan)
      - **status**: A nudget method are found by Duncan.
      - **to do**: do some tests on it.

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
#### PV application in paleoclimate?

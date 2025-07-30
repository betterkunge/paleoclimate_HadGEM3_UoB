[[_HadGEM3 MOC]]

# JASMIN (by CEDA: Centre for Environmental Data Analysis)
-  from their [website](https://www.ceda.ac.uk/services/jasmin/) : *"JASMIN is a globally unique data intensive supercomputer for environmental science. Over 160 science projects are currently supported, covering topics ranging from climate science and oceanography to air pollution, earthquake deformation and analysis of wildlife populations. JASMIN supports the data analysis requirements of the UK and European climate and earth system modelling community. It consists of multi-Petabyte fast storage co-located with data analysis computing facilities, with dedicated light paths to various key facilities and institutes within the community."*
-  we will transfer all model output from NEXCS/Monsoon to JASMIN (automatic post-processing that needs to be configured for each suite) to allow access/analysis/storage of large amounts of data
-  a very neat feature is the ability to use the [[JASMIN Notebook Service]] to interactively analyse the data in a web browser

### getting an account:

- instructions to get started are conveniently listed at https://help.jasmin.ac.uk/article/189-get-started-with-jasmin, but I think not all steps are essential. I did the following:
	1. [Create a JASMIN portal account](https://help.jasmin.ac.uk/article/4435-get-a-jasmin-account-portal). Don't specify a SSH key yet, we will generate one later on NEXCS/Monsoon.
	2. [Request ssh login access to JASMIN](https://help.jasmin.ac.uk/article/161-get-login-account)
	3. Apply for access to additional services and [group workspaces on JASMIN](https://help.jasmin.ac.uk/article/3801-apply-for-access-to-a-gws):  
		- **hpxfer** (use of high-performance data transfer services; don’t worry about supplying an IP address because NEXCS is whitelisted) 
		- **pmip4** (Paleoclimate Modelling Intercomparison Project PMIP4; access now managed by [[Charlie Williams]])
		- **nexcs** (workspace for interactions with Met Office NEXCS system; not sure if really necessary) 
		- **ncas_generic, nerctools, ncas-sci-M and ncas-climate** (analysis/post-processing tools)  
		- **nerctools** (gives access to NERC software tools.)

# JASMIN (by CEDA: Centre for Environmental Data Analysis)
-  from their [website](https://www.ceda.ac.uk/services/jasmin/) : *"JASMIN is a globally unique data intensive supercomputer for environmental science. Over 160 science projects are currently supported, covering topics ranging from climate science and oceanography to air pollution, earthquake deformation and analysis of wildlife populations. JASMIN supports the data analysis requirements of the UK and European climate and earth system modelling community. It consists of multi-Petabyte fast storage co-located with data analysis computing facilities, with dedicated light paths to various key facilities and institutes within the community."*
-  we will transfer all model output from NEXCS/Monsoon to JASMIN (automatic post-processing that needs to be configured for each suite) to allow access/analysis/storage of large amounts of data
-  a very neat feature is the ability to use the [[JASMIN Notebook Service]] to interactively analyse the data in a web browser

### getting an account:

- instructions to get started are conveniently listed at https://help.jasmin.ac.uk/article/189-get-started-with-jasmin, but I think not all steps are essential. I did the following:
	1. [Create a JASMIN portal account](https://help.jasmin.ac.uk/article/4435-get-a-jasmin-account-portal).
	2. [Request ssh login access to JASMIN](https://help.jasmin.ac.uk/article/161-get-login-account)
	3. Apply for access to additional services and [group workspaces on JASMIN](https://help.jasmin.ac.uk/article/3801-apply-for-access-to-a-gws):  
		- **hpxfer** (use of high-performance data transfer services; don’t worry about supplying an IP address because NEXCS is whitelisted) 
		- **pmip4** (Paleoclimate Modelling Intercomparison Project PMIP4; access now managed by [[Charlie Williams]])
		- **nexcs** (workspace for interactions with Met Office NEXCS system; not sure if really necessary) 
		- **ncas_generic, nerctools, ncas-sci-M and ncas-climate** (analysis/post-processing tools)  
		- **nerctools** (gives access to NERC software tools.)

### [Goup work space](https://help.jasmin.ac.uk/docs/short-term-project-storage/introduction-to-group-workspaces/)

[Manage a GWS](https://help.jasmin.ac.uk/docs/short-term-project-storage/managing-a-gws/#authorising-access-to-the-gws)
use `getent group | grep gws_past2future` to comfirm the list of users of GWS past2future.

### [sci server](https://help.jasmin.ac.uk/docs/interactive-computing/sci-servers/)
[How to connect to SCI servers](https://help.jasmin.ac.uk/docs/interactive-computing/login-servers/#connecting-to-a-sci-server-via-a-login-server)


### Use Globus to automatically transfer data from ARCHER2 to JASMIN ###
firstly, set a globus link as described below
(https://cms.ncas.ac.uk/unified-model/pptransfer-globus/)

secondly
to get through the task `fcm_make_pp_jasmin`.
copy you ssh-key of JASMIN on the PUMA2. 
and set a config file as in the page [login-servers](https://help.jasmin.ac.uk/docs/interactive-computing/login-servers/)
my config set:
```
Host login-*
  HostName %h.jasmin.ac.uk
  User an25872
  IdentityFile ~/.ssh/id_ecdsa_jasmin
  ForwardAgent yes

Host sci-vm-*
  HostName %h.jasmin.ac.uk
  ProxyJump login-02
  IdentityFile ~/.ssh/id_ecdsa_jasmin
```


### DEBUG ###
task:fcm_make_pp_jasmin in GC5
```
  1 [FAIL] sci-vm-01:cylc-run/u-ds758/run9/share/fcm_make_pp_jasmin: cannot create mirror ta    rget
  2 [FAIL] ssh -n -oBatchMode=yes sci-vm-01 pwd # rc=255
  3 [FAIL] ssh: Could not resolve hostname sci-vm-01: Name or service not known^M
  4
  5 [FAIL] fcm make -f /home4/home/n02-puma/an25872/cylc-run/u-ds758/run9/work/19780901T0000    Z/fcm_make_pp_jasmin/fcm-make.cfg -C /home/n02/n02/an25872/cylc-run/u-ds758/run9/share/f    cm_make_pp_jasmin -j 4 mirror.target=sci-vm-01:cylc-run/u-ds758/run9/share/fcm_make_pp_j    asmin 'mirror.prop{config-file.name}=2' # return-code=2
  6 2025-10-30T16:59:21Z CRITICAL - failed/ERR
```
see [A error about ssh to svi machine of JASMIN](https://cms-helpdesk.ncas.ac.uk/t/could-not-resolve-hostname-jasmin/427/3)



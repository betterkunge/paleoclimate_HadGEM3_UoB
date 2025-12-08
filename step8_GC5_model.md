### GC5 Configuration overview
[GC5-accessment](https://code.metoffice.gov.uk/trac/gmed/wiki/Assessment/GC5Assessment)

## A example suite
u-da412
u-ds758（copy）

From GC3 turn to GC5 on PUMA2
https://cms.ncas.ac.uk/cylc8/ 



## Some command transition
rose suite-run --local-install-only -> cylc install  
cylc run -> cylc vip   
cylc gui -> cylc tui  

use cylc config <flow-name> to generate a file like suite.rc.processed

see command at：
https://cylc.github.io/cylc-doc/stable/html/user-guide/cheat-sheet.html

## some set transition
default archive directory in gc3.1:
$ROSE_DATAC：cylc_run/{your suite}/share/cycle/

default archive directory in gc5:
$ROSE_DATAC：{your work directory}/ARCHIVE/{your suite}






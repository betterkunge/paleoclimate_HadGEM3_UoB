suite.rc.proceed:the final version

suite.rc 

jinja2 include the other rc in suite.rc.proceed.
{% include 'restart_runtime.rc' %}

{% include 'site/'+SITE+'.rc' %}

{% include 'ozone-redistribution.rc' %}

What is the meaning of inherit=None, otherone

use `sacctmgr list user withassoc where account=n02-ncas ` to check the user under the account n02-ncas

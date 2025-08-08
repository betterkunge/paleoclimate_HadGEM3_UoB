suite.rc.proceed:the final version

suite.rc 

jinja2 include the other rc in suite.rc.proceed.
{% include 'restart_runtime.rc' %}

{% include 'site/'+SITE+'.rc' %}

{% include 'ozone-redistribution.rc' %}

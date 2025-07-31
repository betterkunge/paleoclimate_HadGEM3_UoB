# **Background:**  
On the PUMA2, the rose and cylc have been well installed. By the **setup** proceeding in step2, we can directly invoke the rose and cylc. This description is just for a deeper understanding of the configuration and workflow of rose&cylc.
Throughout this tutorial we will refer to settings in the following format:
- file - will refer to a Rose configuration file.
- file|setting - will refer to a setting in a Rose configuration file.
- file[section] - will refer to a section in a Rose configuration file.
- file[section]setting - will refer to a setting in a section in a Rose configuration file.

## what is rose?
Rose is a toolkit for writing, editing and running application configurations.
Rose also contains other optional tools for:

- Version control.

- Suite discovery and management.

- Validating and transforming Rose configurations.

- Interfacing with Cylc.  
    
## Installation
Visit: [Installation of rose&cylc](https://metomi.github.io/rose/doc/html/tutorial/rose/index.html)
After installation, run the command below to ensure all required dependencies are installed:
```bash
rose check-software
```
(know more in https://metomi.github.io/rose/doc/html/api/command-reference.html#command-rose-check-software)
## Text Editor
## Rose Configuration
Rose configurations are **directories** containing a Rose configuration file （**rose-*.conf**） along with other optional assets which define behaviours such as:
- Execution.
- File installation.
- Environment variables.

Rose configurations may be used standalone or alternatively in combination with the Cylc workflow engine. There are two types of Rose configuration for use with Cylc:

- Rose application configuration
A runnable Rose configuration which executes a defined command.

- Rose suite configuration
A Rose configuration designed to run Cylc workflows. For instance it may be used to define Jinja2 variables for use in the flow.cylc file.

### Why use rose configuration
With Rose configurations the **inputs** and **environment** required for a particular purpose can be encapsulated in a simple **human-readable** configuration.
Configuration settings can have **metadata** associated with them which may be used for multiple purposes including automatic checking and transforming.
Rose configurations can be edited either using a text editor or with the **rose config-edit** (rose config-edit filename.conf) GUI which makes use of metadata for display and on-the-fly validation purposes.

### Rose Applications
A Rose application or “Rose app” is a runnable Rose configuration which executes a defined command.
Rose applications provide a convenient way to encapsulate all of this configuration, storing it all in one place to make it easier to handle and maintain.
the configuration of Rose app (application configuration) is a diectory containing a **rose-app.conf** file. 

### Rose Glossary
- rose config-edit
- rose app-run





## what is cylc?  
Cylc (pronounced silk) is a workflow engine, a system that automatically executes tasks according to schedules and dependencies.
visit:https://cylc.github.io/cylc-doc/stable/html/user-guide/introduction.html#what-is-cylc

# **Official version:**  


## **Background:**  
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
Rose configurations are **directories** containing a Rose configuration file （`rose-*.conf`） along with other optional assets which define behaviours such as:
- Execution.
- File installation.
- Environment variables.

Rose configurations may be used standalone or alternatively in combination with the Cylc workflow engine. There are two types of Rose configuration for use with Cylc:

- Rose application configuration
A runnable Rose configuration which executes a defined command.

- Rose suite configuration
A Rose configuration designed to run Cylc workflows. For instance it may be used to define Jinja2 variables for use in the `flow.cylc` file.

### Why use rose configuration
With Rose configurations the **inputs** and **environment** required for a particular purpose can be encapsulated in a simple **human-readable** configuration.
Configuration settings can have **metadata** associated with them which may be used for multiple purposes including automatic checking and transforming.    
Rose configurations can be edited either using a text editor or with the `rose config-edit` (rose config-edit filename.conf) GUI which makes use of metadata for display and on-the-fly validation purposes.    

### Rose Applications
A Rose application or “Rose app” is a runnable Rose configuration which executes a defined command.
Rose applications provide a convenient way to encapsulate all of this configuration, storing it all in one place to make it easier to handle and maintain.    
the configuration of Rose app (application configuration) is a diectory containing a `rose-app.conf` file.     
Try to reproduce the example on [Rose Applications tutorial](https://metomi.github.io/rose/doc/html/tutorial/rose/applications.html) for a deeper understanding.    

To run a Rose application, navigate to the directory containing the rose-app.conf file and run the following command：
```bash
rose app-run -q  # -q for quiet output
```
But the more recomanded way is to add a **run** folder then run the application in it:
```bash
mkdir run
cd run
rose app-run -q -C ../  # -C option to provide the path to the application
```
## Rose Metadata
Metadata can be used to provide information about settings in Rose configurations.

It is used for:
- Documenting settings.
- Performing automatic checking (e.g. type checking).
- Formatting the rose config-edit GUI.
Metadata can be used to ensure that configurations are valid before they are run and to assist those who edit the configurations.
### The Metadata Format
The metadata of a configuration is written in a  `rose-meta.conf` which uses the standard rose configuration format. This file can either be stored inside a Rose configuration in a meta/ directory, or elsewhere outside of the configuration.
The metadata for a setting is written in a section named `[section=setting]` where setting is the name of the `setting` and `section` is the section to which the setting belongs (left blank if the setting does not belong to a section).
For example, below is a rose-meta of a 'hello world' app:
```bash
[env=WORLD]
description=The name of the world to say hello to.
values=Mercury, Venus, Earth, Mars, Jupiter,
      =Saturn, Uranus, Neptune
```
This example gives the `WORLD` variable a title and a list of allowed `values`.
Try to reproduce the example on [Pratical about editting metadata](https://metomi.github.io/rose/doc/html/tutorial/rose/metadata.html) for a deeper understanding.    

### 


## Rose Glossary
- rose config-edit
- rose app-run
- rose metadata-check









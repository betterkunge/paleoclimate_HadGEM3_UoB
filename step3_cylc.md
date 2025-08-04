# Caveat
This note is for cylc 7. The newest version Cylc 8 differs from Cylc 7 in many ways: architecture, scheduling algorithm, security, UIs, working practices, and more. please refer to [Migration guide from cylc7 to cylc8] (https://cylc.github.io/cylc-doc/stable/html/7-to-8/index.html) for the detail.
## what is cylc?  
Cylc (pronounced silk) is a workflow engine, a system that automatically executes tasks according to schedules and dependencies.
For a detailed guidance, visit: https://cylc.github.io/cylc-doc/stable/html/user-guide/introduction.html#what-is-
For a quick start, visit: https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/index.html
## `suite.rc` format
We refer to a Cylc workflow as a **Cylc suite**. A Cylc suite is a directory containing a `suite.rc` file. This configuration file is where we define our workflow. The `suite.rc` file uses a nested INI-based format:
- Comments start with a `#` character.
- Settings are written as `key = value` pairs.
- Settings can be contained within sections.
- Sections are written inside square brackets i.e. `[section-name]`.
- Sections can be nested, by adding an extra square bracket with each level, so a sub--- section would be written `[[sub-section]]`, a sub-sub-section `[[[sub-sub-section]]]`, and so on.
```INI
# Comment
[section]
    key = value
    [[sub-section]]
        another-key = another-value  # Inline comment
        yet-another-key = """
A
Multi-line
String
```
Throughout this tutorial we will refer to settings in the following format:
- `[section]` - refers to the entire section.
- `[section]key` - refers to a setting within the section.
- `[section]key=value` - expresses the value of the setting.
- `[section][sub-section]another-key`. Note we only use one set of square brackets with nested sections.
- 
## Graphing
The graph of a suite refers to the graph strings contained within the [scheduling][dependencies] section. 

### [Graph Strings](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/scheduling/graphing.html#graph-strings)

In Cylc we consider workflows in terms of [tasks](https://metomi.github.io/rose/2019.01.8/html/glossary.html#term-task) and [dependencies](https://metomi.github.io/rose/2019.01.8/html/glossary.html#term-dependency). 
[Uploading Graphing — Rose Documentation 2019.01.8 documentation.html…]()

Task are represented as words and dependencies as arrows (`=>`), so the following text defines two tasks where `make_dough` is dependent on `purchase_ingredients`:
```INI
purchase_ingredients => make_dough
```
In a Cylc workflow this would mean that make_dough would only run when purchase_ingredients has succeeded. These dependencies can be chained together:
```INI
purchase_ingredients => make_dough => bake_bread => sell_bread
```
This line of text is referred to as a graph string. These graph strings can be combined to form more complex workflows:
```INI
purchase_ingredients => make_dough => bake_bread => sell_bread
pre_heat_oven => bake_bread
bake_bread => clean_oven
```
Graph strings can also contain “and” (`&`) and “or” (`|`) operators, for instance the following lines are equivalent to the ones just above:
purchase_ingredients => make_dough
pre_heat_oven & make_dough => bake_bread => sell_bread & clean_oven
Collectively these graph strings are referred to as a graph.

### [cylc graphs](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/scheduling/graphing.html#cylc-graphs)
In a Cylc suite the graph is stored under the [scheduling][dependencies]graph setting, i.e:
```INI
[scheduling]
    [[dependencies]]
        graph = """
            purchase_ingredients => make_dough
            pre_heat_oven & make_dough => bake_bread => sell_bread & clean_oven
        """
```
This is a minimal Cylc suite. We have not yet provided Cylc with the scripts or binaries to run for each task. This will be covered later in the [runtime tutorial](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/index.html#tutorial-runtime).
Cylc provides a GUI for visualising graphs. It is run on the command line using the `cylc graph <path>` command where the path `path` is to the `suite.rc` file you wish to visualise.

## Basic Cycling




### Glossary
- cylc graph <path>
used to display a diagram of a suite.rc.




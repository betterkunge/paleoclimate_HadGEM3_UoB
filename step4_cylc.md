# Caveat
This note is for cylc 7. The newest version Cylc 8 differs from Cylc 7 in many ways: architecture, scheduling algorithm, security, UIs, working practices, and more. please refer to [Migration guide from cylc7 to cylc8] (https://cylc.github.io/cylc-doc/stable/html/7-to-8/index.html) for the details.
## what is cylc?  
Cylc (pronounced silk) is a workflow engine, a system that automatically executes tasks according to schedules and dependencies.
visit:https://cylc.github.io/cylc-doc/stable/html/user-guide/introduction.html#what-is-
## cylc suite
We refer to a Cylc workflow as a **Cylc suite**. A Cylc suite is a directory containing a `suite.rc` file. This configuration file is where we define our workflow. The `suite.rc` file uses a nested INI-based format:
- Comments start with a `#` character.
- Settings are written as `key = value` pairs.
- Settings can be contained within sections.
- Sections are written inside square brackets i.e. `[section-name]`.
- Sections can be nested, by adding an extra square bracket with each level, so a sub--- section would be written `[[sub-section]]`, a sub-sub-section `[[[sub-sub-section]]]`, and so on.

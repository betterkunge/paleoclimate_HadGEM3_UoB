## [Introduction] (https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/introduction.html#introduction)
So far, we have been working with  `[schedule]` section. This is where the workflow is defined in terms of tasks and dependencies.  

In order to make the workflow runnable 
we must **associate tasks with scripts or binaries** to be executed when the task runs. 
This means working with the `[runtime]` section which determines what runs, as well as where and how it runs.

### [The script Setting](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/introduction.html#the-script-setting)
We tell Cylc what to execute when a task is run using the `script` setting. To avoid make this note a simple repetition of the rutorial, 
I will introduce the runtime sector and its configuration by the instance in the [pratical of the tutorial](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/introduction.html#admonition-3).
```INI
[cylc]
    UTC mode = True
[scheduling]
    initial cycle point = 2000-01-01T00Z
    final cycle point = 2000-01-01T06Z
    [[dependencies]]
        [[[T00/PT3H]]]
            graph = """
                get_observations_camborne => consolidate_observations
                get_observations_heathrow => consolidate_observations
                get_observations_shetland => consolidate_observations
                get_observations_belmullet => consolidate_observations
            """
        [[[T06/PT6H]]]
            graph = """
                consolidate_observations => forecast
                consolidate_observations[-PT3H] => forecast
                consolidate_observations[-PT6H] => forecast
                get_rainfall => forecast
                forecast => process_exeter
            """
        [[[T12/PT6H]]]
            graph = """
                forecast[-PT6H] => forecast
            """

[runtime]
    [[get_observations_belmullet]]
        script = get-observations

    [[get_observations_camborne]]
        script = get-observations

    [[get_observations_heathrow]]
        script = get-observations

    [[get_observations_shetland]]
        script = get-observations

    [[consolidate_observations]]
        script = sleep 3;

    [[forecast]]
        script = echo 'Running forecast.'; sleep 5

    [[get_rainfall]]
        script = cat >rainfall <<<'The rain in Spain falls mainly on the plain'

    [[process<site>]]
        script = """python3 -c "
            from random import choice
            print choice(['Windy', 'Rainy', 'Sunny', 'Snowy'])"
        """
```
In the runtime section of this `suite.rc`, the tasks claimed in Scheduling are associated by concrete commands by the defining the script. Among the runtime tasks, the `process<site>` is a special one, because there is no identical identical task can be found in scheduling.
The task `process<site>` in the `[runtime]` section is a **template task** — it's not a task that runs by itself. Instead, it's used to define common settings (like the script or environment) for a family of tasks that follow the same structure but vary by name.
In the `[scheduling]` section, we see an instance of this template used:
```ini
forecast => process_exeter
```
Here, `process_exeter` is a concrete task instance derived from the `process<site>` template. Cylc automatically recognizes that process_exeter matches the template and applies the corresponding runtime configuration from `process<site>`.



## Glossary
- `cylc validate <path/to/suite>`
a command which automatically checks for any obvious configuration issues
- `cylc run <name>`
run the suite
- `cylc reg <name> <path/to/suite>`
register a workflow suite before running it, not necessary.
- `cylc gscan`
Scans and lists all currently running suites for the current user.
- `cylc help all`
lists all the available command of cylc


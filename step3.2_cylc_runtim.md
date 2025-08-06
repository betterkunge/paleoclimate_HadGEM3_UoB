## [Introduction] (https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/introduction.html#introduction)
So far, we have been working with  `[schedule]` section. This is where the workflow is defined in terms of tasks and dependencies.  

In order to make the workflow runnable 
we must **associate tasks with scripts or binaries** to be executed when the task runs. 
This means working with the `[runtime]` section which determines what runs, as well as where and how it runs.

To avoid make this note a simple repetition of the rutorial, 
I will introduce the runtime sector and its configuration by the instance in the [pratical of the tutorial](https://metomi.github.io/rose/2019.01.8/html/tutorial/cylc/runtime/introduction.html#admonition-3).
### weather forecasting suite
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
```

## Glossary
- `cylc validate <path/to/suite>`
a command which automatically checks for any obvious configuration issues
- `cylc run <name>`
run the suite
- `cylc reg <name> <path/to/suite>`
register a workflow suite before running it, not necessary.

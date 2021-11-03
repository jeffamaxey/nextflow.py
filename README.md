# nextflow.py

![](https://github.com/goodwright/nextflow.py/actions/workflows/main.yml/badge.svg)
[![](https://img.shields.io/pypi/pyversions/nextflow.svg?color=3776AB&logo=python&logoColor=white)](https://www.python.org/)
[![](https://img.shields.io/pypi/l/nextflow.svg?color=blue)](https://github.com/goodwright/nextflow.py/blob/master/LICENSE)

nextflow.py is a Python wrapper around the Nextflow pipeline framework. It lets
you run Nextflow pipelines from Python code.

```python
import nextflow

pipeline = nextflow.Pipeline("main.nf")
execution = pipeline.run(params={"param1": "123"})
print(execution.status)
```

## Installation

nextflow.py is available through PyPI:

```bash
pip install nextflow
```

You must install the Nextflow executable itself separately: see the
[Nextflow Documentation](https://www.nextflow.io/docs/latest/getstarted.html#installation)
for help with this.

## Use

The starting point for any nextflow.py pipeline is the Pipeline object. This is
initialised with a path to the file in question, and, optionally, the location
of an accompanying config file and input schema (formatted acording to nf-core)
guidelines:

```python
pipeline1 = nextflow.Pipeline("pipelines/my-pipeline.nf")
pipeline2 = nextflow.Pipeline("main.nf", config="nextflow.config", schema="inputs.json")
```

These are executed with the `run()` method. Arguments are:

- `location` (default `"."`) - the directory the pipeline will be run from. This
is where the `work` directory will be created, and where the log file will be
written to.

- `params` (default `None`) - any inputs to be passed to the pipeline at runtime
as a Python dictionary.

- `profile` (default `None`) - any settings profiles to apply, as a list of
profile names.

```python
execution1 = pipeline1.run()
execution2 = pipeline2.run(location="./rundir", params={"param1": "123"}, profile=["docker", "test"])
```

Upon completion you get an `Execution` object back. This is a record of the
execution that just ran, and you can interrogate various properties from it:

- `id` - The unique ID of that run, generated by Nextflow.

- `datetime` - When the pipeline ran.

- `duration` - how long the execution took.

- `status` - the status Nextflow reports on completion.

- `command` - the command used to run the pipeline.

- `log` - the full text of the log file produced.

- `process_executions` - the associated `NextflowProcess` objects.

A `NextflowProcess` is the record of the execution of a specific process within
the overall Pipeline execution. The precise attributes are generated
automatically at runtime based on the output of `nextflow log`, but some useful
ones are:

- `process` - the process's name within Nextflow.

- `name` - the unique name for this process execution.

- `stdout` - the process's text output.

- `stderr` - the process's error text output.

You can get all of the descriptive data at once with the `fields` attribute.

## Changelog

### 0.1.2

*3rd November, 2021*

- Better handling of missing Nextflow executable.

### 0.1.1

*29th October, 2021*

- Renamed `nextflow_processes` to `process_executions`.

- Added quotes around paths to handle spaces in paths.

### 0.1

*18th October, 2021*

- Basic Pipeline object.

- Basic Execution object.

- Basic ProcessExecution object.
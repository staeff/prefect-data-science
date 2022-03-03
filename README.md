# Orchestrate a Data Science Project in Python With Prefect

Following https://towardsdatascience.com/orchestrate-a-data-science-project-in-python-with-prefect-e69c61a49074

Original code: https://github.com/khuyentran1401/Data-science

## Why should you care about having an optimizing and orchestrated data science workflow?

Non-linear workflows can have many advantages over linear workflows.

* Steps that do not depend on each other, can be run simultaneously (orchestration)
* Intermediate results might get lost in an linear workflow in case of errors later in the flow

## Prefect

Prefect is framework to build workflows. It makes it easy to build, run, and monitor data pipelines at scale.
Building a workflow with Prefect consists of this 3 steps: `create tasks`, `create a flow`, `add parameters`.

A `Task` is a discrete action in a Prefect `flow`.
Create a task by using the decorator `prefect.task` on the processing functions.

A `Flow` represents the entire workflow by managing the dependencies between `tasks`.
Create a `flow` by putting the code to run the processing functions inside the `with Flow(...)` context manager.

This example data science project uses an Iris dataset, that has been saved to `data/raw` in this project
The file `data_engineering.py` contains the functions to process your data.
The output of the workflow is saved to `data/processed`.

## Run as cmd

* with params

```sh
$ prefect run -p data_engineering.py --param test_data_ratio=0.2
```

* if you have many params, using a json file might be handy

```sh
$ prefect run -p data_engineering.py --param-file='params.json'
```

The above describes how to use Prefect locally. Below you find the integration of Prefect cloud to monitor a workflow in the cloud.

**This is still work in progress**

## Prefect Cloud - Monitor Your Workflow

You can monitor your workflow in [Prefect Cloud](https://docs.prefect.io/orchestration/getting-started/set-up.html#prefect-cloud-and-server).

```sh
$ prefect create project "Iris Project"
$ prefect agent local start
```

After these two steps, run the `data_engineering.py` with the line `flow.register(project_name="Iris Project")` enabled and
head over to prefect cloud in the browser.

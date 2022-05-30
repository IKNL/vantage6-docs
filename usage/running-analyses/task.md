---
description: In this section, you will learn how to create a task from a client.
---
# Creating a task

## Preliminaries

Here we assume that
- you have a Python session with an authenticated Client object, as created in [authentication](authentication.md)
- you already have the algorithm you want to run available as a container in a docker registry (see [here](https://vantage6.discourse.group/t/developing-a-new-algorithm/31) for more details on developing your own algorithm)
- the nodes are configured to look at the right database

In this manual, we'll use the averaging algorithm from `harbor2.vantage6.ai/demo/average`, so the second requirement is met. This container assumes a comma-seperated (*.csv) file as input, and will compute the average over one of the named columns. We'll assume the nodes in your collaboration have been configured to look at a comma-seperated database, i.e. their config contains something like

```
  databases:
      default: /path/to/my/example.csv
      my_other_database: /path/to/my/example2.csv
```

so that the third requirement is also met. As an end-user running the algorithm, you'll need to allign with the node owner about which database name is used for the database you are interested in. For more info on configuring the nodes, see [configuring the node](../running-the-node/configuration.md).

## Determining which collaboration / organizations to create a task for

First, you'll want to determine which collaboration to submit this task to, and which organizations from this collaboration you want to be involved in the analysis

```python
>>> client.collaboration.list(fields=['id', 'name', 'organizations])
[{'id': 1, 'name': 'example_collab1', 'organizations': [{'id': 2, 'link': '/api/organization/2', 'methods': ['GET', 'PATCH']}, {'id': 3, 'link': '/api/organization/3', 'methods': ['GET', 'PATCH']}, {'id': 4, 'link': '/api/organization/4', 'methods': ['GET', 'PATCH']}]}]
```

In this example, we see that the collaboration called 'example_collab1' has three organizations associated with it, of which the organization id's are `2`, `3` and `4`. To figure out the names of these organizations, we run:

```python
>>> client.organization.list(fields=['id', 'name'])
>>> client.organization.list(fields=['id', 'name'])
[{'id': 1, 'name': 'root'}, {'id': 2, 'name': 'example_org1'}, {'id': 3, 'name': 'example_org2'}, {'id': 4, 'name': 'example_org3'}]
```

i.e. this collaboration consists of the organizations `example_org1` (with id `2`), `example_org2` (with id `3`) and `example_org3` (with id `4`).

## Creating a task using a client that runs the master algorithm

Now, we have two options: create a task that will run the master algorithm, or create a task that will (only) run the RPC methods. Typically, the RPC methods only run the node local analysis (e.g. compute the averages per node), whereas the master algorithms performs aggregation of those results as well (e.g. starts the node local analyses and then also computes the overall average). First, let us create a task that runs the master algorithm of the `harbor2.vantage6.ai/demo/average` container

```python
input_ = {'method': 'master',
          'kwargs': {'column_name': 'age'},
          'master': True}

average_task = client.task.create(collaboration=1,
                                  organizations=[2,3],
                                  name="an-awesome-task",
                                  image="harbor2.vantage6.ai/demo/average",
                                  description='',
                                  input=input_,
                                  data_format='json')
```

Note that the 'kwargs' we specified in the `input_` are specific to this algorithm: this algorithm expects an argument `column_name` to be defined, and will compute the average over the column with that name. Furthermore, note that here we created a task for collaboration with id `1` (i.e. our `example_collab1`) and the organizations with id `2` and `3` (i.e. `example_org1` and `example_org2`). I.e. the algorithm need not necessarily be run on _all_ the organizations involved in the collaboration. Finally, note that `client.task.create()` has an optional argument called `database`. Suppose that we would have wanted to run this analysis on the database called `my_other_database` instead of the `default` database, we could have specified an additional `database = 'my_other_database'` argument. Check `help(client.task.create)` for more information.

## Creating a task using a client that runs the RPC algorithm

You might be interested to know output of the RPC algorithm (in this example: the averages for the 'age' column for each node). In that case, you can run only the RPC algorithm, ommitting the aggregation that the master algorithm will normally do:

```python
input_ = {'method': 'average_partial',
          'kwargs': {'column_name': 'age'},
          'master': False}

average_task = client.task.create(collaboration=1,
                                  organizations=[2,3],
                                  name="an-awesome-task",
                                  image="harbor2.vantage6.ai/demo/average",
                                  description='',
                                  input=input_,
                                  data_format='json')
```

## Inspecting the results

Of course, it will take a little while to run your algorithm. You can use the following code snippet to run a loop that checks the server every 3 seconds to see if the task has been completed:

```python
print("Waiting for results")
task_id = average_task['id']
task_info = client.task.get(task_id)
while not task_info.get("complete"):
    task_info = client.task.get(task_id, include_results=True)
    print("Waiting for results")
    time.sleep(3)

print("Results are ready!")
```

When the results are in, you can get the result_id from the task object:

```python
result_id = task_info['id']
```

and then retrieve the results
```python
result_info = client.result.list(task=result_id)
```

The number of results may be different depending on what you run, but for the master algorithm in this example, we can retrieve it using:
```python
>>> result_info['data'][0]['result']
{'average': 53.25}
```

while for the RPC algorithm, dispatched to two nodes, we can retreive it using
```python
>>> result_info['data'][0]['result']
{'sum': 253, 'count': 4}
>>> result_info['data'][1]['result']
{'sum': 173, 'count': 4}
```

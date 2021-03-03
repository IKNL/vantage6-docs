---
description: >-
  In this section the basic steps for creating an algorithm for horizontal
  partitioned data are explained.
---

# Creating a New Algorithm

It is assumed that it is mathematically possible to create a federated version of the algorithm you want to use. In the following sections we create a federated algorithm to compute the average of a distributed dataset. An overview of the steps that we are going though:

1. Mathematically decompose the model
2. Implement and test locally
3. Vantage6 algorithm wrapper
4. Dockerize and push to a registry

## 📝 The mathematical problem

We want to now the average of column **X** from a dataset **Q** which contains **n** samples. Dataset **Q** is **horizontally partitioned in dataset** $$A = [a_1, a_2 ... a_j] = [q_1, q_2 ... q_j]$$ ****and $$B = [b_{1}, b_{2} ... b_k] = [q_{j+1}, q_{j+2}...q_{n}]$$ **.** The average of dataset **Q** is computed as:

$$
Q_{mean} = \frac{1}{n} \sum \limits_{i=1}^{n} {q_i} = \frac{q_1 + q_2 + ... + q_n}{n}
$$

Now we would like to compute $$Q_{mean}$$ from dataset A and B. This could be computed as:

$$
Q_{mean} = \frac{(a_1+a_2+...+a_j) + (b_1+b_2+...+b_k)}{j+k} = \frac{\sum A + \sum B }{j+k}
$$

We both need to count the number of samples in each dataset and we need the total sum of each dataset. Then we can compute the global average of dataset **A** and **B.**

{% hint style="danger" %}
We cannot simply compute the average on each node and combine them, as this would be mathematically incorrect. This would only work in the case if dataset **A** and **B** contain the same number of samples.
{% endhint %}

## 👨💻 Implementation

{% hint style="warning" %}
In this examples we use python, however you are free to choose any language. The only requirements are: 1\) It needs to be able to create HTTP-requests, and 2\) and needs to be able to read and write to files.

However is you use a different language you are not able to use our wrapper. Reach us on [Discord](https://discord.gg/yAyFf6Y) to discuss how this works.
{% endhint %}

Now that we have figured out the maths, we can translate it to an implementation. A federated algorithm consist of two parts:

1. Central part of the algorithm which is responsible for combining the partial results from the data station. In our case that would be dividing the sum of the totals with the sum of observations.
2. Federated part of the algorithm which is responsible for the creating the partial results. In our case this would be computing the total \(=sum\) and number of observations.

{% hint style="info" %}
The central part of the algorithm can either be run on the machine of the researcher himself or in a master container which runs on a node, the latter is the preferred method.

In case the researcher runs this part himself he needs to have a proper setup to do so \(i.e. python 3.5+ and the necessary dependencies\). This is useful when developing new algorithms.
{% endhint %}

### 💕 Federated Part

The node that runs this part contains a CSV-file with one column **numbers** which we want to use to compute the global mean. We assume that this column has no NaN values.

```python
import pandas

def federated_part(path, column_name="numbers"):
    """Compute the sum and number of observations of a column"""

    # extract the column numbers from the CSV
    numbers = pandas.read_csv(path)[column_name]

    # compute the sum, and count number of rows
    local_sum = numbers.sum()
    local_count = len(numbers)

    # return the values as a dict
    return {
        "sum": local_sum,
        "count": local_count
    }
```

### ❤ The central algorithm

The central algorithm receives the sums and counts from all sites and combines these to a global mean. This could be from one or more sites.

```python
def central_part(node_outputs):
    """Combine the partial results to a global average"""
    global_sum = 0
    global_count = 0
    for output in node_outputs:
        global_sum += output["sum"]
        global_count += output["count"]

    return {"average": global_sum / global_count}
```

### 🧪 Local testing

To test simple create two datasets **A** and **B**, both having a numerical column **numbers**. Then run the following:

```python
outputs = [
    federated_part("path/to/dataset/A"),
    federated_part("path/to/dataset/B")
]
Q_average = central_part(outputs)["average"]
print(f"global average = {Q_average}.")
```

## 🌯 Algorithm wrapper

{% hint style="success" %}
A good starting point would be to use the boilerplate from our [Github](https://github.com/iknl/v6-boilerplate-py). This section gives background on all the steps needed to get to this boilerplate but also provides some background information.
{% endhint %}

Now that we have a federated implementation of our algorithm we need to incorporate it in the vantage6 infrastructure. The infrastructure handles the communication with the server and provides data access to the algorithm.

The algorithm consumes a file containing the input. This contains both the method name to be triggered as well as the arguments provided to the method. The algorithm has also access to a CSV file \(in the future this could also be a database\) on which the algorithm can run. Finally when the algorithm is finished it writes back the output to a different file.

The central part of the algorithm needs to be able to create \(sub\)tasks. These subtasks are responsible to execute the federated part of the algorithm. The central part of the algorithm can either be executed on the machine of the researcher or also on one of the nodes in the vantage6 network. In this example we only show the case in which one of the nodes executes the central part of the algorithm. The node provides the algorithm with a JWT token so that the central part of the algorithm has access to the server to post these subtasks.

{% hint style="info" %}
In this example the node uses a CSV-file as database 📔. There are implementations that use traditional databases and triple stores. We expect to support these use cases better in the future.
{% endhint %}

### 📂Package Structure

The algorithm need to be structured as a [package](https://packaging.python.org/tutorials/packaging-projects/). This way the algorithm can be installed within the Docker image. The minimal file-structure would be:

```bash
project_folder
├── Dockerfile
├── setup.py
└── algorithm_pkg
    └── __init__.py
```

We also recommend adding a `README.md`, `LICENSE` and `requirements.txt` to the project\_folder.

#### setup.py

Contains the setup method to create a package from your algorithm code. Here you specify some details about your package and the dependencies it requires.

```python
from os import path
from codecs import open
from setuptools import setup, find_packages

# we're using a README.md, if you do not have this in your folder, simply
# replace this with a string.
here = path.abspath(path.dirname(__file__))
with open(path.join(here, 'README.md'), encoding='utf-8') as f:
    long_description = f.read()

# Here you specify the meta-data of your package. The `name` argument is
# needed in some other steps.
setup(
    name='v6-average-py',
    version="1.0.0",
    description='vantage6 average',
    long_description=long_description,
    long_description_content_type='text/markdown',
    url='https://github.com/IKNL/v6-average-py',
    packages=find_packages(),
    python_requires='>=3.6',
    install_requires=[
        'vantage6-client',
        # list your dependancies here:
        # pandas, ...
    ]
)
```

{% hint style="info" %}
The `setup.py` above is sufficient in most cases. However if you want to do more advanced stuff \(like adding static data, or a CLI\) you can use the [extra arguments](https://packaging.python.org/guides/distributing-packages-using-setuptools/#setup-args) from `setup`.
{% endhint %}

#### 🐳 Dockerfile

Contains the recipe for building the Docker image. Typically you only have to change the argument `PKG_NAME` to the name of you package. This name should be the same as as the name you specified in the `setup.py`. In our case that would be `v6-average-py`.

```bash
# This specifies our base image. This base image contains some commonly used
# dependancies and an install from all vantage6 packages. You can specify a
# different image here (e.g. python:3). In that case it is important that
# `vantage6-client` is a dependancy of you project as this contains the wrapper
# we are using in this example.
FROM harbor.vantage6.ai/algorithms/algorithm-base

# Change this to the package name of your project. This needs to be the same
# as what you specified for the name in the `setup.py`.
ARG PKG_NAME="v6-average-py"

# This will install your algorithm into this image.
COPY . /app
RUN pip install /app

# This will run your algorithm when the Docker container is started. The
# wrapper takes care of the IO handling (communication between node and
# algorithm). You dont need to change anything here.
ENV PKG_NAME=${PKG_NAME}
CMD python -c "from vantage6.tools.docker_wrapper import docker_wrapper; docker_wrapper('${PKG_NAME}')"
```

#### `__init__.py`

This contains the code for your algorithm. It is possible to split this into multiple files, however the methods that should be available to the researcher should be in this file. You can do that by simply importing them into this file \(e.g. `from .average import my_nested_method`\)

We can distinguish two types of methods that a user can trigger:

| name | description | prefix | arguments |
| :--- | :--- | :--- | :--- |
| master | Central part of the algorithm. Recieves a `client` as argument which provides an interface to the central server. This way the master can create tasks and collect their results. |  | `(client, data, *args, **kwargs)` |
| Remote procedure call | Consumes the data at the node to compute the partial. | `RPC_` | `(data, *args, **kwargs)` |

{% hint style="info" %}
The `client` the master method receives is a `ContainerClient` which is different than the client you use as a user.
{% endhint %}

{% hint style="info" %}
Everything that is behind a `return` statement is send back to the central server. This should never contain any privacy sensitive information
{% endhint %}

For our average algorithm the implementation will look as follows:

```python
import time

from vantage6.tools.util import info

def master(client, data, column_name):
    """Combine partials to global model

    First we collect the parties that participate in the collaboration.
    Then we send a task to all the parties to compute their partial (the
    row count and the column sum). Then we wait for the results to be
    ready. Finally when the results are ready, we combine them to a
    global average.

    Note that the master method also receives the (local) data of the
    node. In most usecases this data argument is not used.

    The client, provided in the first argument, gives an interface to
    the central server. This is needed to create tasks (for the partial
    results) and collect their results later on. Note that this client
    is a different client than the client you use as a user.
    """

    # Info messages can help you when an algorithm crashes. These info
    # messages are stored in a log file which is send to the server when
    # either a task finished or crashes.
    info('Collecting participating organizations')

    # Collect all organization that participate in this collaboration.
    # These organizations will receive the task to compute the partial.
    organizations = client.get_organizations_in_my_collaboration()
    ids = [organization.get("id") for organization in organizations]

    # Request all participating parties to compute their partial. This
    # will create a new task at the central server for them to pick up.
    # We've used a kwarg but is is also possible to use `args`. Although
    # we prefer kwargs as it is clearer.
    info('Requesting partial computation')
    task = client.create_new_task(
        input_={
            'method': 'average_partial',
            'kwargs': {
                'column_name': column_name
            }
        },
        organization_ids=ids
    )

    # Now we need to wait untill all organizations(/nodes) finished
    # their partial. We do this by polling the server for results. It is
    # also possible to subscribe to a websocket channel to get status
    # updates.
    info("Waiting for resuls")
    task_id = task.get("id")
    task = client.get_task(task_id)
    while not task.get("complete"):
        task = client.get_task(task_id)
        info("Waiting for results")
        time.sleep(1)

    # Once we now the partials are complete, we can collect them.
    info("Obtaining results")
    results = client.get_results(task_id=task.get("id"))

    # Now we can combine the partials to a global average.
    global_sum = 0
    global_count = 0
    for result in results:
        global_sum += result["sum"]
        global_count += result["count"]

    return {"average": global_sum / global_count}

def RPC_average_partial(data, column_name):
    """Compute the average partial

    The data argument contains a pandas-dataframe containing the local
    data from the node.
    """

    # extract the column_name from the dataframe.
    info(f'Extracting column {column_name}')
    numbers = data[column_name]

    # compute the sum, and count number of rows
    info('Computing partials')
    local_sum = numbers.sum()
    local_count = len(numbers)

    # return the values as a dict
    return {
        "sum": local_sum,
        "count": local_count
    }
```

### 🏡 Local testing

Now that we have a vantage6 implementation of the algorithm it is time to test it. Before we run it into a vantage6 setup we can test it locally by using the `ClientMockProtocol` which simulates the communication with the central server.

Before we can locally test it we need to \(editable\) install the algorithm package so that the Mock client can use it. Simply move to the root directory of your algorithm package \(with the `setup.py` file\) and run the following:

```bash
pip install -e .
```

Then create a script to test the algorithm:

```python
from vantage6.tools.mock_client import ClientMockProtocol

# Initialize the mock server. The datasets simulate the local datasets from
# the node. In this case we have two parties having two different datasets:
# a.csv and b.csv. The module name needs to be the name of your algorithm
# package. This is the name you specified in `setup.py`, in our case that
# would be v6-average-py.
client = ClientMockProtocol(
    datasets=["local/a.csv", "local/b.csv"],
    module="v6-average-py"
)

# to inspect which organization are in your mock client, you can run the
# following
organizations = client.get_organizations_in_my_collaboration()
org_ids = ids = [organization["id"] for organization in organizations]

# we can either test a RPC method or the master method (which will trigger the
# RPC methods also). Lets start by triggering an RPC method and see if that
# works. Note that we do *not* specify the RPC_ prefix for the method! In this
# example we assume that both a.csv and b.csv contain a numerical column `age`.
average_partial_task = client.create_new_task(
    input_={
        'method':'average_partial',
        'kwargs': {
            'column_name': 'age'
        }
    },
    organization_ids=org_ids
)

# You can directly obtain the result (we dont have to wait for nodes to
# complete the tasks)
results = client.get_results(average_partial_task.get("id"))
print(results)

# To trigger the master method you also need to supply the `master`-flag
# to the input. Also note that we only supply the task to a single organization
# as we only want to execute the central part of the algorithm once. The master
# task takes care of the distribution to the other parties.
average_task = client.create_new_task(
    input_={
        'master': 1,
        'method':'master',
        'kwargs': {
            'column_name': 'age'
        }
    },
    organization_ids=[org_ids[0]]
)
results = client.get_results(average_task.get("id"))
print(results)
```

### 🏗 Building and 🚛 Distributing

Now that we have a full tested algorithm for the vantage6 infrastructure. We need to package it so that it can be distributed to the data-stations/nodes. Algorithms are delivered in Docker images. So that's where we need the `Dockerfile` for. To build an image from our algorithm \(make sure you have docker installed and it's running\) you can run the following command from the root directory of your algorithm project.

```bash
docker build -t harbor2.vantage6.ai/demo/average .
```

The option `-t` specifies the \(unique\) identifier used by the researcher to use this algorithm. Usually this includes the registry address \(harbor2.vantage6.ai\) and the project name \(demo\).

{% hint style="info" %}
In case you are using docker hub as registry, you do not have to specify the registry or project as these are set by default to the Docker hub and your docker hub username.
{% endhint %}

```bash
docker push harbor2.vantage6.ai/demo/average
```

{% hint style="info" %}
Reach out to us on [Discord](https://discord.gg/yAyFf6Y) if you want to use our registries \(harbor.vantage6.ai and harbor2.vantage6.ai\).
{% endhint %}

## 🤞 Cross-language serialization

It is possible that a vantage6 algorithm is developed in one programming language, but you would like to run the task from another language. For these kinds of usecases the python algorithm wrapper and client support cross-language serialization. By default input to the algorithms and output back to the client are serialized using pickle. However, it is possible to define a different serialization format.

Input and output serialization can be specified as follows:

```bash
client.post_task(
    name='mytask',
    image='harbor2.vantage6.ai/testing/v6-test-py',
    collaboration_id=COLLABORATION_ID,
    organization_ids=ORGANIZATION_IDS,
    data_format='json', # Specify input format to the algorithm
    input_={
        'method': 'column_names',
        'kwargs': {'data_format': 'json'}, # Specify output format
    }
)
```


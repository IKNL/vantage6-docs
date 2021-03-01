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

We want to now the average of column **X** from a dataset **Q** which contains **n** samples. Dataset **Q** is ****horizontally partitioned in dataset $$A = [a_1, a_2 ... a_j] = [q_1, q_2 ... q_j]$$ ****and $$B = [b_{1}, b_{2} ... b_k] = [q_{j+1}, q_{j+2}...q_{n}]$$ **.** The average of dataset **Q** is computed as:

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
In this example the node uses a CSV-file as database 📔.  There are implementations that use traditional databases and triple stores. We expect to support these use cases better in the future.
{% endhint %}

### Package Structure

The algorithm need to be structured as a [package](https://packaging.python.org/tutorials/packaging-projects/). This way the algorithm can be installed within the Docker image. The minimal file-structure would be:

```bash
project_folder
├── Dockerfile
├── setup.py
└── algorithm_pkg
    └── __init__.py
```

#### Dockerfile

Contains the rece

### IO files and variables

#### /app/input

The recommended format \(to keep maximum flexibility\) is a JSON file containing three keys: `method`, `args` and `kwargs` in which the `method` is the method name and the `args` and `kwargs` the input for this method in python-style. In case of the node algorithm in python:

```yaml
{
    "method": "node_algorithm",
    "args": [],
    "kwargs": {}
}
```

#### /app/output

This file contains the output of the method that was triggered in the Docker image. If possible, use JSON as the output format. 

#### Environment variables

There following environment variables are available to the algorithm:

* `DATABASE_URI` contains the path to the database file
* `HOST` contains the host name and protocol \(http/https\) of the central server
* `API_PATH` contains the api-path of the central server
* `PORT` contains the port to which the central server listens 

{% hint style="danger" %}
`HOST`, `PORT` and `API_PATH` are going to be changed in a future release of **Vantage** as the containers lose their internet connection. They will communicate by proxy to the central server.
{% endhint %}

### Algorithms

In the previous section we created the node- and central-algorithm. In order to read the input, write the output we need another part of code. This can be seen as the main entry point for both the algorithms. It should handle the following:

1. Read`/app/input.txt`, extract method, args and kwargs
   1. In case of the central algorithm the `/app/token.txt` needs to be read. This allows the central algorithm to post tasks back to the server. **Note: This is subject to change.** 
2. Execute method \(using parameters\) specified in `/app/input.txt`
3. Write output from the method to `/app/output.txt`

This main entry point script could be very similar for different algorithms. 

Then we need one final piece of code. The `master_algorithm` is responsible for creating \(node\_algorithm\) tasks at the server, and for retrieving the results of these tasks. After this has been done, the `central_algorithm` method can run to compute the global mean. The project should look as follows now:

{% tabs %}
{% tab title="main.py" %}
```python
import json
import os

from .average import node_algorithm, master_algorithm 

# read input from the mounted inputfile
info("Reading input")
with open("app/input.txt") as fp:
    input_ = json.loads(fp.read())


# determine function from input, summarize is used by default.
# and get the args and kwargs input for this function
method_name = input_.get("method","summary")
method = {
    "node": node_algorithm,
    "master": master_algorithm
}.get(method_name)
if not method:
    warn(f"method name={method_name} not found!\n")
    exit()

args = input_.get("args", [])
kwargs = input_.get("kwargs", {})

# call function
if method_name == "master":
    info("Reading token")
    # This is subjected to change in a future release (!)
    with open("app/token.txt") as fp:
        token = fp.read().strip()
        info(token)
        
    output = method(token, *args, **kwargs)
else: 
    output = method(*args, **kwargs)

# write output to mounted output file
info("Writing output")
with open("app/output.txt", 'w') as fp:
    fp.write(json.dumps(output))
```
{% endtab %}

{% tab title="average.py" %}
```python
import pandas

from vantage6.client import ContainerClient

def master_algorithm():
    # setup communication client, Note this is subjected to change
    client = ContainerClient(
        token=token, 
        host=os.environ["HOST"],
        port=os.environ["PORT"], 
        path=os.environ["API_PATH"]
    )
    
    # define inputs
    input_ = {"method": "node", "args": [], "kwargs": {}}
    
    # create tasks at the server
    task = client.create_new_task(input_)
    
    # poll for results
    task_id = task.get("id")
    task = client.request(f"task/{task_id}")
    while not task.get("complete"):
        task = client.request(f"task/{task_id}")
        time.sleep(1)
     # obtain and format results
    results = client.get_results(task_id=task.get("id"))
    results = [json.loads(result.get("result")) for result in results]
    
    # run and return central algorithm
    return central_algorithm(outputs)

def central_algorithm(node_outputs):
    global_sum = 0
    global_count = 0
    for output in node_outputs:
        global_sum += output["sum"]
        global_count += output["count"]
    
    return {"average": global_sum / global_count}
    
def node_algorithm(path, column_name="numbers"):
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
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note that if we obtain the results, from the dictonairy, we use the key **result** which we do not supply in the return values of the functions \(e.g. `node_algorithm`\). The node exapsulates the result from the algorithm to also contain some metadata. Therefore the actual results need to be retrieved from the _result_ key.
{% endhint %}

## Dockerize distributed algorithm

In this final step, the algorithm is dockerized and pushed to a registry from which the nodes can retrieve the algorithms. Add the following file to the project:

{% tabs %}
{% tab title="Dockerfile" %}
```text
# basic python3 image as base
FROM python:3

# install dependancies of the algorithm
COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# copy all local files to the image
COPY . /

# execute algorithm in the container
CMD ["python", "./main.py"]
```
{% endtab %}

{% tab title="requirements.txt" %}
```
pandas >= 0.24.0rc1
ppdli
```
{% endtab %}
{% endtabs %}

Now a docker-image can be created from our project using this docker-recipe, and we can push it to the registry.

```bash
docker build -t harbor2.vantage6.ai/project/dmean .
docker push harbor2.vantage6.ai/project/dmean 
```

## Cross-language serialization

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




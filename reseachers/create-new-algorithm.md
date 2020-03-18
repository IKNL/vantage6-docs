---
description: >-
  In this section the basic steps for creating an algorithm for horizontal
  partitioned data are explained.
---

# Create a New Algorithm

It is assumed that the algorithm that you want to use is mathematically possible for a separated dataset. In the following sections we use a simple average algorithm to explain the different steps to create an algorithm for Vantage. The following steps should be taken to create a Vantage ready algorithm:

1. Mathematically split the model
2. Implement and test locally using your preferred language
3. Standardize I/O
4. Dockerize and push to a registry 

## The mathematical problem

We want to now the average of column **X** from a dataset **Q** which contains **n** samples. Dataset **Q** is ****horizontally partitioned in dataset $$A = [a_1, a_2 ... a_j] = [q_1, q_2 ... q_j]$$ ****and $$B = [b_{1}, b_{2} ... b_k] = [q_{j+1}, q_{j+2}...q_{n}]$$ **.** The average of dataset **Q** is computed as:

$$
Q_{mean} = \frac{1}{n} \sum \limits_{i=1}^{n} {q_i} = \frac{q_1 + q_2 + ... + q_n}{n}
$$

Now we would like to compute $$Q_{mean}$$ from dataset A and B. This could be computed as:

$$
Q_{mean} = \frac{(a_1+a_2+...+a_j) + (b_1+b_2+...+b_k)}{j+k} = \frac{\sum A + \sum B }{j+k}
$$

Thus we need to count the number of samples in each dataset, and we need to the total sum of each dataset. Only then we can compute the global average of dataset **A** and **B.**

{% hint style="danger" %}
We cannot simply compute the average on each node and combine them, as this would be mathematically incorrect. This would only work in the case if dataset **A** and **B** contain the same number of samples.
{% endhint %}

## The algorithm

The algorithm consist of two separate parts: 1\) the central part of the algorithm, and 2\) the part that is executed at the node. In case of the average the nodes compute the sum and the count of samples in the dataset, the central part of the algorithm will combine these to a global average.

{% hint style="warning" %}
In these examples we use python, however you are free to choose any language. The only requirements are: 1\) It needs to be able to create HTTP-requests, and 2\) and needs to be able to read and write to files. 
{% endhint %}

{% hint style="info" %}
The central part of the algorithm can either be run on the machine of the researcher himself or in a master container which runs on a node, the latter is the preferred method.

In case the researcher runs this part himself he needs to have a proper setup to do so \(i.e. python 3.5+ and the necessary dependencies\). This is especially useful when developing new algorithms.
{% endhint %}

### The node algorithm

The node that runs this part contains a CSV-file with one column **numbers** which we want to use to compute the global mean. We assume that this column has no NaN values.

```python
import pandas

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

### The central algorithm

The central algorithm receives the sums and counts from all sites and combines these to a global mean.

```python
def central_algorithm(node_outputs):
    global_sum = 0
    global_count = 0
    for output in node_outputs:
        global_sum += output["sum"]
        global_count += output["count"]
    
    return {"average": global_sum / global_count}
        
```

### Local testing

To test simple create two datasets **A** and **B**, both having a numerical column **numbers**. Then run the following:

```python
outputs = [
    node_algorithm("path/to/dataset/A"),
    node_algorithm("path/to/dataset/B")
]
Q_average = central_algorithm(outputs)["average"]
print(f"global average = {Q_average}.")
```

## Standardizing IO

The algorithm receives parameter input in a txt-file, and also writes the output back to a txt-file. The database is also available as a path in the environment variables. In the case o

### IO files and variables

#### /app/input.txt

The recommended format \(to keep maximum flexibility\) is a JSON file containing three keys: `method`, `args` and `kwargs` in which the `method` is the method name and the `args` and `kwargs` the input for this method in python-style. In case of the node algorithm in python:

```yaml
{
    "method": "node_algorithm",
    "args": [],
    "kwargs": {}
}
```

#### /app/output.txt

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

from pytaskmanager.node.FlaskIO import ClientContainerProtocol

def master_algorithm():
    # setup communication client, Note this is subjected to change
    client = ClientContainerProtocol(
        token=token, 
        host=os.environ["HOST"],
        port=os.environ["PORT"], 
        path=os.environ["API_PATH"]
    )
    
    # define inputs
    input_ = {"method": "node","args": [],"kwargs": {}}
    
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
Note that if we obtain the results we use the key **result** which we do not supply in the return values of the functions \(e.g. `node_algorithm`\). This key is added by the node if the results are send back to the server. 
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
docker build -t docker-registry.distributedlearnin.ai/dmean .
docker push docker-registry.distributedlearnin.ai/dmean 
```


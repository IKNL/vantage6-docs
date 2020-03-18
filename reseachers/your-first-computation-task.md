---
description: >-
  In this section you will learn how to create a simple computation task and
  retrieve the results.
---

# Create Computation Task

It is assumed that you obtained a **username** and **password** for a Vantage server and that the organization to which you belong participates at least in one collaboration \(which could be a collaboration consisting only of your organization\). It is also assumed that the algorithm you want to execute is already available \(and allowed in this collaboration\). If you want to add your own algorithm please see [Create New Algorithm](create-new-algorithm.md).

The following steps are required:

1. Authenticate
2. Request new computation
3. Obtain the results

## Authentication

Before you can do any computations you need to authenticate to the server to obtain a JWT-token. This JWT-token can then be used to create new computation tasks and obtain the results when ready.

```python
url = "https://api.distributedlearning.ai"
api_path = ""


username = "john-doe"
password = "super-secret-password!"
```

{% api-method method="get" host="" path="/token/user" %}
{% api-method-summary %}
Token
{% endapi-method-summary %}

{% api-method-description %}
Obtain a user JWT-token using your **username** and **password** 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="username" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="password" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
Ok, authenticated
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
No or wrong body specified, or username/password combination unknown
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

To authenticate you can use the following REST endpoint \(todo: insert link to REST documentation\) `<server_url><api_path>/token/user` \(e.g. [https://api.distributedlearning.ai/token/user](https://api.distributedlearning.ai/token/user)\).

```python
import requests
import json

json_data = {
  "username": username,
  "password": password
}

# make an http-call to obtain a JWT-token
response = requests.post(
  f"{url}{api_path}/token/user", 
  json=json_data
)

# extract JWT access token
data = response.json()
access_token = data["access_token"]
refresh_token = data["refresh_token"]
```

Once the Bearer/JWT token \(`access_token`\) is obtained it is possible to create tasks \(or in more general access other API endpoints\). Tasks are always assigned to collaborations \(it is possible to assign tasks to specific organizations within a collaboration though\). It is mandatory that you, as a researcher, belong to an organization that is part of the collaboration to which you are posting the computation task.

## Permissions

You only can post tasks to **collaborations** in which the organization you are part participates in this collaboration.

{% api-method method="post" host="" path="/task" %}
{% api-method-summary %}
Task
{% endapi-method-summary %}

{% api-method-description %}
Create a new task for a specific collaboration.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="organization\_ids" type="string" required=false %}
Organizations \(that are within the collaboration\) that should execute this task. By default all organizations will execute the task
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
Human readable description
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Human readable name
{% endapi-method-parameter %}

{% api-method-parameter name="image" type="string" required=true %}
Docker image to use
{% endapi-method-parameter %}

{% api-method-parameter name="collaboration\_id" type="string" required=true %}
collaboration to which the task should be posted
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

An example would be:

```python
headers = {'Authorization': 'Bearer ' + access_token}

task_spec = {
  "description": "human readable description, not used by the code",
  "image": "docker-registry.distributedlearning.ai/docker-image-name",
  "input": "some input",
  "name": "human readable",
  "collaboration_id":2
}

task = requests.post(
  f"{url}{api_path}/task", 
  headers=headers,
  json=task_spec
)
```

When the computation task is created, the task is distributed to all the nodes \(if not specified by the `organization_ids`\) that belong to this collaboration.

## Organization and Nodes

Each organization can have multiple collaborations. For each collaboration a separate node instance is created. As a researcher you only have to specify the collaboration \(optionally you can specify specific organizations within the collaboration\) to which you want to post the task. So do not worry about nodes to much!

All the nodes from the organization execute the tasks \(if online, otherwise they will start the task when they come online\). Once they completed the task the results are returned to the central server where they can be collected.

## Obtain results



There are two ways in order to know when the results are available. The first method is polling for results at the `<server_url><api_path>/task/{id}/result` and the other method is listening to a websocket channel. The latter is the preferred way, although polling is easier to implement. In both cases you need to make a API call to obtain the results.

### Polling

 The polling method checks if the Boolean field `complete` is set to true, after which the results can be obtained. Make sure to pause between each poll to not overload the server with requests.

{% code title="" %}
```python
from time import sleep

task_id = task.get("id")
task = requests.get(
  f"{url}{api_path}/task/{task_id}", 
  headers=headers
)

while not task.get("complete"):
	task = requests.get(
      f"{url}{api_path}/task/{task_id}",
  	  headers=headers
    )
  sleep(1)
```
{% endcode %}

### Websocket

The websocket method subscribes to the collaboration channel to which status updates are posted.

```python
from socketIO_client import SocketIO, SocketIONamespace

class TasksNamespace(SocketIONamespace):
	"""Socket namespace"""
  
    def on_status_update(self, result_id):
        results = client.get_results(id=result_id)
        for result in results:
            node_id = result.get("node")
            print("-"*80)
            print(f"Results from node = {node_id}")
            print(result.get("result"))
    
socket = SocketIO(url, headers={
    "Authorization": f"Bearer {access_token}"
})


taskNamespace = socket.define(TasksNamespace, "/tasks")
taskNamespace.emit("join_room", f"collaboration_{collaboration_id}")
socket.wait()
```


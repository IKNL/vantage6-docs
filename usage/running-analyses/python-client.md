---
description: A Python client to interact with the vantage6 server
---

# Python client

## Initialization & authentication

```python
from vantage6.client import Client

# Configure the server details into the client
client = Client("http://localhost", 5000, "")

# Disable encryption, or supply the private key path the to 
# TODO: toggle between encryption or not
client.setup_encryption(None)

# Authenticate using a username and password
client.authenticate("root", "root")

# collect results from computation task
client.view_result(id=1)
# {
#   'assigned_at': '2021-01-14T13:25:44.333898+00:00',
#   'started_at': None,
#   'finished_at': None,
#   'log': 'logging from algorithm',
#   'result': None,
#   'input': None,
#   'id': 3,
#   'organization': {
#     'id': 4,
#     'link': '/organization/4',
#     'methods': ['GET', 'PATCH']
#   },
#   'task': {'id': 1, 'link': '/task/1', 'methods': ['DELETE', 'GET']}
# }

# you can also filter the output
client.view_result(id=1, fields=['result', 'log'])
# {
#   'result': ...
#   'log': ...
# }
```

## Authentication

```python

```

## Filtering of the output


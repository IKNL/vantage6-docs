---
description: How to authenticate a client with the vantage6 server
---

# Client authentication

First, we create a config file to store our login information. We do this so we do not have to define the `server_url`, `server_port` and so on every time we want to use the client. Moreover, it enables us to seperate the sensitive information (login details, organization key) that you do not want to make publicly available, from other parts of the code you might write later (e.g. on submitting particular tasks) that you might want to share publicly.


```python
# config.py

server_url = "https://MY VANTAGE6 SERVER"
server_port = 443 # This is specified when you first created the server
server_api = "" # This is specified when you first created the server

username = "MY USERNAME"
password = "MY PASSWORD"

organization_key = "FILEPATH TO MY PRIVATE KEY" # This can be empty if you do not want to set up encryption
```

Note that the `organization_key` should be a filepath that points to the private key that was generated when the organization to which your login belongs was first created (see [Creating an organization](organization.md)).

Then, we connect to the vantage 6 server by initializing a Client object, and authenticating
```python
from vantage6.client import Client
# Note: we assume here the config.py you just created is in the current directory.
# If it is not, then you need to make sure it can be found on your PYTHONPATH
import config

# Initialize the client object, and run the authentication
client = Client(config.server_url, config.server_port, config.server_api)
client.authenticate(config.username, config.password)

# Optional: setup the encryption, if you have an organization_key
client.setup_encryption(config.organization_key)
```

{% hint style="success" %}
If you are having trouble authenticating, we recommend adding `verbose=True` as additional argument when creating the Client(...) object. This will print much more information that can be used to debug the issue.&#x20;
{% endhint %}

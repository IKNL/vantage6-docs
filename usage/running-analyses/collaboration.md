---
description: In this section, you will learn how to use the client to create a new collaboration on the server.
---
# Creating a collaboration

Here, we assume that you have a Python session with an authenticated Client object, as created in [authentication](authenticate.md). We also assume that you have a login on the Vantage6 server that has administrative rights, otherwise, you will not be able to create a new collaboration.

A collaboration is an association of multiple [organizations](organization.md) that want to run analyses together. First, you will need to find the organization id's of the organizations you want to be part of the collaboration.

```python
client.organization.list(fields=['id', 'name'])
```

Once you know the id's of the organizations you want in the collaboration (e.g. 1 and 2), you can create the collaboration:

```python
collaboration_name = "fictional_collab"
organization_ids = [1,2] # the id's of the respective organizations
client.collaboration.create(name = collaboration_name, 
                            organizations = organization_ids, 
                            encrypted = True)
```

Note that a collaboration can require participating organizations to use encryptions, by passing the `encrypted = True` argument (as we did above) when creating the collaboration. It is recommended to do so, but requires that a keypair was created when [creating the organization](organization.md) and that each user of that organization has access to the private key so that they can run the `client.setup_encryption(...)` command after [authenticating](authentication.md).

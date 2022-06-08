---
description: In this section, you will learn how to use the client to register a new node with the server.
---
# Registering a node

Here, we assume that you have a Python session with an authenticated Client object, as created in [authentication](authentication.md). We also assume that you have a login on the Vantage6 server that has the permissions to create a new node (regular end-users typically do not have these permissions, this is typically only for administrators).

A node is assocated with both a collaboration and an organization (see [Concepts](../preliminaries.md#concepts)). You will need to find the collaboration and organization id's for the node you want to register:

```python
client.organization.list(fields=['id', 'name'])
client.collaboration.list(fields=['id', 'name'])
```

Then, we register a node with the desired organization and collaboration. In this example, we create a node for the organization with id 1 and collaboration with id 1.

```python
# A node is associated with both a collaboration and an organization
organization_id = 1
collaboration_id = 1
api_key = client.node.create(collaboration = collaboration_id, organization = organization_id)
print(f"Registered a node for collaboration with id {collaboration_id}, organization with id {organization_id}. The API key that was generated for this node was {api_key}")
```

Hang on to the `api_key` that is returned here, since you will need it when [configuring the node](../running-the-node/configuration.md).

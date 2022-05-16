---
description: Interacting with the server
---

# Client

The client interacts with the server. It can create computation tasks and collect their results, manage organizations, collaborations, users, etc. The server hosts an API which the client can use for this purpose. There is a client library available for [R](r-client.md) and [Python](python-client.md).&#x20;

For tutorials on how to use the clients, please visit our discourse pages: [https://vantage6.discourse.group/c/tutorials/](https://vantage6.discourse.group/c/tutorials/).

## API documentation

To see the API docs of your server visit:

{% swagger method="get" path="/apidocs" baseUrl="https://SERVER[/api_path]" summary="View the API documentation of the server you are using" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

For Petronas, the API docs can thus be found at [https://petronas.vantage6.ai/apidocs](https://petronas.vantage6.ai/apidocs).&#x20;

## Permissions

To access the API you need to be a user, node or container (algorithm). Nodes and containers have a fixed set of permissions at the server. Users can have rules assigned to them by another user.&#x20;

These rules can be assigned from the root user using the API. The root user which is created during initialization at the first run of the server has all available permissions.&#x20;

{% hint style="success" %}
It is not possible to assign rules to another user if you do not own the rule you are trying to assign.&#x20;
{% endhint %}

It is possible to collect rules into a single role so that you can assign them to multiple users. This approach allows you to implement your own roles and distribution of permissions within your organization.

The following roles are pre-defined, and are created when a new server is started for the first time.

| **Role**   | **Description**                                                             |
| ---------- | --------------------------------------------------------------------------- |
| root       | Superuser with all permissions, can be assigned only by a superuser itself. |
| node       | Role that all nodes have                                                    |
| containers | Role that all containers have                                               |

{% hint style="success" %}
An administrator can also assign rules and roles using the [shell](../running-the-server/shell.md#roles-and-rules).
{% endhint %}

## Client libraries

You can use any language to interact with the API of the server as long as it is able to create HTTP-requests. However, we do recommend using one of our client libraries, as this will greatly simplify the process of serializing and encrypting data. It also helps to find the parameters needed for each of the API endpoints.&#x20;

* [Python client](python-client.md)
* [R client](r-client.md)
* [Authentication](authentication.md)
* [Creating an organization](organization.md)
* [Creating a collaboration](collaboration.md)
* [Registering a node](node.md)

{% hint style="info" %}
The R client is only suitable for creating tasks and retrieve their results. With the Python client it is possible to use the entire API.
{% endhint %}

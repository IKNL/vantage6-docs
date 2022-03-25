---
description: These apply to all components
---

# Preliminaries

## Concepts

There are several entities in vantage6, such as users, organizations, tasks, etc. The following statements should help you understand the relationships.&#x20;

* A **collaboration** is a collection of one or more **organizations**.&#x20;
* For each collaboration, each participating organization needs a **node** to compute tasks.&#x20;
* Each organization can have **users** who can perform certain actions.&#x20;
* The permissions of the user are defined by the assigned **rules**.&#x20;
* It is possible to collect multiple rules into a **role**, which can also be assigned to a user.
* Users can create **tasks** for one or more organizations within a collaboration.
* A task should produce a **result** for each organization involved in the task.

The following schema is a _simplified_ version of the database:

![Simplified database model](<../.gitbook/assets/Artboard 1@4x (2).png>)

## End to end encryption

Encryption in vantage6 is handled at organization level. Whether encryption is used or not, is set at collaboration level. All the nodes in the collaboration need to agree on this setting. You can enable or disable encryption in the node configuration file, see [here](running-the-node/configuration.md#configuration-file-structure).

![Encryption takes place between organizations therefore all nodes and users from the a single organization should use the same private key.](<../.gitbook/assets/Artboard 1@4x (3).png>)

The encryption module encrypts data so that the server is unable to read communication between users and nodes. The only messages that go from one organization to another through the server are computation requests and their results. Only the algorithm input and output are encrypted. Other metadata (e.g. time started, finished, etc), can be read by the server.

The encryption module uses RSA keys. The public key is uploaded to the vantage6-server. Tasks and other users can use this public key (this is automatically handled by the python-client and R-client) to send messages to the other parties.&#x20;

{% hint style="success" %}
The RSA key is used to create a shared secret which is used for encryption and decryption of the payload
{% endhint %}

When the node starts, it checks that the public key stored at the server is derived from the local private key. If this is not the case, the node will replace the public key at the server.

{% hint style="danger" %}
If an organization has multiple nodes and/or users, they must use the same private key.&#x20;
{% endhint %}

In case you want to generate a new private key, you can use the command `vnode create-private-key`. If a key already exists at the local system, the existing key is reused (unless you use the `--force` flag). This way, it is easy to configure multiple nodes to use the same key.

It is also possible to generate the key yourself and upload it by using the following endpoint:

{% swagger method="patch" path="/organization/<ID>" baseUrl="https://SERVER[/api_path]" summary="Update the public key" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

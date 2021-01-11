---
description: In this section you will learn how to deal with the permission management system in vantage6.
---

# 🧻 Rules and Roles
Each user can have multiple roles and rules assigned to him. Roles consist of a set of rules. The final permissions the user has are a union of all the rules assigned to him either by direct assignment or through a role.

As an organization is it possible to define your own set of rules within your rule space. This means that you can not assign any rule that you do not own. This approach allows you to implement your own roles and distribution of permissions within your organization.

{% hint style="info" %}
Nodes and containers have a fixed role in the system which are created on the first run of the server instance.
{% endhint %}

# 📦 Predefined Roles
There are a few pre-defined roles.

| role | description |
| --- | --- |
| superuser | super user with all permissions, can be assigned only by a superuser itself.
| node | role that all nodes have
| containers | role that all containers have
| ??? |

# 🐕‍🦺 How to assign 'em
The server administrator can assign rules and roles through the [iPython shell](shell.md). Other users can use API to assign roles/rules that they own to other users.

{% hint style="info" %}
We are currently working to extend the server UI to also be able to manage permissions.
{% endhint %}

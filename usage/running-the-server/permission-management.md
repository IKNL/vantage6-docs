---
description: >-
  In this section you will learn how to deal with the permission management
  system in vantage6.
---

# Permission management

## 🧻 Rules and Roles

Each user can have multiple roles and rules assigned to him. Roles consist of a set of rules. The final permissions the user has are a union of all the rules assigned to him either by direct assignment or through a role.

As an organization is it possible to define your own set of rules within your rule space. This means that you can not assign any rule that you do not own. This approach allows you to implement your own roles and distribution of permissions within your organization.

{% hint style="info" %}
Nodes and containers have a fixed role in the system which are created on the first run of the server instance.
{% endhint %}

## 📦 Predefined Roles

There are a few pre-defined roles.

| role | description |
| :--- | :--- |
| superuser | super user with all permissions, can be assigned only by a superuser itself. |
| node | role that all nodes have |
| containers | role that all containers have |
| ??? |  |

## 🐕‍🦺 How to assign 'em

The server administrator can assign rules and roles through the [iPython shell](shell.md). Other users can use API to assign roles/rules that they own to other users.

First we need to initialize the client:

{% tabs %}
{% tab title="Python" %}
```python
from vantage6.client import Client

# setting the verbose option to True gives you a lot more
# information 
client = Client("https://harukas.vantage6.ai", 443, "", verbose=True)
# Welcome to
#                   _                     __
#                 | |                   / /
# __   ____ _ _ __ | |_ __ _  __ _  ___ / /_
# \ \ / / _` | '_ \| __/ _` |/ _` |/ _ \ '_ \
#  \ V / (_| | | | | || (_| | (_| |  __/ (_) |
#   \_/ \__,_|_| |_|\__\__,_|\__, |\___|\___/
#                             __/ |
#                            |___/
# 
#  --> Join us on Discord! https://discord.gg/rwRvwyK
#  --> Docs: https://docs.vantage6.ai
#  --> Blog: https://vantage6.ai
# ---------------------------------------------

# Authenticate using a username and password
client.authenticate("frank", "password")
# Authenticating using {'username': 'frank', 'password': 'password'}
# Successfully authenticated
# Making request: GET | https://harukas.vantage6.ai:443/user/1 | None
# Making request: GET | https://harukas.vantage6.ai:443/organization/1 | None
#  --> Succesfully authenticated
#  --> Name: frank (id=1)
#  --> Organization: root (id=1)

# Even if your collaboration is not using encryption
# you need to set this (This might change in a future
# release.
client.setup_encryption(None)

# Display all available rules, but only display the description and id
# the save some space
client.rule.list(fields=('id', 'description'))
# [{'id': 1, 'description': 'view any node'}, {'id': 2, 'description': 
# 'view your own node info'}, {'id': 3, 'description': 'edit any node'}, 
# {'id': 4, 'description': 'edit node that is part of your organization'}, 
# {'id': 5, 'description': 'create node for any organization'}, ... ]

# Display all roles. Roles consist of a set of rules. We only show the id
# name and assigned users
client.role.list(fields=('id', 'name', 'users'))
# [{'id': 1, 'name': 'container', 'users': None}, {'id': 2, 'name': 'node', 
# 'users': None}, {'id': 3, 'name': 'Root', 'users': [{'id': 1, 'link': 
# '/user/1', 'methods': ['GET', 'DELETE', 'PATCH']}]}, {'id': 4, 'name': 
# 'iknl-admin', 'users': [{'id': 4, 'link': '/user/4', 'methods': ['GET', 
# 'DELETE', 'PATCH']}]}, {'id': 5, 'name': 'jrc-admin', 'users': [{'id': 5, 
# 'link': '/user/5', 'methods': ['GET', 'DELETE', 'PATCH']}]}]
```
{% endtab %}
{% endtabs %}

To display rules and roles you can use the `rule` and `role` part of the client. Note that you can limit the keys of the output by specifying the `fields` argument.

{% tabs %}
{% tab title="Python" %}
```python

# Display all available rules, but only display the description and id
# the save some space
client.rule.list(fields=('id', 'description'))
# [{'id': 1, 'description': 'view any node'}, {'id': 2, 'description': 
# 'view your own node info'}, {'id': 3, 'description': 'edit any node'}, 
# {'id': 4, 'description': 'edit node that is part of your organization'}, 
# {'id': 5, 'description': 'create node for any organization'}, ... ]

# Display all roles. Roles consist of a set of rules. We only show the id
# name and assigned users
client.role.list(fields=('id', 'name', 'users'))
# [{'id': 1, 'name': 'container', 'users': None}, {'id': 2, 'name': 'node', 
# 'users': None}, {'id': 3, 'name': 'Root', 'users': [{'id': 1, 'link': 
# '/user/1', 'methods': ['GET', 'DELETE', 'PATCH']}]}, {'id': 4, 'name': 
# 'iknl-admin', 'users': [{'id': 4, 'link': '/user/4', 'methods': ['GET', 
# 'DELETE', 'PATCH']}]}, {'id': 5, 'name': 'jrc-admin', 'users': [{'id': 5, 
# 'link': '/user/5', 'methods': ['GET', 'DELETE', 'PATCH']}]}]
```
{% endtab %}
{% endtabs %}

To assign a role or rule to a user you need to update the user:

```python
frank = client.user.get(1)
print(frank)
# {'email': 'frank@grafikus.nl', 'last_seen': '2021-03-23T10:12:14.463590+00:00', 
# 'organization': {'id': 1, 'link': '/organization/1', 'methods': ['PATCH', 
# 'GET']}, 'username': 'root', 'lastname': 'Martin', 'id': 1, 'type': 'user', 
# 'ip': '172.17.21.1', 'roles': [{'id': 3, 'link': '/role/3', 'methods': ['GET', 
# 'DELETE', 'PATCH']}], 'firstname': 'frank', 'status': None, 'rules': None}

# It is always a good idea to check the documentation of the update function 
# first
help(client.user.update)
# Help on method update in module vantage6.client:

# update(id_: int = None, firstname: str = None, lastname: str = None, password: str = None, organization: int = None, rules: <function UserClient.User.list at 0x00000184E134F948> = None, roles: <function UserClient.User.list at 0x00000184E134F948> = None, email: str = None) -> dict method of vantage6.client.User instance
#     Update user details
# 
#    In case you do not supply a user_id, your user is being
#     updated.
# 
#     Parameters
#     ----------
#     user_id : int
#         User `id` from the user you want to update
#     firstname : str
#         Your first name
#     lastname : str
#         Your last name
#     password : str
#         The password you use to login
#     organization : int
#         Organization id of the organization you want to be part
#         of. This can only done by super-users.
#     rules : list of ints
#         USE WITH CAUTION! Rule ids that should be assigned to
#         this user. All previous assigned rules will be removed!
#     roles : list of ints
#         USE WITH CAUTION! Role ids that should be assigned to
#         this user. All previous assigned roles will be removed!
#     email : str
#         New email from the user
# 
#     Returns
#     -------
#     dict
#         A dict containing the updated user data

# So if we assign new roles or rules we need to becarefull not to remove previously 
# owned rules from this user.
already_owned_rules = [rule.get('id') for rule in frank['rules']] \
    if frank['rules'] else []
print(already_owned_rules)
# []

# assign rule 1 to frank
rules = already_owned_rules + [1]
client.user.update(user_id=frank.get('id'), rules=rules)
# Making request: PATCH | https://harukas.vantage6.ai:443/user/1 | None
# {'email': 'frank@grafikus.nl', 'last_seen': '2021-03-23T12:32:40.672706+00:00', 
# 'organization': {'id': 1, 'link': '/organization/1', 'methods': ['PATCH', 'GET']}, 
# 'username': 'root', 'lastname': 'Martin', 'id': 1, 'type': 'user', 'ip': 
# '172.17.21.1', 'roles': [{'id': 3, 'link': '/role/3', 'methods': ['GET', 'DELETE', 
# 'PATCH']}], 'firstname': 'frank', 'status': None, 'rules': [{'id': 1, 'link': 
# '/rule/1', 'methods': ['GET']}]}

# we can do a similar thing for the roles
already_owned_roles = [rule.get('id') for rule in frank['roles']] \
    if frank['roles'] else []
print(already_owned_roles)
# [3]

# assign role 4 to frank
roles = already_owned_roles + [4]
client.user.update(user_id=frank.get('id'), roles=roles)
# Making request: PATCH | https://harukas.vantage6.ai:443/user/1 | None
# {'email': 'frank@grafikus.nl', 'last_seen': '2021-03-23T12:31:10.660080+00:00', 
# 'organization': {'id': 1, 'link': '/organization/1', 'methods': ['PATCH', 'GET']}, 
# 'username': 'root', 'lastname': 'Martin', 'id': 1, 'type': 'user', 'ip': 
# '172.17.21.1', 'roles': [{'id': 3, 'link': '/role/3', 'methods': ['GET', 'DELETE', 
# 'PATCH']}, {'id': 4, 'link': '/role/4', 'methods': ['GET', 'DELETE', 'PATCH']}], 
# 'firstname': 'frank', 'status': None, 'rules': 'rules': [{'id': 1, 'link': 
# '/rule/1', 'methods': ['GET']}
```

{% hint style="info" %}
We are currently working to extend the server UI to also be able to manage permissions.
{% endhint %}


---
description: The Harukas release notes
---

# 2 Harukas

## 2.0.0

* **Feature**
  * Role/rule based access control
    * Roles consist of a bundle of rules. Rules profided access to certain API endpoints at the server.
    * By default 3 roles are created: 1\) Container, 2\) Node, 3\) Root. The root role is assigned to the root user on the first run. The root user can assign rules and roles from there.
  * Major update on the _python_-client. The client also contains management tools for the server \(i.e. to creating users, organizations and managing permissions. The client can be imported from `from vantage6.client import Client` . 
  * You can use the agrument `verbose` on the client to output status messages. This is usefull for example when working with Jupyter notebooks.
  * Added CLI `vserver version` , `vnode version` , `vserver-local version` and `vnode-local version` commands to report the version of the node or server they are running
  * The logging contains more information about the current setup, and refers to this documentation and our Discourd channel
  * 
*  **Bugfix**
  * Issue with the DB connection. Session management is updated. Error still occurs from time to time but can be reset by using the endpoint `/health/fix` . This will be patched in a newer version.


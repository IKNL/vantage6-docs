---
description: The Harukas release notes
---

# 2 Harukas

## 2.1.0

* **Feature**
  * Custom algorithm environment variables can be set using the `algorithm_env` key in the configuration file. [See this Github issue](https://github.com/IKNL/vantage6-node/issues/32).
  * Support for non-file-based databases on the node. [See this Github issue](https://github.com/IKNL/vantage6/issues/66). 
  * Added flag `--attach` to the `vserver start` and `vnode start` command. This directly attaches the log to the console.
  * Auto updating the node and server instance is now limited to the major version. [See this Github issue](https://github.com/IKNL/vantage6/issues/65).
    * e.g. if you've installed the Trolltunga version of the CLI you will always get the Trolltunga version of the node and server.
    * Infrastructure images are now tagged using their version major. \(e.g. `trolltunga` or `harukas` \)
    * It is still possible to use intermediate versions by specifying the `--image` option when starting the node or server. \(e.g. `vserver start --image harbor.vantage6.ai/infrastructure/server:2.0.0.post1` \)
* **Bugfix**
  * Fixed issue where node crashed if the database did not exist on startup. [See this Github issue](https://github.com/IKNL/vantage6/issues/67).

## 2.0.0.post1

* **Bugfix**
  * Fixed a bug that prevented the usage of secured registry algorithms

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


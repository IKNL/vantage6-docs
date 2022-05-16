# Release notes

## 3.2.0

* **Feature**
  * Horizontal scaling for the vantage6-server instance by adding support for RabbitMQ.
  * It is now possible to connect other docker containers to the private algorithm network. This enables you to attach services to the algorithm network using the `docker_services` setting.
  * Many additional select and filter options on API endpoints, see swagger docs endpoint (`/apidocs`). The new options have also been added to the Python client.
  * Users are now always able to view their own data
  * Usernames can be changed though the API
* **Bugfix**
  * (Confusing) SQL errors are no longer returned from the API.
  * Clearer error message when an organization has multiple nodes for a single collaboration.
  * Node no longer tries to connect to the VPN if it has no `vpn_subnet` setting in its configuration file.
  * Fix the VPN configuration file renewal
  * Superusers are no longer able to post tasks to collaborations its organization does not participate in. Note that superusers were never able to view the results of such tasks.
  * It is no longer possible to post tasks to organization which do not have a registered node attach to the collaboration.
  * The `vnode create-private-key` command no longer crashes if the ssh directory does not exist.
  * The client no longer logs the password
  * The version of the `alpine` docker image (that is used to set up algorithm runs with VPN) was fixed. This prevents that many versions of this image are downloaded by the node.
  * Improved reading of username and password from docker registry, which can be capitalized differently depending on the docker version.
  * Fix error with multiple-database feature, where default is now used if specific database is not found

## 3.1.0

* **Feature**
  * Algorithm-to-algorithm communication can now take place over multiple ports, which the algorithm developer can specify in the Dockerfile. Labels can be assigned to each port, facilitating communication over multiple channels.
  * Multi-database support for nodes. It is now also possible to assign multiple data sources to a single node in Petronas; this was already available in Harukas 2.2.0. The user can request a specific data source by supplying the _database_ argument when creating a task.&#x20;
  * The CLI commands `vserver new` and `vnode new` have been extended to facilitate configuration of the VPN server.
  * Filter options for the client have been extended.
  * Roles can no longer be used across organizations (except for roles in the default organization)
  * Added `vnode remove` command to uninstall a node. The command removes the resources attached to a node installation (configuration files, log files, docker volumes etc).
  * Added option to specify configuration file path when running `vnode create-private-key`.
* **Bugfix**
  * Fixed swagger docs
  * Improved error message if docker is not running when a node is started
  * Improved error message for `vserver version` and `vnode version` if no servers or nodes are running
  * Patching user failed if users had zero roles - this has been fixed.
  * Creating roles was not possible for a user who had permission to create roles only for their own organization - this has been corrected.

## 3.0.0&#x20;

* **Feature**
  * Direct algorithm-to-algorithm communication has been added. Via a VPN connection, algorithms can exchange information with one another.
  * Pagination is added. Metadata is provided in the headers by default. It is also possible to include them in the output body by supplying an additional parameter`include=metadata`. Parameters `page` and `per_page` can be used to paginate. The following endpoints are enabled:
    * GET `/result`
    * GET `/collaboration`
    * GET `/collaboration/{id}/organization`
    * GET `/collaboration/{id}/node`
    * GET `/collaboration/{id}/task`
    * GET `/organization`
    * GET `/role`
    * GET `/role/{id}/rule`
    * GET `/rule`
    * GET `/task`
    * GET `/task/{id}/result`
    * GET `/node`
  * API keys are encrypted in the database
  * Users cannot shrink their own permissions by accident
  * Give node permission to update public key
  * Dependency updates
* **Bugfix**
  * Fixed database connection issues
  * Don't allow users to be assigned to non-existing organizations by root
  * Fix node status when node is stopped and immediately started up
  * Check if node names are allowed docker names

## 2.3.0 - 2.3.4

* **Feature**&#x20;
  * Allows for horizontal scaling of the server instance by adding support for RabbitMQ. Note that this has not been released for version 3(!)
* **Bugfix**
  * Performance improvements on the `/organization` endpoint

## 2.2.0

* **Feature**
  * Multi-database support for nodes. It is now possible to assign multiple data sources to a single node. The user can request a specific data source by supplying the _database_ argument when creating a task.&#x20;
  * The mailserver now supports TLS and SSL options
* **Bugfix**
  * Nodes are now disconnected more gracefully. This fixes the issue that nodes appear offline while they are in fact online
  * Fixed a bug that prevented deleting a node from the collaboration
  * A role is now allowed to have zero rules
  * Some http error messages have improved
  * Organization fields can now be set to an empty string

## 2.1.2 and 2.1.3

* **Bugfix**
  * Changes to the way the application interacts with the database. Solves the issue of unexpected disconnects from the DB and thereby freezing the application.

## 2.1.1

* **Bugfix**
  * Updating the country field in an organization works again\\
  * The `client.result.list(...)` broke when it was not able to deserialize one of the in- or outputs.

## 2.1.0

* **Feature**
  * Custom algorithm environment variables can be set using the `algorithm_env` key in the configuration file. [See this Github issue](https://github.com/IKNL/vantage6-node/issues/32).
  * Support for non-file-based databases on the node. [See this Github issue](https://github.com/IKNL/vantage6/issues/66).&#x20;
  * Added flag `--attach` to the `vserver start` and `vnode start` command. This directly attaches the log to the console.
  * Auto updating the node and server instance is now limited to the major version. [See this Github issue](https://github.com/IKNL/vantage6/issues/65).
    * e.g. if you've installed the Trolltunga version of the CLI you will always get the Trolltunga version of the node and server.
    * Infrastructure images are now tagged using their version major. (e.g. `trolltunga` or `harukas` )
    * It is still possible to use intermediate versions by specifying the `--image` option when starting the node or server. (e.g. `vserver start --image harbor.vantage6.ai/infrastructure/server:2.0.0.post1` )
* **Bugfix**
  * Fixed issue where node crashed if the database did not exist on startup. [See this Github issue](https://github.com/IKNL/vantage6/issues/67).

## 2.0.0.post1

* **Bugfix**
  * Fixed a bug that prevented the usage of secured registry algorithms

## 2.0.0

* **Feature**
  * Role/rule based access control
    * Roles consist of a bundle of rules. Rules profided access to certain API endpoints at the server.
    * By default 3 roles are created: 1) Container, 2) Node, 3) Root. The root role is assigned to the root user on the first run. The root user can assign rules and roles from there.
  * Major update on the _python_-client. The client also contains management tools for the server (i.e. to creating users, organizations and managing permissions. The client can be imported from `from vantage6.client import Client` .&#x20;
  * You can use the agrument `verbose` on the client to output status messages. This is usefull for example when working with Jupyter notebooks.
  * Added CLI `vserver version` , `vnode version` , `vserver-local version` and `vnode-local version` commands to report the version of the node or server they are running
  * The logging contains more information about the current setup, and refers to this documentation and our Discourd channel
  *
* &#x20;**Bugfix**
  * Issue with the DB connection. Session management is updated. Error still occurs from time to time but can be reset by using the endpoint `/health/fix` . This will be patched in a newer version.

## 1.2.3

* **Feature**
  * The node is now compatible with the Harbor v2.0 API

## 1.2.2

* **Bug fixes**
  * Fixed a bug that ignored the `--system` flag from `vnode start`&#x20;
  * Logging output muted when the `--config`  option is used in `vnode start`&#x20;
  * Fixed config folder mounting point when the option `--config` option is used in `vnode start`&#x20;

## 1.2.1

* **Bug fixes**&#x20;
  * starting the server for the first time resulted in a crash as the root user was not supplied with an email address.
  * Algorithm containers could still access the internet through their host. This has been patched.

## 1.2.0

* **Features**
  * Cross language serialization. Enabling algorithm developers to write algorithms that are not language dependent.&#x20;
  * Reset password is added to the API. For this purpose two endpoints have been added: `/recover/lost`and `recover/reset` . The server config file needs to extended to be connected to a mail-server in order to make this work.
  * User table in the database is extended to contain an email address which is mandatory.
* **Bug fixes**
  * Collaboration name needs to be unique
  * &#x20;API consistency and bug fixes:
    * GET `organization` was missing domain key
    * PATCH `/organization` could not patch domain
    * GET `/collaboration/{id}/node` has been made consistent with `/node`
    * GET `/collaboration/{id}/organization` has been made consistent with `/organization`
    * PATCH `/user` root-user was not able to update users
    * DELETE `/user` root-user was not able to delete users
    * GET `/task` null values are now consistent: `[]` is replaced by `null`&#x20;
    * POST, PATCH, DELETE `/node` root-user was not able to perform these actions
    * GET `/node/{id}/task` output is made consistent with the&#x20;
* **other**
  * `questionairy` dependency is updated to 1.5.2
  * `vantage6-toolkit` repository has been merged with the `vantage6-client` as they were very tight coupled.

## 1.1.0

* **Features**
  * new command `vnode clean` to clean up temporary docker volumes that are no longer used&#x20;
  * Version of the individual packages are printed in the console on startup
  * Custom task and log directories can be set in the configuration file
  * Improved **CLI** messages
  * Docker images are only pulled if the remote version is newer. This applies both to the node/server image and the algorithm images
  * Client class names have been simplified (`UserClientProtocol` -> `Client`)
* **Bug fixes**
  * Removed defective websocket watchdog. There still might be disconnection issues from time to time.

## 1.0.0

* **Updated Command Line Interface (CLI)**
  * The commands `vnode list` , `vnode start` and the new command`vnode attach` are aimed to work with multiple nodes at a single machine.
  * System and user-directories can be used to store configurations by using the `--user/--system` options. The node stores them by default at user level, and the server at system level.
  * Current status (online/offline) of the nodes can be seen using `vnode list` , which also reports which environments are available per configuration.
  * Developer container has been added which can inject the container with the source. `vnode start --develop [source]`. Note that this Docker image needs to be build in advance from the `development.Dockerfile` and tag `devcon`.&#x20;
  * `vnode config_file` has been replaced by `vnode files` which not only outputs the config file location but also the database and log file location.
* **New database model**
  * Improved relations between models. Thereby updating the Python API, see [here](../usage/running-the-server/shell.md).
  * Input for the tasks is now stored in the result table. This was required as the input is encrypted individually for each organization (end-to-end encryption (E2EE) between organizations).
  * The `Organization` model has been extended with the `public_key` (String) field. This field contains the public key from each organization, which is used by the  E2EE module.
  * The `Collaboration` model has been extended with the `encrypted` (Boolean) field which keeps track if all messages (tasks, results) need to be E2EE for this specific collaboration.
  * The `Task` keeps track of the initiator (organization) of the organization. This is required to encrypt the results for the initiator.
* **End to end encryption**
  * All messages between all organizations are by default be encrypted.&#x20;
  * Each node requires the private key of the organization as it needs to be able to decrypt incoming messages. The private key should be specified in the configuration file using the `private_key` label.&#x20;
  * In case no private key is specified, the node generates a new key an uploads the public key to the server.&#x20;
  * If a node starts (using `vnode start`), it always checks if the `public_key` on the server matches the private key the node is currently using.
  * In case your organization has multiple nodes running they should all point to the same private key.
  * Users have to encrypt the input and decrypt the output, which can be simplified by using our client `vantage6.client.Client` __ for Python __ or `vtg::Client` __ for R.
  * Algorithms are not concerned about encryption as this is handled at node level.
* **Algorithm container isolation**
  * Containers have no longer an internet connection, but are connected to a private docker network.
  * Master containers can access the central server through a local proxy server which is both connected to the private docker network as the outside world. This proxy server also takes care of the encryption of the messages from the algorithms for the intended receiving organization.
  * In case a single machine hosts multiple nodes, each node is attached to its own private Docker network.
* **Temporary Volumes**
  * Each algorithm mounts temporary volume, which is linked to the node and the `run_id` of the task
  * The mounting target is specified in an environment variable `TEMPORARY_FOLDER`. The algorithm can write anything to this directory.
  * These volumes need to be cleaned manually. (`docker rm VOLUME_NAME`)&#x20;
  * Successive algorithms only have access to the volume if they share the same `run_id` . Each time a _**user**_ creates a task, a new `run_id` is issued. If you need to share information between containers, you need to do this through a master container. If a master container creates a task, all slave tasks will obtain the same `run_id`.
* **RESTful API**
  * All RESTful API output is HATEOS formatted. **(**[**wiki**](https://en.wikipedia.org/wiki/HATEOAS)**)**
* **Local Proxy Server**
  * Algorithm containers no longer receive an internet connection. They can only communicate with the central server through a local proxy service.&#x20;
  * It handles encryption for certain endpoints (i.e. `/task`, the input or `/result` the results)
* **Dockerized the Node**
  * All node code is run from a Docker container. Build versions can be found at our Docker repository: `harbor.distributedlearning.ai/infrastructure/node` . Specific version can be pulled using tags.
  * For each running node, a Docker volume is created in which the data, input and output is stored. The name of the Docker volume is: `vantage-NODE_NAME-vol` . This volume is shared with all incoming algorithm containers.
  * Each node is attached to the public network and a private network: `vantage-NODE_NAME-net`.




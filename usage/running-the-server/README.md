---
description: >-
  The server manages users, organizations, collaborations, tasks and results. In
  this section we will explain how to configure and manage a server.
---

# Server

It is assumed that you successfully installed [vantage6-server](../../installation/server/). To verify this, you can run the command `vserver --help` . If that prints a list of commands, your installation is successful. Also, make sure that Docker is running.

### Quick start

To create a new server, run the command below. A menu will be started that allows you to set up a server configuration file. For more details, check out the [server-configuration.md](server-configuration.md "mention") page.

```
vnode new
```

To run a server, execute the command below. The `--attach` flag will cause log output to be printed to the console.

```
vserver start --name <your_server> --attach
```

Finally, a server can be stopped again with:

```
vserver stop --name <your_server>
```

### Available commands

The following commands are available in your environment. To see all the options that are available per command use the `--help` flag, e.g. `vserver start --help`.

| **Command**       | **Description**                                                |
| ----------------- | -------------------------------------------------------------- |
| `vserver new`     | Create a new server configuration file                         |
| `vserver start`   | Start a server                                                 |
| `vserver stop`    | Stop a server                                                  |
| `vserver files`   | List the files that a server is using                          |
| `vserver attach`  | Show a server's logs in the current terminal                   |
| `vserver list`    | List the available server instances                            |
| `vserver shell`   | Run a server instance python shell                             |
| `vserver import`  | Import server entities as a batch                              |
| `vserver version` | Shows the versions of all the components of the running server |

The following sections explain how to use these commands to configure and maintain a vantage6-server instance:

* [server-configuration.md](server-configuration.md "mention")
* [importing-entities.md](importing-entities.md "mention")
* [shell.md](shell.md "mention")
* [deployment.md](deployment.md "mention")
* [logging.md](logging.md "mention")


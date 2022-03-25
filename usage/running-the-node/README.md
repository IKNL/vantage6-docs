---
description: The node runs algorithms requested by clients
---

# Node

It is assumed you have successfully installed [vantage6-node](./). To verify this you can run the command `vnode --help`. If that prints a list of commands, the installation is completed. Also, make sure that Docker is running.

{% hint style="success" %}
An organization runs a node for each of the collaborations it participates in
{% endhint %}

### Quick start

To create a new node, run the command below. A menu will be started that allows you to set up a node configuration file. For more details, check out the [configuration.md](configuration.md "mention") page.

```
vnode new
```

To run a node, execute the command below. The `--attach` flag will cause log output to be printed to the console.

```
vnode start --name <your_node> --attach
```

Finally, a node can be stopped again with:

```
vnode stop --name <your_node>
```

### Available commands

Below is a list of all commands you can run for your node(s). To see all available options per command use the `--help` flag, i.e. `vnode start --help` .

| **Command**                | **Description**                                          |
| -------------------------- | -------------------------------------------------------- |
| `vnode new`                | Create a new node configuration file                     |
| `vnode start`              | Start a node                                             |
| `vnode stop`               | Stop one or all nodes                                    |
| `vnode files`              | List the files of a node                                 |
| `vnode attach`             | Print the node logs to the console                       |
| `vnode list`               | List all available nodes                                 |
| `vnode create-private-key` | Create and upload a new public key for your organization |

See the following sections on how to configure and maintain a vantage6-node instance:

* [configuration.md](configuration.md "mention")
* [security.md](security.md "mention")
* [logging.md](logging.md "mention")




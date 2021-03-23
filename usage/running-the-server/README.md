---
description: >-
  The server manages users, organizations, collaborations, tasks and results. In
  this section we will explain how to configure and manage a server.
---

# Running the server

## 🆕 \(Re-\)Configuring a \(New\) Server

After installing the necessary elements, we need to create and configure a server.

Servers are created from a configuration file. This file can be generated through the command `vserver new`or manually from a YAML file manually, as explained [in this section](server-configuration.md).

## 📥 Importing entities

After configuring the server, we need to define the users, organizations, collaborations, etc. This can be done using the `vserver import` command, as explained [in this section](importing-entities.md).

Notice that we need to follow the same steps when loading/importing new data.

## 🏤 Managing Server Data

Managing entities at the server \(e.g. users, organizations, etc...\) is usually done through the RESTful API. As administrator you also have access to a Python API by using the command `vserver shell` which opens an iPython shell in which all database models are available. To see what you can do with these models, see [this](shell.md) page!

## 🏎 Starting the server

Once the configuration is done and you created at least one user, you can start the server instance by using `vserver start` and then selecting the configuration you want to use.  If you know the name of the configuration, you can use the flag `--name` to specify it. Use `vserver start --help` to see all options.

If you have a configuration file which is in a non-default location than you can specify this by the `--config` flag. 

{% hint style="info" %}
Note that having Docker installed is **not** strictly  necessary to run the server. However, it is needed if you want to run a private registry on the same machine. See [Docker registry](../../installation/docker-registry.md) for details.
{% endhint %}

## 💻 Server Commands

As a reference, these are the sub-commands available to manage the server\(s\). These commands can also be found by simply calling `vserver` . To see which options are available for each of the individual commands, use `vserver CMD --help`. 

| Command | Description |
| :--- | :--- |
| `vserver attach` | Attach the logs from the Docker container to the terminal |
| `vserver files` | List the file locations of the server instance |
| `vserver import` | Import server entities as a batch |
| `vserver list` | List the available server instances |
| `vserver new` | Create a new server configuration |
| `vserver shell` | Run a server instance python shell |
| `vserver start` | Start the server |
| `vserver stop` | Stop a or all running server |
| `vserver version` | Shows the current version of the running server |

## ✍ Logging

If you attach the log to the current console, you can see a information about the server. You can either attach the logs when you start the server `vserver start --attach` or you can attach to an already running server by using `vserver attach` and select the server you want to see the log files from.

For example, the output below shows:

```text
- ---------------------------------------------
-  Welcome to
-                   _                     __
-                  | |                   / /
- __   ____ _ _ __ | |_ __ _  __ _  ___ / /_
- \ \ / / _` | '_ \| __/ _` |/ _` |/ _ \ '_ \
-  \ V / (_| | | | | || (_| | (_| |  __/ (_) |
-   \_/ \__,_|_| |_|\__\__,_|\__, |\___|\___/
-                             __/ |
-                            |___/
-
-  --> Join us on Discord! https://discord.gg/rwRvwyK
-  --> Docs: https://docs.vantage6.ai
-  --> Blog: https://vantage6.ai
- ---------------------------------------------
- Started application vantage6 with environment application
- Current working directory is '/'
- Succesfully loaded configuration from '/mnt/config/Dave-A.yaml'
- Logging to '/mnt/log/dave-a.log'
- Common package version '2.1.0'
- vantage6 version '2.1.0'
- Node package version '2.1.0'
- Connecting server: https://trolltunga.vantage6.ai:443
```

## 🧁 Deployment of a Production environment

We give some recommendations for deploying a vantage6 server in [this](deployment.md) section.


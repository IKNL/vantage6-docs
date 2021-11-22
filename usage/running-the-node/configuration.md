---
description: In this section you will learn how to (re-)configure a nod
---

# 📃 Configure

When creating a new node you can use the command line or create the YAML configuration file manually. In case you want to reconfigure a node you need to edit the YAML file.&#x20;

## :mage: Configure using the Wizard

The most straight forward way of creating a new server configuration is using the command `vnode new` which allows you to configure the most basic settings.

![](<../../.gitbook/assets/vnodenew (1).jpg>)

By default the node configuration file is stored at user level, which isolates this configuration from other users. In case you want this configuration to be available for all users, simply add the `--system` flag when you invoke `vnode new`.

{% hint style="info" %}
To update a configuration you need to modify the created YAML file. To see where this file is located you can use the command `vnode files` . Do not forget to specify the `--system` flag in the case of a system-wide configuration.
{% endhint %}

## Create / Edit configuration file

The configuration wizard outputs a YAML file which can be loaded into vantage6. It is also possible to create this file yourself, to see [Configuration File Structure](./#configuration-file-structure) or an example on our [github](https://github.com/iknl/ppdli) page. This file can be stored (and referred to) at any location at the machine, although it recommended to use either the vantage6 system or user folder. These folders are different per operating system.

| Windows | C:\ProgramData\vantage6\node               | C:\Users\\\<user>\AppData\Local\vantage6\node            |
| ------- | ------------------------------------------ | -------------------------------------------------------- |
| MacOS   | /Library/Application Support/vantage6/node | /Users/\<user>/Library/Application Support/vantage6/node |
| Linux   | /etc/vantage6/node                         | /home/\<user>/.config/vantage6/node                      |

{% hint style="info" %}
To start a node using a configuration file at an arbitrary location you should use the config option:`vnode start --config /path/to/config.yaml` note that this will overwrite all other options.
{% endhint %}

## 🗃 Configuration File Structure

Similar to the server, each node instance (configuration) can have multiple environments. You can specify these under the key `environments` which allows four types: `dev` , `test`,`acc` and `prod` .

{% hint style="info" %}
We use [DTAP for key environments](https://en.wikipedia.org/wiki/Development,\_testing,\_acceptance\_and\_production). In short:

* `dev` Development environment. It is ok to break things here
* `test` Testing environment. Here, you can verify that everything works as expected. This environment should resemble as much as possible the target environment where the final solution will be deployed.
* `acc` Acceptance environment. If the tests were successful, you can try this environment, where the final user will test  his/her analysis to verify if everything meets his/her expectations.
* `prod` Production environment. Final version of the proposed solution.
{% endhint %}

If you do not want to specify any environment, you should only specify the key `application` .

In the end, the configuration file should have the following structure:

```yaml
application:
  api_key: api-key-from-server
  server_url: https://api.distributedlearning.ai
  port: 5000
  api_path: '/api'
  task_dir: tasks
  encryption:
    enabled: true
    private_key: /path/to/private_key.pem
  docker_registries:
    - registry: harbor.vantage6.ai
      username: docker-registry-user
      password: your-password!
  allowed_images:
    - ^harbor.vantage6.ai/[a-zA-Z]+/[a-zA-Z]+
  databases:
    default: /path/to/database.csv
    alternative: /path/to/other/database.csv
  logging:
      level:        DEBUG             # Can be on of 'DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'
      file:         node.log          # Filename of logfile
      use_console:  True              # Log output to the console?
      backup_count: 5                 # Number of logs to keep
      max_size:     1024              # Specified in kB (i.e. 1024 means a maximum file size of 1MB)
      format:       "%(asctime)s - %(name)-14s - %(levelname)-8s - %(message)s"
      datefmt:      "%H:%M:%S"
```

## 📰 Parameter description

| Parameter           | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `api_key`           | API key used to authenticate at the server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `server_url`        | URL of the vantage6 server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `port`              | Port of the vantage6 server. Should be `443` in most cases.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `api_path`          | Path of the API. Usually empty or `/api`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `task_dir`          | Local task directory name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `encryption`        | <p>Section that contains encryption settings: <br><code>enabled</code>: Boolean to indicate whenever encryption is used or not. <code>private_key</code>: path to private key file. <br> See <a href="configuration.md#encryption">here</a> for more details.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `docker_registries` | <p>Section that contains  a list Docker registry login credentials: <br><code>registry</code>: url of the docker registry<br><code>username</code>: username<br><code>password</code>: password <br>See <a href="configuration.md#docker-login">here</a> for more detail.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `allowed_images`    | List of [regular expressions](https://en.wikipedia.org/wiki/Regular\_expression) that control which algorithms are allowed on this node. See [here](configuration.md#allowed-images) for more detail.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `databases`         | List of databases in `key`:`value` pair (💔 broken in current version. Will be fixed in a future release, only the default database can be used.)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `logging`           | <p><code>file</code>: filename of the log-file, used by <a href="https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler">RotatingFileHandler</a> <br><code>backup_count</code>: the number of log files that are kept, used by <a href="https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler">RotatingFileHandler</a> <br><code>max_size</code>: size kb of a single log file, used by <a href="https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler">RotatingFileHandler</a> <br><code>format</code>: input for <code>logging.Formatter</code>, see <a href="https://docs.python.org/3/library/logging.html#logging.Formatter">here</a>. <br><code>level</code>: debug level used, see <a href="https://docs.python.org/3/library/logging.html#logging-levels">here</a>. <br><code>use_console</code>: whenever the output needs to be shown in the console</p> |
| `algorithm_env`     | List of additional environment variables you want to provide to the algorithm containers. See [here](https://app.gitbook.com/@vantage6/s/vantage6/\~/drafts/-MW3WGlJQBsVFTFQ\_9md/usage/running-the-node/configuration#extra-environment-variables-2-0-1) for more detail.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

### 🔒 Encryption

Both the server and node need to agree if encryption is used or not. Encryption at the server is managed at collaboration level, i.e. each collaboration determines whenever encryption of all communication is used or not. You can enable or disable encryption in the configuration file by setting the `enabled` key in the encryption section.

```yaml
application:
  ...
  encryption:
    enabled: true
    private_key: /path/to/private_key.pem
  ...
```

To generate a new private key and upload the public key to the server you can use the command `vnode create-private-key`. You can also generate the key yourself and upload it by using the `/organization/<id>` endpoint.

{% hint style="info" %}
Note that public keys are managed at organization level, meaning that you only can use one private key for all your nodes. It is not possible (yet) to create an unique private key for each node you own.
{% endhint %}

### 🏳 Allowed Images

To control which algorithms are allowed at the node you can set the `allowed_images` key in the configuration file. This is expected to be a valid [regular expression](https://en.wikipedia.org/wiki/Regular\_expression).

```yaml
application:
  ...
  allowed_images:
    - ^harbor.vantage6.ai/[a-zA-Z]+/[a-zA-Z]+
  ...
```

### 🐳 Docker Login

If you are using a private docker repository the environment in which the node needs to be logged in to that repository. In case the Dockerized version of vantage6 is used you need to specify this in the configuration file using the `docker_registries` key.

```yaml
application:
  ...
  docker_registries:
    - registry: harbor.vantage6.ai
      username: docker-registry-user
      password: your-password!
  ...
```

### 📃 Extra Environment Variables \[2.0.1+]

It is possible to define additional environment variables for the algorithm containers. For example, this could be useful for sharing credentials for accessing a data-store.

```yaml
application:
    ...
    algorithm_env:
        var1: value1
        var2: 2
    ...
```

---
description: In this section you will learn how to configure the server.
---

# Configuration

## 🔤 Preliminaries

It is assumed that the Python package `vantage6` has been successfully installed \(see [Dockerized installation](../../installation/dockerized-installation.md) if necessary\).

## 🧙 Configuration Using the Wizard

The most straight forward way of creating a new server configuration is using the command `vserver new` in Python which allows you to configure the most basic settings. See the image below to get an overview of what happens. And see [parameter description](server-configuration.md#-parameter-description) for an explanation of the parameters.

![](../../.gitbook/assets/vservernew%20%281%29.jpg)

By default the `configuration-file` is stored at system level, which makes this configuration available for _all_ users. In case you want to use a user directory you can add the `--user` flag.

To update a configuration you need to modify the created YAML file. To see where this file is located you can use the command `vserver files` . Do not forget to specify the flag `--system` in the case of a system-wide configuration or the flag `--user` in case of a user-level configuration.

{% hint style="info" %}
**Node and server on the same machine**  
The challenge is that the node can reach \(find\) the server. This is only difficult when using the dockerized node, and differs per operating system.

**Windows and Mac**  
Setting the server `ip` to `127.0.0.1` makes the server reachable at your localhost \(even when the dockerized version is used\). In order for the node to reach this server, set the `server_url` to `host.docker.internal`. 

**Linux**  
You should bind the server to the network interface, this ip is reachable from within docker \(thus from the node\)
{% endhint %}

## 👩🔬 Configuration Using a Custom YAML File

The configuration wizard outputs a YAML file that can be used by the server. It is also possible to create this YAML file by hand. An example of the structure of this file is shown [below](server-configuration.md#configuration-file-structure).

While it's technically possible to store the configuration files anywhere on your machine, it is _highly_ recommended to use the default **vantage6** system \(or user\) folder; this is done automatically if you use the [wizard](server-configuration.md#configure-using-the-wizard). The location of this folder depends on the operating system used \(see the table below\).

| OS | System | User |
| :--- | :--- | :--- |
| Windows | `C:\ProgramData\vantage\server` | `C:\Users\<user>\AppData\Local\vantage\server\` |
| MacOS | `/Library/Application Support/vantage/server/` | `/Users/<user>/Library/Application Support/vantage/server/` |
| Ubuntu | `/etc/xdg/vantage/server/` | `~/.config/vantage/server/` |

## 🗃 Configuration File Structure

Each server instance \(configuration\) can have multiple environments. You can specify these under the key `environments` which allows four types: `dev` , `test`,`acc` and `prod` .

{% hint style="info" %}
We use [DTAP for key environments](https://en.wikipedia.org/wiki/Development,_testing,_acceptance_and_production). In short:

* `dev` Development environment. It is ok to break things here
* `test` Testing environment. Here, you can verify that everything works as expected. This environment should resemble as much as possible the target environment where the final solution will be deployed.
* `acc` Acceptance environment. If the tests were successful, you can try this environment, where the final user will test  his/her analysis to verify if everything meets his/her expectations.
* `prod` Production environment. Final version of the proposed solution.
{% endhint %}

If you do not want to specify any environment, you should only specify the key `application` .

In the end, the configuration file should look like this:

```yaml
application:
  ...
environments:
  test:
    description: Test
    type: test
    ip: '127.0.0.1'
    port: 5000
    api_path: /api
    uri: sqlite:///test.sqlite
    allow_drop_all: True
    jwt_secret_key: super-secret-key! #recommended but optional
    logging:
      level:        DEBUG
      file:         test.log
      use_console:  True
      backup_count: 5
      max_size:     1024
      format:       "%(asctime)s - %(name)-14s - %(levelname)-8s - %(message)s"
      datefmt:      "%Y-%m-%d %H:%M:%S"

  prod:
    ...
```

## 📰 Parameter description

| Parameter | Details |
| :--- | :--- |
| `description` | Human readable description of the server instance. This is to help your peers to identify the server. |
| `type` | Should be `prod`, `acc`, `test` or `dev`. In case the _type_ is set to `test` the JWT-tokens expiration is set to 1 day \(default is 6 hours\). The other types can be used in future releases of vantage6. |
| `ip` | Internal ip-address where the server can be reached. Note that in case you are using the Docker version of vantage6 this is the ip address inside the container, and binds to the ip address `127.0.0.1` at your host machine \(🚧 This will change in a future release in vantage6\). |
| `port` | Port to which the server listens. In case of the Dockerized version this will be used both internally in the container as at your host \(At your host machine the server will be reachable at 127.0.0.1:`port`\). |
| `api_path` | API path prefix. \(e.g. `https://yourdomain.org/api_path/...`\) |
| `secret_key` | The secret key used to generate JWT authorization tokens. This should be kept secret as others are able to generate access tokens if they know this secret. This parameter is optional. In case it is not provided in the configuration it is generated each time the server starts. Thereby invalidating all previous handout keys. |
| `uri` | The URI to the database. This should be a valid SQLAlchemy URI, See [here](https://docs.sqlalchemy.org/en/latest/core/engines.html#database-urls) for more information. \(e.g. for an Sqlite database: `sqlite:///database-name.sqlite`, or Postgres `postgresql://username:password@172.17.0.1/database`. |
| `allow_drop_all` | This should be set to `false` in production❗ as this allows to completely wipe the database in a single command. Useful to set to `true` when testing/developing. |
| `logging` |  `file`: filename of the log-file, used by [RotatingFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler) `backup_count`: the number of log files that are kept, used by [RotatingFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler) `max_size`: size kb of a single log file, used by [RotatingFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler) `format`: input for `logging.Formatter`, see [here](https://docs.python.org/3/library/logging.html#logging.Formatter). `level`: debug level used, see [here](https://docs.python.org/3/library/logging.html#logging-levels) `use_console`: whenever the output needs to be shown in the console |

See [Configuration File Structure](server-configuration.md#-configuration-file-structure) for example values.

### 📫 Mail server

{% hint style="info" %}
The vantage6 server would work without configuring the mail server. However in order to let users reset their password \(and in the future receive notifications\) these settings are required.
{% endhint %}

To configure a Mail server add the following block to your configuration file:

```yaml
prod: 
    ...
    smtp:
      port: 587
      server: smtp.office365.com
      username: your-username
      password: super-secret-password
    ...
```




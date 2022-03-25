---
description: In this section you will learn how to (re)configure nodes.
---

# Configure

The vantage6-node requires a configuration file to run. This is a `yaml` file with a specific format.  To create an initial configuration file, start the configuration wizard via: `vnode new` . You can also create and/or edit this file manually.

The directory where the configuration file is stored depends on your operating system (OS). It is possible to store the configuration file at **system** or at **user** level. By default, node configuration files are stored at **user** level. The default directories per OS are as follows:

| **Operating System** | **System-folder**                            | **User-folder**                                           |
| -------------------- | -------------------------------------------- | --------------------------------------------------------- |
| Windows              | `C:\ProgramData\vantage\node`                | `C:\Users\<user>\AppData\Local\vantage\node`              |
| MacOS                | `/Library/Application Support/vantage6/node` | `/Users/<user>/Library/Application Support/vantage6/node` |
| Linux                | `/etc/vantage6/node`                         | `/home/<user>/.config/vantage6/node`                      |

{% hint style="warning" %}
The command `vnode` looks in certain directories by default. It is possible to use any directory and specify the location with the `--config` flag. However, note that using a different directory requires you to specify the `--config` flag every time!
{% endhint %}

## Configuration file structure

Each node instance (configuration) can have multiple environments. You can specify these under the key `environments` which allows four types: `dev` , `test`,`acc` and `prod` . If you do not want to specify any environment, you should only specify the key `application` (not within `environments`) .

{% hint style="success" %}
We use [DTAP for key environments](https://en.wikipedia.org/wiki/Development,\_testing,\_acceptance\_and\_production). In short:

* `dev` Development environment. It is ok to break things here
* `test` Testing environment. Here, you can verify that everything works as expected. This environment should resemble the target environment where the final solution will be deployed as much as possible.
* `acc` Acceptance environment. If the tests were successful, you can try this environment, where the final user will test his/her analysis to verify if everything meets his/her expectations.
* `prod` Production environment. The version of the proposed solution where the final analyses are executed.
{% endhint %}

```yaml
application:

  # API key used to authenticate at the server.
  api_key: ***
  
  # URL of the vantage6 server  
  server_url: https://petronas.vantage6.ai
  
  # port the server listens to
  port: 443
    
  # API path prefix that the server uses. Usually '/api' or an empty string
  api_path: ''
  
  # subnet of the VPN server 
  vpn_subnet: 10.76.0.0/16
  
  # add additional environment variables to the algorithm containers.
  # this could be usefull for passwords or other things that algorithms
  # need to know about the node it is running on
  # OPTIONAL
  algorithm_env:
  
    # in this example the environment variable 'player' has 
    # the value 'Alice' inside the algorithm container
    player: Alice
  
  # path or endpoint to the local data source. The client can request a
  # certain database to be used if it is specified here. They are 
  # specified as label:local_path pairs.
  databases:
    default: D:\data\datafile.csv
  
  # end-to-end encryption settings
  encryption:
    
    # whenever encryption is enabled or not. This should be the same
    # as the `encrypted` setting of the collaboration to which this 
    # node belongs. 
    enabled: false
    
    # location to the private key file
    private_key: /path/to/private_key.pem
  
  # To control which algorithms are allowed at the node you can set 
  # the allowed_images key. This is expected to be a valid regular 
  # expression
  allowed_images:
    - ^harbor.vantage6.ai/[a-zA-Z]+/[a-zA-Z]+
  
  # credentials used to login to private Docker registries
  docker_registries:
    - registry: docker-registry.org
      username: docker-registry-user
      password: docker-registry-password
    
  # Settings for the logger
  logging:
      # Controls the logging output level. Could be one of the following
      # levels: CRITICAL, ERROR, WARNING, INFO, DEBUG, NOTSET
      level:        DEBUG
      
      # Filename of the log-file, used by RotatingFileHandler
      file:         my_node.log
      
      # whenever the output needs to be shown in the console
      use_console:  True
      
      # The number of log files that are kept, used by RotatingFileHandler
      backup_count: 5
      
      # Size kb of a single log file, used by RotatingFileHandler
      max_size:     1024
      
      # format: input for logging.Formatter,
      format:       "%(asctime)s - %(name)-14s - %(levelname)-8s - %(message)s"
      datefmt:      "%Y-%m-%d %H:%M:%S"
  
  # directory where local task files (input/output) are stored 
  task_dir: C:\Users\<your-user>\AppData\Local\vantage6\node\tno1
```

## Configure using the Wizard

The most straightforward way of creating a new server configuration is using the command `vnode new` which allows you to configure the most basic settings.

![](<../../.gitbook/assets/vnodenew (1).jpg>)

By default, the configuration is stored at user level, which makes this configuration available only for your user. In case you want to use a system directory you can add the `--system` flag when invoking the `vnode new` command.

## Update configuration

To update a configuration you need to modify the created `yaml` file. To see where this file is located, you can use the command `vnode files` . Do not forget to specify the flag `--system` in case of a system-wide configuration or the `--user` flag in case of a user-level configuration.

## Local test setup

Refer to [here ](../running-the-server/server-configuration.md#local-test-setup)if you want to run both the node and server on the same machine.

##

# Configuration

## 🧙 Configure using the Wizard

The most straight forward way of creating a new server configuration is using the command `vnode new` which allows you to configure the most basic settings.

![Configure a new node using the wizard.](../../.gitbook/assets/node-configuration.png)

By default the node configuration file is stored at user level, which isolates this configuration from other users. In case you want this configuration to be available for all users, simply add the `--system` flag \(make sure you have sufficient rights to do this\).

{% hint style="info" %}
To update a configuration you need to modify the created YAML file. To see where this file is located you can use the command `vnode files` . Do not forget to specify the `--system` flag in the case of a system-wide configuration.
{% endhint %}

## 👩🔬 Configure using a custom YAML file

The configuration wizard outputs a YAML file which can be loaded into VANTAGE6. It is also possible to create this file yourself, to see [Configuration File Structure](./#configuration-file-structure) or an example on our [github](https://github.com/iknl/ppdli) page. This file can be stored \(and referred to\) at any location at the machine, although it recommended to use either the VANTAGE6 system or user folder. These folders are different per operating system.

| Operating System | System-folder | User-folder |
| :--- | :--- | :--- |
| Windows | C:\ProgramData\vantage\node | C:\Users\&lt;user&gt;\AppData\Local\vantage\node |
| MacOS |  |  |
| Linux |  | /home/&lt;user&gt;/.config/vantage/node/ |

{% hint style="info" %}
To start a node using a configuration file at an arbitrary location you should use the config option:`vnode start --config /path/to/config.yaml` note that this will overwrite all other options.
{% endhint %}

## 🗃 Configuration File Structure

Each node instance \(configuration\) can have multiple environments. If you do not want to specify any environment you should only specify the `application` key. In the case you do want to use environments you can specify this in `environments` which allows up to four environments: `dev`, `test`,`acc`, and `prod` . It should have the following structure:

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

## 🔒 Encryption

Both the server and node need to agree if encryption is used or not. Encryption at the server is managed at collaboration level, i.e. each collaboration determines whenever encryption of all communication is used or not. You can enable or disable encryption in the configuration file by setting the `enabled` key in the encryption section. To generate a new private key and upload the public key to the server you can use the command `vnode create-private-key`.

{% hint style="warn" %}
Note that public keys are managed at organization level, meaning that you only can use one private key for all your nodes. It is not possible (yet) to create an unique private key for each node you own.
{% endhint %}

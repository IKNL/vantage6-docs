---
description: >-
  In this section you will learn how to configuring the server to your needs.
  This can be done either through the command line or by creating a custom YAML
  configuration file.
---

# Configuration

It is assumed that the joey Python package has been successfully installed. If not, have a look at previous steps described in [Install VANTAGE6](../install-vantage6.md).

## 🧙♂ Configure Using the Wizard 

The most straight forward way of creating a new server configuration is using the command `vserver new` in python which allows you to configure the most basic settings.  Follow the image below to get an overview of what happens.

![Configure a new server using the wizard](../../.gitbook/assets/annotation-2019-06-13-112656.png)

By default the configuration-file is stored at system level, which makes this configuration available for all users. In case you want to use a user directory you can add the `--user` flag. 

{% hint style="info" %}
To update a configuration you need to modify the created YAML file. To see where this file is located you can use the`vserver files` . Do not forget to specify the `--system` flag in the case of a system-wide configuration.
{% endhint %}

## 👩🔬 Configure Using a custom YAML file

The configuration wizard outputs a YAML file which can be loaded into VANTAGE6. It is also possible to create this YAML file yourself, to see the structure of this file please see [below](server-configuration.md#configuration-file-structure). This file can be stored \(and referred to\) at any location at the machine, although it is recommended to use either the VANTAGE6 system or user folders. This folder is different for different operating systems. The table below gives you an idea of where the file can be located having followed our instructions of installing VANTAGE6.

| OS | System | User |
| :--- | :--- | :--- |
| Windows | C:\ProgramData\vantage\server | C:\Users\&lt;user&gt;\AppData\Local\vantage\server\ |
| MacOS |  |  |
| Ubuntu | /etc/xdg/vantage | ~/.config/vantage/server/ |

{% hint style="danger" %}
Make sure that the uri is understood by SQLAlchemy, see [here](https://docs.sqlalchemy.org/en/latest/core/engines.html#database-urls) for more information.
{% endhint %}

### 🗃 Configuration File Structure

Each server instance \(configuration\) can have multiple environments. If you do not want to specify any environment you should only specify the key `application` . In case you do want to use environments you can specify this in `environments` which allows four types: `dev` , `test`,`acc` and `prod` .  It should look like this:

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

## ✍ Logging

If logging to the console is enabled, starting the server or loading the fixtures should output some information that can be helpful in determining the cause of problems. For example, the output below shows:

* Which environment was used
* What configuration file was used
* Which database was used

```bash
################################################################################
#                                pytaskmanager                                 #
################################################################################
Started application 'pytaskmanager' with environment 'test'
Current working directory is '/home/test-user'
Succesfully loaded configuration from '/home/test-user/.config/pytaskmanager/server/default.yaml'
Logging to '/home/test-user/.cache/pytaskmanager/log/server/default.log'
Initializing the database
  driver:   sqlite
  host:     None
  port:     None
  database: /home/test-user/.local/share/pytaskmanager/server/default/test.sqlite
  username: None
Database initialized!
```


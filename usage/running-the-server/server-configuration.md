---
description: In this section you will learn how to (re)configure a server
---

# Configure

The vantage6-server requires a configuration file to run. This is a `yaml` file with specific contents. You can create and edit this file manually. To create an initial configuration file you can also use the configuration wizard: `vserver new`.

The directory where to store the configuration file depends on you operating system (OS). It is possible to store the configuration file at **system** or at **user** level. By default, server configuration files are stored at **system** level. The default directories per OS are as follows:

| **OS**  | **System**                                      | **User**                                                     |
| ------- | ----------------------------------------------- | ------------------------------------------------------------ |
| Windows | `C:\ProgramData\vantage6\server`                | `C:\Users\<user>\AppData\Local\vantage6\server\`             |
| MacOS   | `/Library/Application Support/vantage6/server/` | `/Users/<user>/Library/Application Support/vantage6/server/` |
| Ubuntu  | `/etc/xdg/vantage6/server/`                     | `~/.config/vantage6/server/`                                 |

{% hint style="warning" %}
The command `vserver` looks in certain directories by default. It is possible to use any directory and specify the location with the `--config` flag. However, note that using a different directory requires you to specify the `--config` flag every time!
{% endhint %}

## Configuration file structure

Each server instance (configuration) can have multiple environments. You can specify these under the key `environments` which allows four types: `dev` ,`test`, `acc` and `prod` . If you do not want to specify any environment, you should only specify the key `application` (not within `environments`) .

{% hint style="success" %}
We use [DTAP for key environments](https://en.wikipedia.org/wiki/Development,\_testing,\_acceptance\_and\_production). In short:

* `dev` Development environment. It is ok to break things here
* `test` Testing environment. Here, you can verify that everything works as expected. This environment should resemble the target environment where the final solution will be deployed as much as possible.
* `acc` Acceptance environment. If the tests were successful, you can try this environment, where the final user will test his/her analysis to verify if everything meets his/her expectations.
* `prod` Production environment. The version of the proposed solution where the final analyses are executed.
{% endhint %}

```yaml
application:
  ...
environments:
  test:
    
    # Human readable description of the server instance. This is to help 
    # your peers to identify the server
    description: Test
    
    # Should be prod, acc, test or dev. In case the type is set to test 
    # the JWT-tokens expiration is set to 1 day (default is 6 hours). The 
    # other types can be used in future releases of vantage6
    type: test
    
    # IP adress to which the server binds. In case you specify 0.0.0.0
    # the server listens on all interfaces
    ip: 0.0.0.0
    
    # Port to which the server binds
    port: 5000
    
    # API path prefix. (i.e. https://yourdomain.org/api_path/<endpoint>)
    api_path: /api
    
    # The URI to the server database. This should be a valid SQLAlchemy URI, 
    # e.g. for an Sqlite database: sqlite:///database-name.sqlite, 
    # or Postgres: postgresql://username:password@172.17.0.1/database).
    uri: sqlite:///test.sqlite
    
    # This should be set to false in production as this allows to completely 
    # wipe the database in a single command. Useful to set to true when 
    # testing/developing.
    allow_drop_all: True
    
    # The secret key used to generate JWT authorization tokens. This should 
    # be kept secret as others are able to generate access tokens if they 
    # know this secret. This parameter is optional. In case it is not 
    # provided in the configuration it is generated each time the server 
    # starts. Thereby invalidating all previous distributed keys.
    # OPTIONAL
    jwt_secret_key: super-secret-key! # recommended but optional

    # Settings for the logger
    logging:
    
      # Controls the logging output level. Could be one of the following
      # levels: CRITICAL, ERROR, WARNING, INFO, DEBUG, NOTSET
      level:        DEBUG
      
      # Filename of the log-file, used by RotatingFileHandler
      file:         test.log
      
      # Whether the output is shown in the console or not
      use_console:  True
      
      # The number of log files that are kept, used by RotatingFileHandler
      backup_count: 5
      
      # Size in kB of a single log file, used by RotatingFileHandler
      max_size:     1024
      
      # format: input for logging.Formatter,
      format:       "%(asctime)s - %(name)-14s - %(levelname)-8s - %(message)s"
      datefmt:      "%Y-%m-%d %H:%M:%S"
      
    # Configure a smtp mail server for the server to use for administrative 
    # purposes. e.g. allowing users to reset their password. 
    # OPTIONAL
    smtp:
      port: 587
      server: smtp.yourmailserver.com
      username: your-username
      password: super-secret-password
    
    # If algorithm containers need direct communication between each other
    # the server also requires a VPN server. (!) This must be a EduVPN
    # instance as vantage6 makes use of their API (!)
    # OPTIONAL
    vpn_server:
        # the URL of your VPN server
        url: https://your-vpn-server.ext
        
        # OATH2 settings, make sure these are the same as in the 
        # configuration file of your EduVPN instance
        redirect_url: http://localhost
        client_id: your_VPN_client_user_name
        client_secret: your_VPN_client_user_password
        
        # Username and password to acccess the EduVPN portal
        portal_username: your_eduvpn_portal_user_name
        portal_userpass: your_eduvpn_portal_user_password
        
  prod:
    ...
```

## Configuration wizard

The most straightforward way of creating a new server configuration is using the command `vserver new` which allows you to configure most settings. See the [#structure](server-configuration.md#structure "mention") what each setting represents.

![screenshot when the command vserver new is invoked.](<../../.gitbook/assets/vservernew (1).jpg>)

By default, the configuration is stored at system level, which makes this configuration available for _all_ users. In case you want to use a user directory you can add the `--user` flag when invoking the `vserver new` command.

## Update configuration

To update a configuration you need to modify the created `yaml` file. To see where this file is located you can use the command `vserver files` . Do not forget to specify the flag `--system` in case of a system-wide configuration or the flag `--user` in case of a user-level configuration.

## Local test setup

If the nodes and the server run at the same machine, you have to make sure that the node can reach the server.&#x20;

**Windows and (intel) Mac**\
****Setting the server IP to `0.0.0.0` makes the server reachable at your localhost (this is also the case when the dockerized version is used). In order for the node to reach this server, set the `server_url` setting to `host.docker.internal`.&#x20;

:warning: On the **M1** mac the local server might not be reachable from your nodes as `host.docker.internal` does not seem to refer to the host machine. Reach out to us on Discourse for a solution if you need this!

**Linux**\
****You should bind the server to `0.0.0.0`. In the node configuration files, you can then use `172.17.0.1`, assuming you use the default docker network settings.

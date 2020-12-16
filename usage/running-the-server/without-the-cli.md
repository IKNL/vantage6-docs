---
description: >-
  In this page we'll explain how to run a Server instance without using the
  vantage6 CLI.
---

# Without the CLI

{% hint style="danger" %}
This is not the intended use! However there are use-cases where you do not want to use our CLI and start the Node docker container manually.
{% endhint %}

Pull a server docker image from our docker registry. If you do not specify a version it will automatically pull the latest version.

{% tabs %}
{% tab title="Bash" %}
```bash
docker pull harbor.vantage6.ai/infrastructure/server[:version]
```
{% endtab %}
{% endtabs %}

## 🏇 Mounts

| Mount path  | Description |
| :--- | :--- |
| /mnt/config.yaml | Contains the server configuration file |
| /mnt/database/ | _\(Optional\)_ folder that contains the database file. Note that if you set this, you also need to specify the environment variable VANTAGE6\_DB\_URI |

{% hint style="info" %}
In case you want to use an external database, you can omit the /mnt/database and specify the URI form the database in the /mnt/config.yaml. Make sure that the Docker container can reach this URI.
{% endhint %}

## 📦 Environment variables

| Variable name | Description |
| :--- | :--- |
| VANTAGE6\_DB\_URI | Local path to where the database file is located. \(e.g. /mnt/database/server.sqlite\) |

## 🏎 Run the Server

The `configuration_name` is the filename without the .yaml extension, and the `environment` is the name of the environment you want to use: `test` , `prod` , `acc` ,`dev` or `application`, see [this ](server-configuration.md#configuration-file-structure)section how the configuration file should be formatted.

{% tabs %}
{% tab title="Bash" %}
```bash
docker run \
    -e VANTAGE_DB_URI="server.sqlite" \
    -v /host/config.yaml:/mnt/config.yaml \
    -v /host/server.sqlite:/mnt/database/server.sqlite \
    harbor.distributedlearning.ai/infrastructure/server \
    configuration_name environment
```
{% endtab %}
{% endtabs %}


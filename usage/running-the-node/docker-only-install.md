---
description: >-
  In this page we'll explain how to run a Node instance without using the
  vantage6 CLI.
---

# Without the CLI

{% hint style="danger" %}
This is not the intended use! However there are use-cases where you do not want to use our CLI and start the Node docker container manually.
{% endhint %}

Pull the latest Node docker image:

{% tabs %}
{% tab title="Bash" %}
```bash
docker pull harbor.vantage6.ai/infrastructure/node
```
{% endtab %}
{% endtabs %}

This Docker container expects multiple mounts, a volume, and a environment variable.

## 🏇 Mounts

| Mount path  | Description |
| :--- | :--- |
| /mnt/database.csv | Contains the database file that contains the data that the Node is allowed to access |
| /mnt/log | _Directory_ where the log files can be stored at the host machine |
| /mnt/data | Docker data volume which is used for transferring data between node and algorithms |
| /mnt/config | _Directory_ containing the configuration file |
| /var/run/docker.sock | Contains the docker.sock from the host system. Usually: //var/run/docker.sock |
| /mnt/private\_key.pem | **Optional**. If provided the private key will be used to decrypt any incoming messages. |

## 📦 Volume

The node expects a single Docker volume, in which it can share data with the algorithm containers. You can give this volume any name as you specify its name in the environment variables.

```text
docker volume create datavolume-name
```

## 🏎 Run the Node

The `configuration_name` is the filename without the .yaml extension, and the `environment` is the name of the environment you want to use: `test` , `prod` , `acc` ,`dev` or `application`, see [this ](configuration.md#configuration-file-structure)section how the configuration file should be formatted.

{% tabs %}
{% tab title="Bash" %}
```bash
docker run \
    -e DATA_VOLUME_NAME="some-name" \
    -v some-name:/mnt/data-volume \
    -v /host/database.csv:/mnt/database.csv \
    -v /host/log:/mnt/log \
    -v datavolume-name:/mnt/data \
    -v /host/config:/mnt/config \
    -v //var/run/docker.sock:/var/run/docker.sock \
    -v /host/private_key.pem:/mnt/private_key \
    harbor.distributedlearning.ai/infrastructure/node \
    configuration_name environment
```
{% endtab %}

{% tab title="PowerShell" %}
    docker run `
        -e DATA_VOLUME_NAME="some-name" `
        -v some-name:/mnt/data-volume `
        -v /host/database.csv:/mnt/database.csv `
        -v /host/log:/mnt/log `
        -v /host/data:/mnt/data `
        -v /host/config:/mnt/config `
        -v //var/run/docker.sock:/var/run/docker.sock `
        -v /host/private_key.pem:/mnt/private_key `
        harbor.distributedlearning.ai/infrastructure/node `
        configuration_name environment
{% endtab %}
{% endtabs %}


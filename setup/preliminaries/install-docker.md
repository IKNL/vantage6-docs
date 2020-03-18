---
description: >-
  Depending on the part of the system you want to install, you'll need the
  Docker run-time to be available. This page aims to get you started with the
  installation of Docker.
---

# Docker

{% hint style="danger" %}
**Only required for the node and algorithm development**

You only need to install Docker when you are installing a node or are planning to develop algorithms. Docker is **not** required for the client \(i.e. using VANTAGE6 to do research\), or if you are setting up a server. 
{% endhint %}

## Linux

VANTAGE6 has been successfully tested on Ubuntu, other Linux distributions will most likely also work. To install Docker, follow the instructions on their [website ](https://docs.docker.com/install/linux/linux-postinstall/)to install the engine. The lazy way would be to run the get-docker script:

{% hint style="danger" %}
Always verify scripts you download before executing them!
{% endhint %}

{% tabs %}
{% tab title="Bash" %}
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
{% endtab %}
{% endtabs %}

If you are logged in as non-root user \(which should be the case\), you need to add your username to the docker-group in order to run VANTAGE6 without root privileges. 

{% tabs %}
{% tab title="Bash" %}
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```
{% endtab %}
{% endtabs %}

First, logout and login again so that the group privileges are granted to your user. Then you can test it by executing docker commands without `sudo`:

{% tabs %}
{% tab title="Bash" %}
```bash
docker run hello-world
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you run into issues, please check the Docker documentation
{% endhint %}

## MacOS 

The easiest way to install Docker on MacOS is installing Docker desktop, see these [instructions](https://docs.docker.com/docker-for-mac/install/). 

{% tabs %}
{% tab title="Shell" %}
```text
docker run hello-world
```
{% endtab %}
{% endtabs %}

## Windows

The easiest way to install Docker on Windows is installing Docker desktop, see these [instructions](https://docs.docker.com/docker-for-windows/install/). Make sure you select **Linux Containers** during the installation process. After installing the Docker engine needs access to the dataset location. Right click on the Docker icon and select settings:

![Find your way to the settings.](../../.gitbook/assets/image.png)

Then go to Shared Drives and make sure the drive where the data is located is checked and press Apply.

![Make sure the drive where the data is located is shared with the Docker engine.](../../.gitbook/assets/image%20%281%29.png)

{% tabs %}
{% tab title="PowerShell" %}
```aspnet
docker run hello-world
```
{% endtab %}
{% endtabs %}


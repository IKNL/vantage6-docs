---
description: >-
  Depending on the part of the system you want to install, you'll need the
  Docker run-time to be available. This page aims to get you started with the
  installation of Docker.
---

# Installation of Docker

{% hint style="info" %}
Docker is required for the node instance to work. In case of the server, this is only required when you also want to host a private docker registry.
{% endhint %}

## MacOS

Download Docker from [docker.com](https://hub.docker.com/editions/community/docker-ce-desktop-mac) and follow the instructions.

## Windows

Download Docker from [docker.com](https://hub.docker.com/editions/community/docker-ce-desktop-windows) and follow the instructions.

## Ubuntu

Follow the instructions from [docker.com](https://docs.docker.com/install/linux/docker-ce/ubuntu/). 

## Debian

Unfortunately Docker is not available in Debian 9 \(stretch\) by default, so we'll have to add a package repository to `apt`. We'll start by installing sudo as this is needed later on. Don't forget to replace `<username>` with the username you're using.

```bash
apt install sudo
addgroup <username> sudo
```

After installing `sudo` you'll need to logout and login again for group membership to take effect. When this is done, we can proceed to install Docker. This has been automated by a shell-script from docker.com.

```bash
sudo apt install curl
curl -sSL https://get.docker.com/ | sh
addgroup $(whoami) docker
```

After installing the `docker` command you'll need to logout and login again for group membership to take effect. Then you can test your installation with the following command:


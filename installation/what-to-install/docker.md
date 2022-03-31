---
description: Required for both the node and server
---

# 🐳 Docker

Docker facilitates encapsulation of applications and their dependencies in packages that can be easily distributed to diverse systems. Algorithms are stored in Docker images which nodes can download and execute. Besides the algorithms, both the node and server are also running from a docker container themselves.&#x20;

Please refer to [this page](https://docs.docker.com/engine/install/) on how to install Docker. To verify that Docker is installed and running you can run the `hello-world` example from Docker.

```bash
docker run hello-world
```

{% hint style="info" %}
🐳 Always make sure that Docker is running while using vantage6!

🐳 We recommend to always use the latest version of Docker.
{% endhint %}

{% hint style="warning" %}
Note that for Linux, some [post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/) may be required. Vantage6 needs to be able to run docker without `sudo`, and these steps ensure just that.
{% endhint %}

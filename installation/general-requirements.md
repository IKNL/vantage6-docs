## General requirements

### 🐍 Python3

Installation of any of the **vantage6** packages requires Python ≥ 3.6. For installation instructions, see [python.org](https://python.org), [anaconda.com](https://anaconda.com) or use the package manager native to your OS and/or distribution \(e.g. `apt` for debian or Ubuntu, `yum` for fedora, or `yast` for SuSE\).

{% hint style="info" %}
We recommend you install **vantage6** in a new, clean environment.
{% endhint %}

### 🐳 Docker

Running a node _always_ requires the Docker runtime to be installed \(and running\). Running the server only requires docker when it's run as a dockerized application.

The correct installation of docker can be verified with the following command:

```bash
docker run hello-world
```

{% hint style="info" %}
🐳 Always make sure that Docker is running!

🐳 If this command is not successful, please see [Docker's website](https://www.docker.com/get-started) for installation instructions specific for your platform.

🐳 We recommend to always use the latest version of Docker.
{% endhint %}

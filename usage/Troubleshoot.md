---
description: >-
  Always looking for trouble
---

# Troubleshoot

## I have an error
Contact us on our [Discord](https://discord.com/invite/eqbAjZ?utm_source=Discord%20Widget&utm_medium=Connect) channel. To help you faster provide the output of `pip freeze` and the output of `docker version` and the error message you are receiving.

## The Node won't start
The most common reason why a node wont start is that there is an error in the configuration file. Make sure you are using a valid configuration by [validating the parameters](running-the-node/configuration.md#parameter-description). 

Another very common cause is that Docker is not running. You can verify this using:
```text
docker run hello-world
```
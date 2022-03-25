# Concepts

Algorithms are executed at the vantage6-node. The node receives a computation task from the vantage6-server. The node will then retrieve the algorithm, execute it and return the results to the server.&#x20;

Algorithms are shared using [Docker images](https://docs.docker.com/get-started/#what-is-a-container-image) which are stored in a [Docker image registry](../../installation/server/docker-registry.md) which is accessible to the nodes. In the following sections we explain the fundamentals of algorithm containers.

[input-and-output.md](input-and-output.md "mention")\
Interface between the node and algorithm container

[wrappers.md](wrappers.md "mention")\
Library to simplify and standardized the node-algorithm input and output

[child-containers.md](child-containers.md "mention")\
Creating subtasks from an algorithm container

[networking.md](networking.md "mention")\
Communicate with other algorithm containers and the vantage6-server

[cross-language.md](cross-language.md "mention")\
Cross language data serialization

[package-and-distribute.md](package-and-distribute.md "mention")\
Packaging and shipping algorithms\

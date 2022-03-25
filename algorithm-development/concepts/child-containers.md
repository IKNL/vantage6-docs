# Child containers

When a user creates a task, one or more nodes spawn an algorithm container. These algorithm containers can create new tasks themselves.&#x20;

Every algorithm is supplied with a [JWT token](input-and-output.md). This token can be used to communicate with the vantage6-server. In case you use a algorithm wrapper, you simply can use the supplied `Client` in the case you use a [central function](wrappers.md#central-functions). &#x20;

A child container can be a parent container itself. There is no limit to the amount of task layers that can be created. It is common to have only a single parent container which handles many child containers.

![Each container can spawn new containers in the network. Each container is provided with a unique token which they can use to communicate to the vantage6-server. ](<../../.gitbook/assets/image (4).png>)

The token to which the containers have access is limited. The token can only be used to create a task in the same collaboration and using the same image.&#x20;

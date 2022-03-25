---
description: Probably important
---

# Security

As a data owner it is important that you take the necessary steps to protect your data. Vantage6 allows algorithms to run on your data and share the results with other parties. It is important that you review the algorithms before allowing them to run on your data.

Once you approved the algorithm, it is important that you can verify that the approved algorithm is the algorithm that runs on your data. There are two important steps to be taken to accomplish this:

* Set the (optional) `allowed_images` option in the node-configuration file. You can specify a regex expression here. For example
  1. `^harbor2.vantage6.ai/[a-zA-Z]+/[a-zA-Z]+`: allows all images from the vantage6 registry
  2. `^harbor2.vantage6.ai/algorithms/glm`: only allows this specific image, but all builds of this image
  3. `^harbor2.vantage6.ai/algorithms/glm@sha256:82becede498899ec668628e7cb0ad87b6e1c371cb8a1e597d83a47fac21d6af3`: allows only this specific build from the GLM to run on your data
* Enable `DOCKER_CONTENT_TRUST` to verify the origin of the image. For more details see the [documentation from Docker](broken-reference).&#x20;

{% hint style="danger" %}
By enabling `DOCKER_CONTENT_TRUST` **** you might not be able to use certain algorithms. You can check this by verifying that the images you want to be used are signed.&#x20;

In case you are using our Docker repository you need to use harbor**2**.vantage6.ai as harbor.vantage6.ai does not have a notary.&#x20;
{% endhint %}

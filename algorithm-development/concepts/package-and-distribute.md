# Package & distribute

Once the algorithm is completed it needs to be packaged and made available for retrieval by the nodes. The algorithm is packaged in a Docker image. A Docker image is created from a Dockerfile, which acts as blue-print. Once the Docker image is created it needs to be uploaded to a registry so that nodes can retrieve it.

## Dockerfile

{% tabs %}
{% tab title="Minimal" %}
A minimal Dockerfile should include a base-image, injecting your algorithm and execution command of your algorithm. For example:

```docker
# python3 image as base
FROM python:3

# copy your algorithm in the container
COPY . /app

# maybe your algorithm is installable.
RUN pip install /app

# execute your application
CMD python /app/app.py
```
{% endtab %}

{% tab title="Python wrapper" %}
When using the [Python wrapper](wrappers.md) the Dockerfile needs to follow a certain format. You should only change the `PKG_NAME` value to the Python package name of your algorithm. &#x20;

```docker
# python vantage6 algorithm base image
FROM harbor.vantage6.ai/algorithms/algorithm-base

# this should reflect the python package name
ARG PKG_NAME="v6-summary-py"

# install federated algorithm
COPY . /app
RUN pip install /app

ENV PKG_NAME=${PKG_NAME}

# Tell docker to execute `docker_wrapper()` when the image is run.
CMD python -c "from vantage6.tools.docker_wrapper import docker_wrapper; docker_wrapper('${PKG_NAME}'
```

{% hint style="info" %}
When using the python wrapper your algorithm file needs to be installable. See [here ](https://packaging.python.org/en/latest/tutorials/packaging-projects/)for more information on how to create a python package.
{% endhint %}
{% endtab %}

{% tab title="R wrapper" %}
When using the [R wrapper](wrappers.md) the Dockerfile needs to follow a certain format. You should only change the `PKG_NAME` value to the R package name of your algorithm. &#x20;

```docker
# The Dockerfile tells Docker how to construct the image with your algorithm.
# Once pushed to a repository, images can be downloaded and executed by the
# network hubs.
FROM harbor2.vantage6.ai/base/custom-r-base

# this should reflect the R package name
ARG PKG_NAME='vtg.package'

LABEL maintainer="Main Tainer <m.tainer@vantage6.ai>"

# Install federated glm package
COPY . /usr/local/R/${PKG_NAME}/

WORKDIR /usr/local/R/${PKG_NAME}
RUN Rscript -e 'library(devtools)' -e 'install_deps(".")'
RUN R CMD INSTALL --no-multiarch --with-keep.source .

# Tell docker to execute `docker.wrapper()` when the image is run.
ENV PKG_NAME=${PKG_NAME}
CMD Rscript -e "vtg::docker.wrapper('$PKG_NAME')"
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Additional Docker directives are needed when using direct communication between different algorithm containers, see [networking.md](networking.md "mention") for more information on this.
{% endhint %}

## Build & upload

If you are in the folder containing the Dockerfile, you can build the project as follows:

```
docker build -t repo/image:tag .
```

The `-t` indicated the name of your image. This name is also used as reference where the image is located on the internet. If you use Docker hub to store your images, you only specify your username as `repo` followed by your image name and tag: `USERNAME/IMAGE_NAME:IMAGE_TAG`. When using a private registry `repo` should contain the URL of the registry also: e.g. `harbor2.vantage6.ai/PROJECT/IMAGE_NAME:TAG`.&#x20;

Then you can push you image:

```
docker push repo/image:tag
```

Now that is has been uploaded it is available for nodes to retrieve when they need it.&#x20;

## Signed images

It is possible to use the Docker the framework to create signed images. When using signed image the node can verify the author of the algorithm image adding an additional protection layer.&#x20;

Dockerfile

* Build project
* CMD
* Expose

Harbor or Docker hub or whatever

public vs private

signed




# Client

The client communicates with the central server. The server hosts an API which can be used for this purpose. To see the API documentation you can use the `/apidocs` page (e.g. [https://petronas.vatage6.ai/apidocs](https://petronas.vatage6.ai/apidocs)).&#x20;

We have written two client libraries: one for R and one for Python. The R client currently only supports creating tasks and retrieving their results.&#x20;

#### Python client library

Before you install the Python client, we recommended to check the version of the server you are going to interact with first. The easiest way of doing that is checking the `/version` endpoint of the server you are going to use:

{% swagger method="get" path="/version" baseUrl="https://SERVER[/api_path]" summary="Retrieve version information of the server" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

Then you can install the `vantage6-client` with:

```
pip install vantage6==VERSION
```

**R client library**

You can install the R client by running:

```r
devtools::install_github('IKNL/vtg.basic', subdir='src')
```

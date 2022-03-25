# Wrappers

The algorithm wrapper simplifies and standardizes the interaction between algorithm and node. The [client libraries](../../usage/running-analyses/#client-libraries) and the algorithm wrapper are tied together and use the same standards. The algorithm wrapper:

* reads the environment variables and file mounts and supplies these to your algorithm.&#x20;
* provides an [entrypoint ](https://docs.docker.com/engine/reference/builder/#entrypoint)for the docker container
* allows to write a single algorithm for multiple types of data sources

The wrapper is language specific and currently we support Python and R. Extending this concept to other languages is not so complex.

![\[Image needs to be updated to the format\]](<../../.gitbook/assets/image (3).png>)

## Federated functions

The signature of your function has to contain `data` as the first argument. The method name should have a `RPC_` prefix. Everything that is returned by the function will be written to the output file.&#x20;

{% tabs %}
{% tab title="Python" %}
```python
def RPC_my_algorithm(data, *args, **kwargs): 
    pass
```
{% endtab %}

{% tab title="R" %}
```r
RPC_my_algorithm <- function(data, ...) {
}
```
{% endtab %}
{% endtabs %}

## Central functions

It is quite common to have a central part of your federated analysis which orchestrates the algorithm and combines the partial results. A common pattern for a central function would be:

1. Request partial models from all participants
2. Obtain the partial models
3. Combine the partial models to a global model
4. (optional) Repeat step 1-3 until the model converges

It is possible to run the central part of the analysis on your own machine, but it is also possible to let vantage6 handle the central part. There are several advantages to letting vantage6 handle this:

* You don't have to keep your machine running during the analysis
* You don't need to use the same programming language as the algorithm in case a language specific serialization is used in the algorithm

{% hint style="success" %}
Note that central functions also run at a node and not at the server.&#x20;
{% endhint %}

In contrast to the federated functions, central functions are not prefixed. The first argument needs to be `client` and the second argument needs to be `data`.  The `data` argument contains the local data and the `client` argument provides an interface to the vantage6-server.&#x20;

{% hint style="danger" %}
The argument data is not present in the R-wrapper. This is a consistency issue which will be solved in a future release.
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
def main(client, data, *args, **kwargs):
   # Run a federated function. Note that we omnit the 
   # RPC_ prefix. This prefix is added automatically
   # by the infrastructure
   task = client.create_new_task(
      {
         "method": "my_algorithm",
         "args": [],
         "kwargs": {}
      },
      organization_ids=[...]
   )
    
    # wait for the federated part to complete 
    # and return
    results = wait_and_collect(task)
    
    return results
```
{% endtab %}

{% tab title="R" %}
```r
main <- function(client, ...) {
    # Run a federated function. Note that we omnit the 
    # RPC_ prefix. This prefix is added automatically
    # by the infrastructure
    result <- client$call("my_algorithm", ...)
    
    # Optionally do something with the results
    
    # return the results
    return(result)
}
```


{% endtab %}
{% endtabs %}

## Different wrappers

The docker wrappers read the local data source and supplies this to your functions in your algorithm. Currently CSV and SPARQL for Python and a CSV wrapper for R is supported. Since the wrapper handles the reading of the data, you need to rebuild your algorithm with a different wrapper to make it compatible with a different type of data source. You do this by updating the `CMD` directive in the dockerfile.

{% tabs %}
{% tab title="Python SPARQL" %}
{% code title="Dockerfile" %}
```docker
...
CMD python -c "from vantage6.tools.docker_wrapper import sparql_wrapper; sparql_wrapper('${PKG_NAME}')"
```
{% endcode %}


{% endtab %}

{% tab title="Python CSV" %}
{% code title="Dockerfile" %}
```python
...
CMD python -c "from vantage6.tools.docker_wrapper import docker_wrapper; docker_wrapper('${PKG_NAME}')"
```
{% endcode %}
{% endtab %}

{% tab title="R CSV" %}
{% code title="Dockerfile" %}
```r
...
CMD Rscript -e "vtg::docker.wrapper('$PKG_NAME')"
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Data serialization

TODO

# R Client

It is assumed you installed the [vantage6-client](../../installation/client.md). The R client can create tasks and retrieve their results. If you want to do more administrative tasks, either use the API directly or use the [python-client.md](python-client.md "mention").

Initialization of the R client can be done by:

```r
setup.client <- function() {
  # Username/password should be provided by the administrator of
  # the server.
  username <- "username@example.com"
  password <- "password"
  
  host <- 'https://petronas.vantage6.ai:443'
  api_path <- ''
  
  # Create the client & authenticate
  client <- vtg::Client$new(host, api_path=api_path)
  client$authenticate(username, password)

  return(client)
}

# Create a client
client <- setup.client()
```

Then this client can be used for the different algorithms. Refer to the README in the repository on how to call the algorithm. Usually this includes installing some additional client-side packages for the specific algorithm you are using.

{% hint style="warning" %}
The R client is subject to change. We aim to make it more similar to the Python client.
{% endhint %}

## Example

First you need to install the client side of the algorithm by:

```r
devtools::install_github('iknl/vtg.coxph', subdir="src")
```

This is the code to run the coxph:

```r
print( client$getCollaborations() )

# Should output something like this:
#   id     name
# 1  1 ZEPPELIN
# 2  2 PIPELINE

# Select a collaboration
client$setCollaborationId(1)

# Define explanatory variables, time column and censor column
expl_vars <- c("Age","Race2","Race3","Mar2","Mar3","Mar4","Mar5","Mar9",
               "Hist8520","hist8522","hist8480","hist8501","hist8201",
               "hist8211","grade","ts","nne","npn","er2","er4")
time_col <- "Time"
censor_col <- "Censor"

# vtg.coxph contains the function `dcoxph`.
result <- vtg.coxph::dcoxph(client, expl_vars, time_col, censor_col)
```

---
description: Interacting with the server API
---

# Using the Client

To interact with the _vantage6_ server there is a Python and R client in development. Keep in mind that these Clients are could change with every release and that the Python and R client are quite different. 

There are basically two reasons to use the Client:

* You want to manage users 👥, organizations 🏢, collaborations 🤝, etc. 🖱 
* You want to request a computation 🧮 and collect their results 🧪 

The R client is currently only able to create computation requests and retrieve their results. The Python-client is also able to manage users, roles, collaborations, etc.

Besides using a client, it is also possible to use the API from the server directly. The API docs can be found [here](https://harukas.vantage6.ai/apidocs).

{% hint style="info" %}
We are also working on a UI to manage users, roles, rules, etc. at the server, see [https://github.com/iknl/vantage6-ui](https://github.com/iknl/vantage6-ui)
{% endhint %}

## 📔 Administration

You can use the _vantage6_ server API to manage these. To make this process easier the Python _vantage6-client_ can be used. The R client does not have these tools \(yet\). Keep in mind that you need permission for _all_ operations you perform. 

{% page-ref page="python-client.md" %}

{% hint style="info" %}
As the root user you can use the [iPython shell](../running-the-server/shell.md) to manage users, organizations, collaborations, etc. _Regular_ users do not have access to this shell. 
{% endhint %}

## 🔭 Computation Requests and Results

This is the core task of vantage6. Receiving tasks \(computation requests\) and collecting the results from the data-station. Both the Python and R client are able to help you perform these tasks. To give you a little bit of intuition what happens under the hood we outline the process here:

1. User requests computation, consisting of:
   * The algorithm to use \(as a Docker image 🐳 name\)
   * Any input you want to provide to the algorithm
   * Which collaboration, and organizations to include
2. Nodes receive and execute the request
   * The algorithm will have access to:
     * The \(local\) node's data
     * The input provided by the user
   * The node returns the results
3. User collect the results from the server

In step \(1\) and \(3\) the user needs to do something. This is where our clients helps.

{% page-ref page="python-client.md" %}

{% page-ref page="r-client.md" %}

## So how, then?

As a researcher you would normally fire up your \(least\) favorite programming language and do something like this if you'd want to check which columns are in your data:

{% tabs %}
{% tab title="R" %}
```r
# This assumes the package devtools is installed:
devtools::install_github('mellesies/vtg.basic', subdir='src')

# Load the SEER dataset located in the vtg.basic package
data('SEER', package='vtg.basic')

# Print all the column names in the dataset
print( colnames(SEER) )

# Expected output:
# [1] "Age"      "Race2"    "Race3"    "Mar2"     "Mar3"     "Mar4"     "Mar5"    
# [8] "Mar9"     "Hist8520" "hist8522" "hist8480" "hist8501" "hist8201" "hist8211"
#[15] "grade"    "ts"       "nne"      "npn"      "er2"      "er4"      "Time"    
#[22] "Censor"  # 
```
{% endtab %}
{% endtabs %}

In a federated situation, you won't have direct access to the data. Instead, you'd have to instruct the nodes to run a docker image that returns the list of column names. Since we'd also have to communicate with the infrastructure, we need two things:

* A Docker image \(with software that returns the column names\)
* A client to facilitate communication

Fortunately, a docker image that returns is already available and using it is not too difficult:

{% tabs %}
{% tab title="R" %}
```r
# This assumes the package 'devtools' is installed and will automatically
# install the package 'vtg'.
devtools::install_github('mellesies/vtg.basic', subdir='src')

# Function to create a client
setup.client <- function() {
  # Username/password should be provided by the administrator of
  # the server.
  username <- "username@example.com"
  password <- "password"
  
  host <- 'https://trolltunga.vantage6.ai'
  api_path <- ''
  
  # Create the client & authenticate
  client <- vtg::Client$new(host, api_path=api_path)
  client$authenticate(username, password)

  return(client)
}

# Create a client
client <- setup.client()

# Get a list of available collaborations
print( client$getCollaborations() )

# Should output something like this:
#   id     name
# 1  1 ZEPPELIN
# 2  2 PIPELINE

# Instruct the client to use collaboration "PIPELINE".
client$setCollaborationId(2)


# Since vtg.basic exports function names that collide with built-in functions, 
# it's probably better to not attach the package, but call functions with a prefix instead. 
vtg.basic::colnames(client)

# Should output something like this:
# [[1]]
#  [1] "Age"      "Race2"    "Race3"    "Mar2"     "Mar3"     "Mar4"     "Mar5"    
#  [8] "Mar9"     "Hist8520" "hist8522" "hist8480" "hist8501" "hist8201" "hist8211"
# [15] "grade"    "ts"       "nne"      "npn"      "er2"      "er4"      "Time"    
# [22] "Censor"  
# 
# [[2]]
#  [1] "Age"      "Race2"    "Race3"    "Mar2"     "Mar3"     "Mar4"     "Mar5"    
#  [8] "Mar9"     "Hist8520" "hist8522" "hist8480" "hist8501" "hist8201" "hist8211"
# [15] "grade"    "ts"       "nne"      "npn"      "er2"      "er4"      "Time"    
# [22] "Censor"  
```
{% endtab %}
{% endtabs %}


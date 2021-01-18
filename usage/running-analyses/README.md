---
description: Here's what's to it ...
---

# Running analyses

## A little background of what's happening under the hood

As explained in previous sections, running an algorithm consists of the following steps:

* Send a task to the nodes in your collaboration, consisting of:
  * The docker image you want the nodes to run \(the docker image contains the algorithm you're interested in\)
  * Any input you might want to provide to the docker image
* Each node will execute the docker image. 
  * The code/algorithm in the docker image will have access to:
    * The node's data
    * The input that was provided in the task
  * The node returns the result returned by the algorithm

Of course, there are few people that would actually like to run a regression \(or another statistical analysis\) this way. Therefore, the algorithms that have been written for the infrastructure have been packaged in such a way that using them is manageable.

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

{% tab title="Python" %}
```
import vantage6.client as vtgclient


# Setup local host
HOST = 'http://vserver'
PORT = 5000
IMAGE = 'harbor2.vantage6.ai/testing/v6-test-py'

USERNAME = 'yourusername'
PASSWORD = 'yourpassword'

# ID of a collaboration that includes your organization
COLLABORATION_ID = 1 

# IDs of the organizations where you wish to run
# the task. They need to be part of the
# collaboration.
ORGANIZATION_IDS = [1, 2, 3]

client = vtgclient.Client(HOST, PORT)
client.authenticate(USERNAME, PASSWORD)

client.setup_encryption(None)

# If you have configured end-to-end encryption between nodes you need to
# uncomment the following line and fill in the path to your rsa private key
# client.setup_encryption('path/to/rsa_private_key')

# Retrieve the colummn names of the datasets at all datastations
task = client.post_task('mytask', image='harbor2.vantage6.ai/testing/v6-test-py', 
                        collaboration_id=COLLABORATION_ID,
                        organization_ids=ORGANIZATION_IDS,
                        input_={'method': 'column_names'})

```
{% endtab %}
{% endtabs %}


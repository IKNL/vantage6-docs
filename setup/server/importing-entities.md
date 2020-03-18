---
description: 'Batch import many organizations, users, collaborations, etc...'
---

# Importing Entities

There are three ways to get data into the server:

1. Through the RESTful API \(you need to have a user first\)
2. Through the Python API
3. The `vserver import` FILE command.

This section will explain how to import multiple entities at once using the third option.

The `vserver import` command expects a path to a YAML file containing the entities you want to import. The expected structure of this YAML-file is as follows:

{% tabs %}
{% tab title="YAML" %}
{% code title="example\_fixtures.yaml" %}
```yaml
organizations:
  
  - name:       IKNL
    domain:     iknl.nl
    address1:   Godebaldkwartier 419
    address2:   
    zipcode:    3511DT
    country:    Netherlands
    users:
      - username: root
        firstname: root
        lastname: robot
        password: password
        roles: "root"
      - ...      
      
    public_key: base64-encoded-public-key
  - ...

collaborations:

  - name: ZEPPELIN
    participants:
      - IKNL
      - ...
    tasks: ["hello-world"]
  
  - ...
```
{% endcode %}
{% endtab %}
{% endtabs %}

To add them to the database, simply run:

```text
vserver import example_fixtures.yaml
```

To change or update one of the entities, you should either use the RESTful API or the [Python API](shell.md).


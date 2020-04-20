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
      - username: admin
        firstname: admin
        lastname: robot
        password: password
        roles: "root"
      - username: frank@iknl.nl
        firstname: Frank
        lastname: Martin
        password: password
        roles: "admin"
      - username: melle@iknl.nl
        firstname: Melle
        lastname: Sieswerda
        password: password
        roles: "researcher"
    public_key: LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FnOEFNSUlDQ2dLQ0FnRUF2eU4wWVZhWWVZcHVWRVlpaDJjeQphTjdxQndCUnB5bVVibnRQNmw2Vk9OOGE1eGwxMmJPTlQyQ1hwSEVGUFhZQTFFZThQRFZwYnNQcVVKbUlseWpRCkgyN0NhZTlIL2lJbUNVNnViUXlnTzFsbG1KRTJQWDlTNXVxendVV3BXMmRxRGZFSHJLZTErUUlDRGtGSldmSEIKWkJkczRXMTBsMWlxK252dkZ4OWY3dk8xRWlLcVcvTGhQUS83Mm52YlZLMG9nRFNaUy9Jc1NnUlk5ZnJVU1FZUApFbGVZWUgwYmI5VUdlNUlYSHRMQjBkdVBjZUV4dXkzRFF5bXh2WTg3bTlkelJsN1NqaFBqWEszdUplSDAwSndjCk80TzJ0WDVod0lLL1hEQ3h4eCt4b3cxSDdqUWdXQ0FybHpodmdzUkdYUC9wQzEvL1hXaVZSbTJWZ3ZqaXNNaisKS2VTNWNaWWpkUkMvWkRNRW1QU29rS2Y4UnBZUk1lZk0xMWtETTVmaWZIQTlPcmY2UXEyTS9SMy90Mk92VDRlRgorUzVJeTd1QWk1N0ROUkFhejVWRHNZbFFxTU5QcUpKYlRtcGlYRWFpUHVLQitZVEdDSC90TXlrRG1JK1dpejNRCjh6SVo1bk1IUnhySFNqSWdWSFdwYnZlTnVaL1Q1aE95aE1uZHU0c3NpRkJyUXN5ZGc1RlVxR3lkdE1JMFJEVHcKSDVBc1ovaFlLeHdiUm1xTXhNcjFMaDFBaDB5SUlsZDZKREY5MkF1UlNTeDl0djNaVWRndEp5VVlYN29VZS9GKwpoUHVwVU4rdWVTUndGQjBiVTYwRXZQWTdVU2RIR1diVVIrRDRzTVQ4Wjk0UVl2S2ZCanU3ZXVKWSs0Mmd2Wm9jCitEWU9ZS05qNXFER2V5azErOE9aTXZNQ0F3RUFBUT09Ci0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=

  - name:       Small Organization
    domain:     small-organization.example
    address1:   Big Ambitions Drive 4
    address2:   
    zipcode:    1234AB
    country:    Nowhereland
    users:
      - username: admin@small-organization.example
        firstname: admin
        lastname: robot
        password: password
        roles: "admin"
      - username: stan
        firstname: Stan
        lastname: the man
        password: password
        roles: "user"
    public_key: LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FnOEFNSUlDQ2dLQ0FnRUF2eU4wWVZhWWVZcHVWRVlpaDJjeQphTjdxQndCUnB5bVVibnRQNmw2Vk9OOGE1eGwxMmJPTlQyQ1hwSEVGUFhZQTFFZThQRFZwYnNQcVVKbUlseWpRCkgyN0NhZTlIL2lJbUNVNnViUXlnTzFsbG1KRTJQWDlTNXVxendVV3BXMmRxRGZFSHJLZTErUUlDRGtGSldmSEIKWkJkczRXMTBsMWlxK252dkZ4OWY3dk8xRWlLcVcvTGhQUS83Mm52YlZLMG9nRFNaUy9Jc1NnUlk5ZnJVU1FZUApFbGVZWUgwYmI5VUdlNUlYSHRMQjBkdVBjZUV4dXkzRFF5bXh2WTg3bTlkelJsN1NqaFBqWEszdUplSDAwSndjCk80TzJ0WDVod0lLL1hEQ3h4eCt4b3cxSDdqUWdXQ0FybHpodmdzUkdYUC9wQzEvL1hXaVZSbTJWZ3ZqaXNNaisKS2VTNWNaWWpkUkMvWkRNRW1QU29rS2Y4UnBZUk1lZk0xMWtETTVmaWZIQTlPcmY2UXEyTS9SMy90Mk92VDRlRgorUzVJeTd1QWk1N0ROUkFhejVWRHNZbFFxTU5QcUpKYlRtcGlYRWFpUHVLQitZVEdDSC90TXlrRG1JK1dpejNRCjh6SVo1bk1IUnhySFNqSWdWSFdwYnZlTnVaL1Q1aE95aE1uZHU0c3NpRkJyUXN5ZGc1RlVxR3lkdE1JMFJEVHcKSDVBc1ovaFlLeHdiUm1xTXhNcjFMaDFBaDB5SUlsZDZKREY5MkF1UlNTeDl0djNaVWRndEp5VVlYN29VZS9GKwpoUHVwVU4rdWVTUndGQjBiVTYwRXZQWTdVU2RIR1diVVIrRDRzTVQ4Wjk0UVl2S2ZCanU3ZXVKWSs0Mmd2Wm9jCitEWU9ZS05qNXFER2V5azErOE9aTXZNQ0F3RUFBUT09Ci0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=
      
  - name:       Big Organization
    domain:     big-organization.example
    address1:   Offshore Accounting Drive 19
    address2:   
    zipcode:    54331
    country:    Nowhereland
    users:
      - username: admin@big-organization.example
        firstname: admin
        lastname: robot
        password: password
        roles: "admin"
    public_key: LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FnOEFNSUlDQ2dLQ0FnRUF2eU4wWVZhWWVZcHVWRVlpaDJjeQphTjdxQndCUnB5bVVibnRQNmw2Vk9OOGE1eGwxMmJPTlQyQ1hwSEVGUFhZQTFFZThQRFZwYnNQcVVKbUlseWpRCkgyN0NhZTlIL2lJbUNVNnViUXlnTzFsbG1KRTJQWDlTNXVxendVV3BXMmRxRGZFSHJLZTErUUlDRGtGSldmSEIKWkJkczRXMTBsMWlxK252dkZ4OWY3dk8xRWlLcVcvTGhQUS83Mm52YlZLMG9nRFNaUy9Jc1NnUlk5ZnJVU1FZUApFbGVZWUgwYmI5VUdlNUlYSHRMQjBkdVBjZUV4dXkzRFF5bXh2WTg3bTlkelJsN1NqaFBqWEszdUplSDAwSndjCk80TzJ0WDVod0lLL1hEQ3h4eCt4b3cxSDdqUWdXQ0FybHpodmdzUkdYUC9wQzEvL1hXaVZSbTJWZ3ZqaXNNaisKS2VTNWNaWWpkUkMvWkRNRW1QU29rS2Y4UnBZUk1lZk0xMWtETTVmaWZIQTlPcmY2UXEyTS9SMy90Mk92VDRlRgorUzVJeTd1QWk1N0ROUkFhejVWRHNZbFFxTU5QcUpKYlRtcGlYRWFpUHVLQitZVEdDSC90TXlrRG1JK1dpejNRCjh6SVo1bk1IUnhySFNqSWdWSFdwYnZlTnVaL1Q1aE95aE1uZHU0c3NpRkJyUXN5ZGc1RlVxR3lkdE1JMFJEVHcKSDVBc1ovaFlLeHdiUm1xTXhNcjFMaDFBaDB5SUlsZDZKREY5MkF1UlNTeDl0djNaVWRndEp5VVlYN29VZS9GKwpoUHVwVU4rdWVTUndGQjBiVTYwRXZQWTdVU2RIR1diVVIrRDRzTVQ4Wjk0UVl2S2ZCanU3ZXVKWSs0Mmd2Wm9jCitEWU9ZS05qNXFER2V5azErOE9aTXZNQ0F3RUFBUT09Ci0tLS0tRU5EIFBVQkxJQyBLRVktLS0tLQo=

collaborations:

  - name: ZEPPELIN
    participants:
      - IKNL
      - Small Organization
      - Big Organization
    tasks: ["hello-world"]
  
  - name: PIPELINE
    participants:
      - IKNL
      - Big Organization
    tasks: ["hello-world"]

  - name: SLIPPERS
    participants:
      - Small Organization
      - Big Organization
    tasks: ["hello-world", "hello-world"]

```
{% endcode %}
{% endtab %}
{% endtabs %}

To add these objects to the database, simply run:

```text
vserver import example_fixtures.yaml
```

To change or update one of the entities, you should either use the RESTful API or the [Python API](shell.md).


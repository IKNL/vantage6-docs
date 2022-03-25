---
description: In this section we'll explain how to deploy a vantage6 server.
---

# Deployment

vantage6 uses Flask as backbone, together with flask-socketio for websocket support. The server runs as a standalone process (listening on its own ip address/port).

{% hint style="info" %}
Because there is no message broker used for the websocket channel, it is currently not possible to horizontally scale the vantage6-server
{% endhint %}

There are many deployment options, so these examples are not complete and exhaustive.

* [NGINX](deployment.md#nginx)
* [Azure app service](deployment.md#undefined)
* ...

## NGINX

Below a basic setup. Note that SSL is not configured in this example.

```nginx
server {
    
    # public port 
    listen 80;
    server_name _;

    # vantage6-server 
    location / {
        include proxy_params;
        
        # internal ip and port 
        proxy_pass http://127.0.0.1:5000;
    }
    
    # allow the websocket traffic
    location /socket.io {
        include proxy_params;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_pass http://127.0.0.1:5000/socket.io;
    }
}
```

## Azure app service&#x20;

TODO


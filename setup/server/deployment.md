---
description: In this section we'll explain how to deploy a VANTAGE6 server.
---

# Deployment

VANTAGE6 uses Flask as backbone. However in the current VANTAGE6 version we also make use of a web-socket interface. Therefore we use the deployment options of `flask-socketio` which can be found [here](https://flask-socketio.readthedocs.io/en/latest/#deployment).

{% hint style="warning" %}
There is currently no support for multiple workers, this will be included in a later release.
{% endhint %}

## NGINX

Below a basic setup. Note that SSL is not configured in this example

```javascript
server {
    listen 80;
    server_name _;

    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:5000;
    }

    location /static {
        alias <path-to-your-application>/static;
        expires 30d;
    }

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

TODO:

* ssl certificates \(lets encrypt\)
* managing multiple servers \(screen\)
* ...


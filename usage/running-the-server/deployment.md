---
description: In this section we'll explain how to deploy a vantage6 server.
---

# Deployment

**vantage6** uses Flask as backbone, together with flask-socketio for websocket support. The server runs as a standalone process \(listening on its own ip address/port\), either as a regular process or as a dockerized service. 

While this is fine for development, when hosting a server, this can become bothersome. For example, a webserver might already running on the same port. Therefore, we recommend running the **vantage6** server behind NGINX, a webserver that can also proxy requests and websocket connections.

The server is first started as you normally would. To keep the server running when you log out, either use the dockerized version or use an application like `tmux` or `screen`. Then NGINX is configured to listen on the public interface and proxy incoming requests.

## NGINX Configuration

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


```conf

    upstream backend {
        server 192.168.0.201:8998;
    }

    upstream log_backend {
        server 192.168.0.201:8999;
    }

    server {
        listen       80;
        server_name  _;
        location / {
                mirror /mirror;
                mirror_request_body on;
                proxy_pass http://backend;
        }

        location = /mirror {
                internal;
                proxy_pass http://log_backend$request_uri;
                proxy_set_header X-SERVER-PORT $server_port;
                proxy_set_header X-SERVER-ADDR $server_addr;
                proxy_set_header X-Original-URI $request_uri;
                proxy_set_header HOST $http_host;
                proxy_set_header X-REAL-IP $remote_addr;
        }
    }
```

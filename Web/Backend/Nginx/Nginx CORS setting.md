
```nginx
server {
    listen 80;
    server_name ec2-52-69-33-14.ap-northeast-1.compute.amazonaws.com;
    client_max_body_size 20M;

    location / {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $http_host;
        proxy_pass       "http://127.0.0.1:8080";
    }

    location /uploads/ {
        alias /home/ubuntu/uploads/;
        autoindex on;
        try_files $uri $uri/ =404;

        # Add CORS headers
        if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
            add_header 'Access-Control-Max-Age' 86400;
            add_header 'Content-Length' 0;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            return 204;
        }

        if ($request_method = 'GET') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
        }
    }
}

```

### Configuration 
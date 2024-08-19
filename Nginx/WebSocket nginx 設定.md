
```nginx
server {
    listen 80;
    server_name stylish.monster;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name stylish.monster;

    ssl_certificate /home/ubuntu/stylish.monster/certificate.crt;
    ssl_certificate_key  /home/ubuntu/stylish.monster/private.key;

    location /chat {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    location / {
	proxy_pass http://localhost:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    error_page 404 /404.html;
    location = /404.html {
        root /usr/share/nginx/html;
    }
}
```

針對要進行 `websocket` 協議升級的端點配置 upgrade header。讓伺服器端可以正確地接收到要進行升級的請求


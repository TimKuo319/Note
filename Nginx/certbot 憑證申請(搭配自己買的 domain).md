
### Certbot 安裝

```sh
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

### 使用 Certbot 為 Domain 申請憑證(自動配置 nginx 設定檔案)

- 先到自己的 DNS 服務商將 A record 指向自己要的 server。這樣才能成功被解析到

```sh
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

- `-d` : 指定 domain name


### 使用 Certbot 為 Domain 申請憑證(僅進行憑證申請)

```sh
sudo certbot certonly --nginx -d your-domain.com -d www.your-domain.com
```

在 `site-available` 中進行檔案配置

```nginx
server {
    listen 80;
    server_name masternode.gitmazon.com;

    location / {
        proxy_pass http://localhost:8082;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name masternode.gitmazon.com;

    ssl_certificate /etc/letsencrypt/live/masternode.gitmazon.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/masternode.gitmazon.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://localhost:8082;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

- 透過 symbol link 在 `sites-enabled` 中建立連結

```
sudo ln -s /etc/nginx/sites-available/masternode.gitmazon.com /etc/nginx/sites-enabled/
```

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
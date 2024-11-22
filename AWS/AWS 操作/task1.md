
## godaddy購買

1. 購買域名
2. 到網域 -> DNS -> 新增Ａ記錄到DNS中
3. 刪除godaddy partked

## ZeroSSL證書申請

+ 申請憑證
+ 依照文件配置nginx

## nginx設定

+ 輸入`www.domainName` ，透過轉址讓網址變成`domainName`


```nginx.conf
server {
    listen 80;


    server_name stylish.monster;
    client_max_body_size 20M;

    location / {
	proxy_pass http://localhost:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

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

    return 301 https://$host$request_uri;
}


server{

    listen 443 ssl;

    ssl_certificate /home/ubuntu/stylish.monster/certificate.crt;
    ssl_certificate_key  /home/ubuntu/stylish.monster/private.key;

    server_name stylish.monster;
    access_log /var/log/nginx/nginx.vhost.access.log;
    error_log /var/log/nginx/nginx.vhost.error.log;

    location / {

	proxy_pass http://localhost:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

    }
}
```

+ https://manage.sslforfree.com/certificate/install/7284fc2e34f19c4a16c9e37431efb506


## Reference
https://medium.com/@anthea.ensui/web-nginx-%E5%BE%9E%E5%85%A5%E9%96%80%E9%96%8B%E5%A7%8B%E7%9A%84%E7%B6%B2%E7%AB%99%E6%9E%B6%E8%A8%AD-3-5b0a08239bb7

https://help.zerossl.com/hc/en-us/articles/360058295894-Installing-SSL-Certificate-on-NGINX

https://www.spatialgeolab.com/freessl-zerossl-apache/

https://jakevin.medium.com/godaddy-%E8%88%87-zerossl-dns%E9%A9%97%E8%AD%89%E6%AD%A5%E9%A9%9F-a7c300fc2593


## to learn

A紀錄
CNAME紀錄

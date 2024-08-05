
## Configuration

1.  在`/etc/sites-available`中建立configuration檔案

2. 撰寫`nginx檔案`
```nginx
server {
    listen 80;
    server_name <domain_name or IP>;

    location / {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $http_host;
        proxy_pass       "http://127.0.0.1:8080";
    }
}
``` 

+ `$remote_addr` - 客戶端的IP
+ `$http_host` - 客戶端的domainName或serverName


3. 建立softlink到`/etc/sites-enabled`
```sh
sudo ln -s /etc/nginx/sites-available/node /etc/nginx/sites-enabled/node
```


4. 重啟nginx server
```sh
sudo service nginx restart
```

## How Nginx load Config File


```nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

	    ##.....
        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

	    ##.....

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*; <---------------------
}
....
```


從預設的`nginx.conf`中看到，有一行`include /etc/nginx/sites-enabled/*`，這就可以幫助`nginx`去讀取我們寫在`/etc/nginx/sites-enabled`中的設定檔。

而前面[Configuration](## Configuration)中的第三步要設定連結的原因是因為，我們撰寫的`.conf`檔案其實在`etc/nginx/sites-available`中，為了要讓nginx讀取到，我們還需要建立一個link在`/etc/nginx/enabled`中，讓nginx讀取到`.conf`檔。

## Reference

https://eladnava.com/binding-nodejs-port-80-using-nginx/

https://nginx.org/en/docs/http/ngx_http_core_module.html#var_remote_addr

[系統設計 - 正向代理跟反向代理 · jyt0532's Blog](https://www.jyt0532.com/2019/11/18/proxy-reverse-proxy/)
#Nginx 

在透過像是 vue 或 react 這類前端框架產生的前端通常會被稱作 SPA (Single Page Application) ，而這樣的應用在最終部署到正式環境的時候都會包成一個 `html` `css` `js`的 folder ，通常叫做 `/dist`。

而這就會導致一個問題，因為前端使用像是 router 這類來進行頁面切換的路徑都是框架所產生的`虛擬路徑`，所以最終在打包後，如果是透過 nginx 去輸入原先在 router 中的路徑，像以下這樣。 

這樣在部署到 nginx 上的時候輸入 `／calendar` 就會報錯，因為 nginx 預設會去 `/usr/share/nginx/html` 下找檔案，而這個目錄下只會有我們放上去的 `/dist` 沒有 `calendar` 這個資料夾。

![[router_path.png]]


### Solution

解決辦法如下，透過直接將 `root` 與 `index` 移動到 server block 的最上層。並搭配 `try-files` ，因為 nginx 在 location 區塊找不到對應的 end point 時，會自動去尋找第一個 block，而因為 `try_files` 的關係，在最後找不到對應的檔案時，就會回傳 index.html，來成功的渲染畫面。

```nginx.conf
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    root   /usr/share/nginx/html;
    index  index.html index.html;

    location / {
	try_files $uri $uri/ /index.html;
    }

    location /events {
        proxy_pass http://backend:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```


## todo 

- root index try_files 的作用



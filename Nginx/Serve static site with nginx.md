
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


	//key point here
    location /uploads/ {
        alias /home/ubuntu/uploads/;
        autoindex on;
        try_files $uri $uri/ =404;
    }
}
```

在你的`.conf`檔案中配置指定的路徑，讓nginx能夠將對應的url指向機器上的`指定路徑`

+ 當nginx被指向一個目錄且沒有指定檔案時，預設會尋找`index.html`檔案是否存在
+ `try_files`
	+ 允許nginx按順序測試不同的路徑
	+ 上面的狀況指的是測試`uri`以及該uri下的目錄
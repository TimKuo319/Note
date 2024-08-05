
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

#### 處理來自browser的預檢請求

```nginx
if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
    add_header 'Access-Control-Max-Age' 86400;
    add_header 'Content-Length' 0;
    add_header 'Content-Type' 'text/plain charset=UTF-8';
    return 204;
}

```

- `if ($request_method = 'OPTIONS')`: 檢查請求方法是否為OPTIONS。
    - **OPTIONS請求**：瀏覽器在發送實際請求之前，會先發送一個預檢請求（OPTIONS請求）來確定伺服器是否允許實際請求。這是CORS的一部分。
- `add_header 'Access-Control-Allow-Origin' '*'`: 允許所有域訪問資源。`*`表示任何域。
- `add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS'`: 指定允許的HTTP方法。此處允許GET、POST和OPTIONS方法。
- `add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization'`: 指定允許的HTTP請求頭。此處允許Content-Type和Authorization請求頭。
- `add_header 'Access-Control-Max-Age' 86400`: 指定預檢請求的結果可以快取的時間（以秒為單位）。此處為86400秒（24小時）。
- `add_header 'Content-Length' 0`: 設定回應的內容長度為0。
- `add_header 'Content-Type' 'text/plain charset=UTF-8'`: 設定回應的內容類型。
- `return 204`: 返回204狀態碼，表示請求已成功處理且無內容返回。204狀態碼適用於預檢請求。


#### 處理來自客戶端的Get請求

```nginx
if ($request_method = 'GET') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
}

```

- `if ($request_method = 'GET')`: 檢查請求方法是否為GET。
    - **GET請求**：實際請求，用於獲取資源。
- `add_header 'Access-Control-Allow-Origin' '*'`: 允許所有域訪問資源。`*`表示任何域。
- `add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS'`: 指定允許的HTTP方法。此處允許GET、POST和OPTIONS方法。
- `add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization'`: 指定允許的HTTP請求頭。此處允許Content-Type和Authorization請求頭。


## Summary

要處理兩個請求方式的原因是因為，瀏覽器在遇到跨域請求時會先發送一個預檢請求，要先通過預檢請求才可以處理client實際發出來真正的請求，這也是為何要處理兩個請求的原因。

CORS細節可見，[[同源政策與CORS]]

## todo

+ Access-Control-Allow-Methods vs Access-Controle-Allow-Headers差異

## What's inside nginx.conf

+ 文字後方加上{}代表一個context
	+ 以下方的例子來說就會被稱作`event context`
```nginx
events {
    worker_connections  1024;
}
```
+ directive
	+ `key-value pair` exist in nginx.conf


## Static page configuration

+ listen - nginx接收的port
+ root - 要serve的網站`根目錄`
+ ==在沒有特別配置的情況下，nginx預設會回傳目錄下的`index.html`==

```nginx
http{
	server{
		listen 8080;
		root /User/test/your_site
	}
}
```

## MIME Types

nginx在處理檔案的時候，預設情況下會將所有檔案的`content-type`都是為`text/plain`，而這樣的結果可能會造成我們的網站顯示不正常。所以在使用時`需要指定`type。

以下的例子來說，就是告訴nginx在serve檔案的時候，將`.css`以及`.html`的Content-Type轉換為`text/css`、`text/html`。
```nginx
http{
	type{
		text/css   css;
		text/html  html;
	}
}
```

然而檔案類型的種類太多了，所以nginx有提供一個包含了大多數檔案類型的檔案，稱為`MIMETYPE`，透過引入`mimetype`，我們就不用自己手打許多檔案類型。

```nginx
http{
	include mime.types
	server{
		...
	}
}
```

## Location Context

Location Context 主要是用來處理不同的路由的狀況，以/images/這個路徑來說，會將他的root directory設為/data，在處理請求時會將`location append到root後方`，所以實際存取檔案位置的時候是存取到`/data/images`

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        # 指定處理根目錄請求的規則
    }

    location /images {
        # 指定處理/images/目錄下的請求的規則
        root /data
    }

    location /api {
        # 指定處理/api/目錄下的請求的規則
    }
}

```

### alias

假設今天我希望另外一個api path也能指向image path，可以透過`alias`來達成，相較於`root`，使用`alias`時，==不會去加上location後方的path==。

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        # 指定處理根目錄請求的規則
    }

    location /images {
        # 指定處理/images/目錄下的請求的規則
        root /data
    }

    location /api {
        # 指定處理/api/目錄下的請求的規則
        alias /data/image
    }
}

```

### try_files

透過try_files可以在找不到`index.html`檔案時，指定其他尋找的檔案，以下方來說，當在`/image`下找不到`index.html`時，就會去尋找另外兩個檔案，都找不到時，就會丟出`404 error`

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        # 指定處理根目錄請求的規則
    }

    location /images {
        # 指定處理/images/目錄下的請求的規則
        root /data
    }

    location /api {
        # 指定處理/api/目錄下的請求的規則
        alias /data/image
        try_files /image/image.html /index.html =404
    }
}

```

### redirect

前方是`http status code`，後方是要redirect的目錄, ==會將url跳轉到目標api==

```nginx
server{
	location /crop{
		return 307 /fruits;
	}
}
```

### rewrite

rewrite一樣可以做到redirect的效果，但不會改變url
```nginx
server {
	rewrite /crop /fruits;
}
```

## Load Balance

透過`round robin`的方式將請求轉發到不同的server。

+ `upstream`
	+ 指定請求要去的一組server list
+ `proxy_pass`
	+ 將對應到此endpoint的請求轉發到upstream中

```nginx
http{
	upstrem backendserver {
		server 127.0.0.1:1111
		server 127.0.0.1:2222
		server 127.0.0.1:3333
		server 127.0.0.1:4444
	};

	server{
		location / {
			proxy_pass http://backendserver/;
		};
	}
}
```

以上面的例子來說，nginx扮演的角色就是`loadbalancer`，有關請求被發到server後的路由是如何進行的就是那些server需要自行處理的事情。

如果是在`nginx.conf`中配置特定的`endpoint`以及要serve的檔案，則是將nginx作為`webserver`在做使用。
## Nginx 基本指令

(todo)

[安裝Nginx on Docker | Jennifer的Docker筆記本](https://cutejaneii.gitbook.io/docker/docker-installation/an-zhuang-nginx-on-docker)

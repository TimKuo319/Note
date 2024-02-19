---
date: 2024-02-19 Mon 9:22
---
---

# Outline

docker-compose是用來整合容器、虛擬網路、volume並且構成應用的一個工具

+ [檔案架構](##檔案架構)
	+ [top-level參數](###top-level參數)
		+ [Service參數](####Service參數)
		+ [networks跟volumes參數](####networks跟volumes參數)
+ [yml及yaml](##yml及yaml)
+ [build](##build)
+ [擴充欄位](##擴充欄位)
+ [檔案覆寫](##檔案覆寫)
+ [Reference](##Reference)
## 檔案架構
```yml
version: '3.9'

services:

  ui:

    image: robeeerto/todo-list-ui:latest

    container_name: ui

    restart: on-failure

    networks:

      - frontend

    ports:

      - 3001:3001

    environment:

      - NEXT_PUBLIC_CABLE_URL=${NEXT_PUBLIC_CABLE_URL}

      - NEXT_PUBLIC_API=${NEXT_PUBLIC_API}

    depends_on:

      - api

  

  api:

    image: robeeerto/todo-list-api:latest

    restart: on-failure

    container_name: api

    ports:

      - 3000:3000

    depends_on:

      - database

      - redis

    networks:

      - frontend

      - backend

    environment:

      - DB_HOST=database

      - DB_USER=${DB_USER}

      - DB_PORT=${DB_PORT}

      - DB_PASSWORD=${DB_PASSWORD}

      - RAILS_ENV=${RAILS_ENV}

      - REDIS_URL=${REDIS_URL}

  

  redis:

    image: redis:7-alpine

    container_name: redis

    restart: on-failure

    networks:

      - backend

    volumes:

      - redis-data:/data

  

  database:

    image: postgres:14-alpine

    container_name: database

    restart: on-failure

    networks:

      - backend

    environment:

      - POSTGRES_PASSWORD=${DB_PASSWORD}

    volumes:

      - database-data:/var/lib/postgresql/data

volumes:

  database-data:

    external: true

  redis-data:

    external: true

  

networks:

  backend:

    external: true

  frontend:

    external: true

```

### Top-level參數

top-level參數主要有三個，分別是:
+ service
	+ 在其中定義要啟動的容器，透過在service內宣告需要的參數在docker-compose時一併啟動
+ volume
	+ 宣告會在service內被呼叫到的volume
+ networks
	+ 宣告會在service內被呼叫到的network

#### Service參數

service內部會填入的參數大多就是在運行單一容器時會填的參數，如`name`、`image`等等。
+ *image*: 指定該服務使用的image，`ex image: postgres:latest`
+ *container_name*: 指定container的名稱，沒有指定的話預設會是`目錄名稱-服務名稱-編號`
+ *ports*: 本機對應container的port，`ex ports: 3000:3000`
+ *==depends_on==*: 當某些服務依賴於其他服務時，`例如後端app server會需要用到資料庫及redis進行存取`，這時就會需要確保redis及資料庫有先啟動成功，而depends_on的宣告就會讓此服務在兩者啟動後才啟動，確保啟動順序正確。`ex depends_on: serviceName1、serviceName2`
+ *networks*: 指定網路，這個地方有用到的就需要在下方networks宣告。
+ *volumes*: 指定volume，這個地方有用到的就需要在下方volumes宣告。
+ *environments*: 容器可能會使用到的變數，像是DB_HOST、DB_USER等等。可以將值透過`${DB_HOST}`這樣的方式儲存起來，並且在當前資料夾下建立`.env`，docker會自動去讀取`.env`檔案中對應的變數。也可以透過`docker compose config 來確定環境變數的應用是否正確`
+ *==restart==*: 定義容器重新啟動的狀態，主要有四個選項
	+ always - 只要容器存在，就會重新啟動
	+ unless-stopped - 如果容器是結束工作而退出狀態，或是被手動停止的話就不會重啟。其他情況都會重啟。
	+ on-failure - 只有當遇到錯誤才會重啟
	+ no - 不論發生何種情況都不會重啟

#### networks跟volumes參數

+ *external*: 宣告volume或network是不是外部已存在來源，如果是的話docker就會去尋找對應的來源。因為在沒有宣告external的狀況下，==docker預設會建立宣告的network或volume==。在沒有宣告networks的狀況下，docker也會預設有一個networks。

## yml及yaml

yml及yaml檔案都可以作為docker-compose的檔名，只是在沒有特別指定檔名的情況下，當yaml及yml同時存在時，yaml的讀取優先度會大於yml。

## build

在compose中，通常還是會包含我們的app server程式碼，每當我們進行一次程式碼更動重新跑compose的時候，若是還需要預先進行docker image build就會讓動作變得沒效率。可以透過在service內宣告build參數，來讓docker-compose幫我們自動build image。

短寫法
```yml
services:
  app:
    build: . <- 自動找到當前目錄的 Dockerfile
```

長寫法:
```yml
services:
  app:
    build:
      context: . <- 路徑
      dockerfile: Dockerfile <- 指定 Dockerfile 的檔案
```

## 擴充欄位

在docker-compose中，我們可能會有一些重複指定一些參數，像是networks，可能有兩三個容器都需要`networks: net`。這時可以利用`x-labels`搭配`<<:`來減少輸入錯字的機會。

```yml
x-labels: &networks  
networks:  
- production  
  
services:  
app:  
image: app  
<<: *networks 
db:  
image: db  
<<: *networks  
redis:  
image: redis  
<<: *networks  
  
networks:  
production:
```

`&networks`是替label命名，&有anchor的意思，接著再透過擴充符號，利用米字號去引用x-labels，就可以將屬性給擴充過去，這樣要修改時也可統一修改x-labels即可。

## 檔案覆寫

因為應用程式可能有多種環境 -> 開發、測試、正式等，可以透過將docker-compose檔案分成核心、開發等，在不同環境切換時透過--file去覆寫檔案。==但port檔案是無法被覆寫的==。

```bash
docker compose --file a --file b ->後方檔案覆蓋前方檔案
```

## Reference

[Docker Compose 篇 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/compose/)
---
date: 2024-02-19 Mon 9:22
---
---

# Outline

docker-compose是用來整合容器、虛擬網路、volume並且構成應用的一個工具

+ [檔案架構](##檔案架構)
	+ [top-level參數](###top-level參數)

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

### top-level參數

top-level參數主要有三個，分別是:
+ service
	+ 在其中定義要啟動的容器，透過在service內宣告需要的參數在docker-compose時一併啟動
+ volume
	+ 宣告會在service內被呼叫到的volume
+ networks
	+ 宣告會在service內被呼叫到的network

#### Service

service內部會填入的參數大多就是在運行單一容器時會填的參數，如`name`、`image`等等。
+ *image*: 指定該服務使用的image，`ex image: postgres:latest`
+ *container_name*: 指定container的名稱，沒有指定的話預設會是`目錄名稱-服務名稱-編號`
+ *ports*: 本機對應container的port，`ex ports: 3000:3000`
+ *==depends_on==*: 當某些服務依賴於其他服務時，`例如後端app server會需要用到資料庫及redis進行存取`，這時就會需要確保redis及資料庫有先啟動成功，而depends_on的宣告就會讓此服務在兩者啟動後才啟動，確保啟動順序正確。`ex depends_on: serviceName1、serviceName2`
+ *networks*: 指定網路。
+ *e
---
date: 2024-01-15 15:35
---
#NestJS

---
## Dependency installation

```bash
npm install @nestjs/typeorm typeorm mysql2 @nestjs/config
```

`@nestjs/typeorm`、`typorm`是要利用typeorm所需的套件，`mysql2`是資料庫連線需要的套件。`@nestjs/config`是變數配置相關的package。

## Connection Configuration

以下是程式碼:

```ts
//app.module.ts
import { Module } from '@nestjs/common';

import { AppController } from './app.controller';

import { AppService } from './app.service';

import { TodoModule } from './todo/todo.module';

import { ConfigModule, ConfigService } from '@nestjs/config';

import { TypeOrmModule } from '@nestjs/typeorm';

import { todoEntity } from './todo/todoEntity';

  

@Module({

  imports: [TodoModule, ConfigModule.forRoot(), TypeOrmModule.forRootAsync({

    imports: [ConfigModule],

    inject: [ConfigService],

    useFactory: (configservice: ConfigService) => ({

      type: 'mariadb',

      host: configservice.get('DB_HOST'),

      port: configservice.get('DB_PORT'),

      username: configservice.get('DB_USER'),

      password: configservice.get('DB_PASSWORD'),

      database: configservice.get('DB_NAME'),

      // entities: [__dirname + '/**/*.entity{.ts,.js}'],

      entities: [todoEntity],

      synchronize: true

    })    

  }), TodoModule],

  controllers: [AppController],

  providers: [AppService],

})

export class AppModule {}
```

這裡主要的重點是import進來的兩個module，`TypeOrmModule`以及`ConfigModule`。就像在express中一樣，我們會透過將變數放在`.env`檔，再透過`procsee.env`去讀取這些環境變數。在Nest中，我們可以透過`ConfigModule`來完成。

`ConfigModule.forRoot()`這個部分是在註冊`ConfigService`，預設會去讀根目錄的`.env`檔案。`forRoot()`method是一個常見的動態註冊方式。

在了解forRoot是用在動態註冊後，`TypeOrmModule`做的事情也是差不多的，因為我們接下來是動態的去拿去環境變數(透過`ConfigModule`)，並不是直接寫死在程式碼內，而這些讀取會需要時間，是非同步的，所以利用`TypeOrmModule`提供的`forRootAsync`，來註冊連線相關資訊。


這裡用到的是動態註冊模組的用法，`imports`引入需要用到的module，`injects`注入需要的`provider`，接著透過`useFactory這個property`動態載入需要的變數並回傳，接著就是透過`configService`裡的`get()`method去取得`.env`內對應的變數名稱。如此一來，就能夠完成與資料庫的連線。`.env`檔如下
```ts
DB_HOST= "your_DB_HOST"

DB_PORT= "your_DB_PORT"

DB_USER= "your_DB_USER"

DB_PASSWORD= "your_DB_PASSWORD"

DB_NAME= "your_DB_NAME"
```

上面在連線的過程中有兩個環境變數以外的屬性`entities`、`synchronize`。
`entities`是指在這次連線中要一同載入的entity，如果我們有需要操作到的entity都必須加入到這個部分中。
`synchronize`指的是在建立連線的過程中要不要自動`重新建立`schema, 這在開發過程中相當的有用，可以方便我們開發或儲錯，但`絕對不能`放到正是環境中，因為要是因為意外更動到程式碼，再加上這個屬性，可能會重建schema造成資料被覆蓋。

## Summary

+ 透過`TypeOrmModule`以及`ConfigModule`建立連線
+ 透過`forRoot()`進行動態註冊
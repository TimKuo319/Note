---
date: 2024-01-15 15:35
---
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

`ConfigModule.forRoot`

typeorm
	forRoot 
	forFeature
auto-load entity
configuration
	@nestjs/config
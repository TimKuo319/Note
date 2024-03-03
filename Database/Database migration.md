---
date: 2024-03-03 15:30
---
#database #migration

---

# Database migration

在軟體開發的流程中，資料庫的結構也會因應需求的不同而隨時間改變。可能會有新的table出現，或需要更改舊有table欄位名稱的狀況存在。而通常在這種狀況下，資料庫本身已經存在大量的資料，==在改變資料庫結構的同時，也需要確保這些資料能夠維持正常供使用==，而這就是需要用到migration的地方。


# Migration using NestJS with TypeORM

以下是簡單透過TypeORM嘗試一下database migration。

在Nest中，TypeORM的migration並沒有被加入到Nest的DI container中，因此是沒辦法透過DI的方式運用在Nest中的。需要額外依照TypeORM的方式去配置migration。

## datasource配置

首先需要配置datasource，也就是資料的來源。大致如下，與進行database connection時進行的配置幾乎一樣。

```ts
import { DataSource } from 'typeorm';

import { join } from 'path';

const connectionSource = new DataSource({

  type: 'mariadb',

  host: 'localhost',

  port: 3306,

  username: 'root',

  password: 'guo123',

  database: 'todolist',

  logging: true,

  entities: [join(__dirname + '/**/*.entity{.ts,.js}')],

  synchronize: false,

  migrations: [join(__dirname, '../migrations/*{.ts,.js}')],

  migrationsRun: false,

});


export default connectionSource;
```

+ *entity* - 指定entity schema所在的位置，讓typeORM去讀取其schema的結構
+ synchronize - schema在每一次啟動的時候是否需要被自動建立
+ migrations - 指定migration檔案的所在的位置，當透過migration的方式要去進行資料變更的時候，就會去這裡找對應的query。
## 產生migration file

透過以下指令
```sh
npx typeorm-ts-node-commonjs migration:create "where you want your migration path in"
```

typorm就會在指定的地方產生migration file。

migration file內會有兩個function，`up`以及`down`，up裡執行想要進行的變更，down裡執行回復的操作。

大致如下

```ts
import { MigrationInterface, QueryRunner } from 'typeorm';

export class UserRefactoring1709447775799 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query(
      `ALTER TABLE todo_entity RENAME COLUMN completed TO complete`,
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query(
      `ALTER TABLE todo_entity RENAME COLUMN complete TO completed`,
    );
  }
}

```

接著就可以透過指令來執行migration。

## 執行及回復migration

因為typeorm的`run`及`revert`指令都只適用在js檔案上。如果想要在未編譯ts檔案就進行migration的話，可以透過`typeorm`及`ts-node`來達成這件事。

```sh
npx typeorm-ts-node-commmonjs migration:run -d "your data source path"
```

後面的commonjs代表的是，ts檔案轉換過後js檔案所運用的模組類型，若是esm則有不同的指令，可以到orm的文件查詢[Migrations | TypeORM](https://typeorm.io/migrations#running-and-reverting-migrations)

透過以上的方式就能夠進行migration，

# Reference

[Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database#migrations-1)
[Migrations | TypeORM](https://typeorm.io/migrations#running-and-reverting-migrations)
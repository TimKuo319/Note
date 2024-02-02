---
date: 2024-01-21 Mon 16:14
---
#NestJS　#TypeORM

---

# Outline

+ [Repository](##Repository)
+ [Using forFeature](<## Using forFeature>)
+ [Reference](##Reference)
+ [autoLoadEntities](##autoLoadEntities)
+ [Summary](##Summary)
+ [Reference](##Reference)

在[NestJS Connection with MySQL using TypeORM](NestJS%20Connection%20with%20MySQL%20using%20TypeORM.md Connection with MySQL using TypeORM.md>)這篇筆記裡面提到了在Nest中利用`TypeORM`與`MySQL`資料庫進行連線。當時是在`app module`中去動態配置環境變數，讓`TypeORM`能夠去連上資料庫。但其實我們要透過`TypeORM`去進行資料庫操作的話，還需要提供資料的table格式給`TypeORM`讓他去尋找資料庫中對應的table或是建立一個全新的table，而我們會稱他叫做`Repository`。

## Repository

Repository大概會長的像這樣的形式:
```ts
import { Column, Entity, PrimaryGeneratedColumn } from "typeorm";

@Entity()

export class todoEntity{

    @PrimaryGeneratedColumn()

    id: number;

    @Column()

    task: string;

    @Column()

    deadline: Date;

    @Column()

    completed: boolean;

}
```

其實就很像我們透過SQL去create table時會寫的樣子，利用`@Entity()` 來裝飾todoEntity，讓`TypeORM`去替這個repository建立database schema。`@PrimaryGeneratorColumn`這一行指的是讓id變成這個entity的primary key，且這個id會隨著資料被匯入自動產生。後面的`@Column`則是宣告欄位的意思。當然還有其他許多decorator可以使用，可以參考官方文件[What is Entity? | TypeORM](https://typeorm.io/entities#column-types)。

## Using forFeature

在建立了repository後，就可以透過DI將他注入到我們需要的地方。以這個例子來說是`todoModule`
```ts
@Module({

  imports: [TypeOrmModule.forFeature([todoEntity])],

  controllers: [TodoController],

  providers: [TodoService]

})

export class TodoModule {}
```

我們要將這個todoEntity給`todoModule`做使用，這個時候就要透過`TypeORM`的`forFeature()`來註冊寫好的repository，這樣一來就能夠將它成功引入到模組中。

當我們在`todoService`內使用repository的相關功能的時候，就透過construtor來幫我們建立instance。

```ts
//todo.service.ts
export class TodoService {
    constructor(@InjectRepository(todoEntity) private todoRepo: Repository<todoEntity>){}
    async getTodo(): Promise<todoEntity[]>{

        return this.todoRepo.find();

    }
   ...
   ...
```

這裡注入時記得需要加上`@InjectRepository`decorator來注入repository，才能夠成功引入。

## autoLoadEntities

同樣是在[NestJS Connection with MySQL using TypeORM](NestJS%20Connection%20with%20MySQL%20using%20TypeORM.md Connection with MySQL using TypeORM.md>)這篇筆記裡面，我們提到過，在透過`TypeORM`連線資料庫的時候，需要在`entity`的屬性內加入需要`在這次連線中會使用到的entity`，來讓我們能夠正常的去使用repository，但如果隨著entity的數量變多，那就會造成那一列變得相當冗長。對此，可以透過`autoLoadEntities`這個屬性。

```ts
 useFactory: (configservice: ConfigService) => ({

      type: 'mariadb',

      host: configservice.get('DB_HOST'),

      port: configservice.get('DB_PORT'),

      username: configservice.get('DB_USER'),

      password: configservice.get('DB_PASSWORD'),

      database: configservice.get('DB_NAME'),

      autoLoadEntities: true;
```

藉由將這個屬性設為true，`TypeORM`會去幫我們找透過`forFearture()`註冊的entity，並自動把他們加入到連線中，如此一來，就可以省去人工輸入許多entity。

## Summary

+ 透過`forFeature()`import repository
+ 利用`@InejctRepository()`引入constructor中來建立repository的instance
+ 用`autoLoadEntities`載入透過`forFeature()`註冊的repository

## Reference

[What is Entity? | TypeORM](https://typeorm.io/entities#column-types)
https://docs.nestjs.com/techniques/database#auto-load-entities
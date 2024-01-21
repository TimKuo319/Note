---
date: 2024-01-21 Mon 16:14
---
#NestJS　#TypeORM

---

- [ ] TypeORM forFeature and antoLoadEntity 🔽


在[NestJS Connection with MySQL using TypeORM](<./NestJS Connection with MySQL using TypeORM.md>)這篇筆記裡面提到了在Nest中利用`TypeORM`與`MySQL`資料庫進行連線。當時是在`app module`中去動態配置環境變數，讓`TypeORM`能夠去連上資料庫。但其實我們要透過`TypeORM`去進行資料庫操作的話，還需要提供資料的table格式給`TypeORM`讓他去尋找資料庫中對應的table或是建立一個全新的table，而我們會稱他叫做`Repository`。

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

其實就很像我們透過SQL去create table時會寫的樣子，利用`@Entity()` 來裝飾todoEntity，讓`TypeORM`去替這個repository建立database schema。`@PrimaryGeneratorColumn`這一行指的是讓id變成這個entity的primary key，且這個id會隨著資料被匯入自動產生。後面的`@Column`則是宣告欄位的意思。當然還有其他許多decorator可以使用，
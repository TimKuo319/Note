---
date: 2023-12-22 Fri 00:12
---
---

利用js搭配nodejs作為伺服器端時，可以利用mongoose這個套件來建立與mongoDB的連線。

```js
const mongoose = require('mongoose')
```

接著開始進行連線
```js
mongoose.connect(DATABASE_URI)
const connection = mongoose.connection

/**

 * connection error handler

 */

connection.on('err', (err) => {

    console.log("connection error",err);

})


/**

 * connection success handler

 */

connection.once('open', () => {

    console.log('connected to database');

})

```


接著就可以開始建立想要的資料格式
```js
const dataSchema = new mongoose.Schema({

    thing:{

        type:String,

        required:true

    },

    isDone:{

        type:Boolean,

        default:false,

        required:true

    },

    createdDate:{

        type: Date,

        default: Date.now,

        required: true

    }

}
```

利用model() function替schema生成對應的model constructor function

```js
mongoose.model('data', dataSchema);
```

上面這個model function的參數第一個是指在mongoDB中的collection名稱，在這個部分，如果填寫的名稱是單數，則在mongoDB中會`自動`將名字變成複數，如果不想要名字變成複數的話，也可以在生成schema的時候加上第二個參數。

```js
const schema = new mongoose.Schema({
	//schema here..
}, {
	collection: 'your colleciont name'
})
```

如此一來，生成的名字就會依照collection

在新增資料時就新增一個該類型的model
利用save() function將資料存進資料庫中對應的collection名稱


undone

資料庫設計
collection關聯
objectid
"資料中的__v參數"

---
date: 2023-12-22 Fri 00:12
---
---

## Connection setup

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

連線成功後，接著就可以開始建立想要的資料格式
## Create data Schema

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

如此一來，生成的名字就會依照collection property指定的名字。

接著我們就可以利用model包含的函式來進行CRUD。

## CRUD

```js
const DataModel = mongoose.model('data', dataSchema)
```
假設我們透過上述方式建立了一個datamodel

### Create

新建立一個dataModel，傳入要新增的資料。
```js
const data = new DataModel({
	key1: value1,
	key2: value2,
	...
	...
})
```

再來利用save() function將資料存進資料庫中。
```js
const newData = await data.save();
```

這邊程式碼看起來很簡短，跟利用mysql在新增資料的方式有點不一樣。主要是因為我們在建立schema的時候已經指定了collection名稱，而且現在調用mongoose時，也會去自動尋找當前連線的資料庫，所以只要用save()這個function就能夠完成資料的更動。

### Read

Read的方式就更容易了，不用新建立model，利用find function就可以完成。
```js
const result = dataModel.find({'key1: value1'});
```

上述的例子中find裡面參數指的是要過濾的參數，可以透過指定特定的資料屬性來讓find指回傳特定資料，若是沒有傳遞參數則會回傳該collection的所有documents。

### Update

Update的方式是透過將現有的資料結果撈取出來，修改後再利用save()來更新結果
```js
const result = await dataModel.findById('document id');
result.key1 = ""
```

undone

資料庫設計
collection關聯
objectid
"資料中的__v參數"

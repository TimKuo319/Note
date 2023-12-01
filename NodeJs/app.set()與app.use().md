---
date: 2023-10-18 23:56
---
---

## app.set()

在利用`express-generator`產生出的檔案中，包含了`app.js`的檔案。

其中會包含像是
```JS
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
```
等等的程式碼，其中set所代表的是設定應用程式級別的變數，前面的字串為`變數名稱`，後面則為該變數的值。

而若想要在程式中其他地方利用這個值，可以使用`get`來取得該值，取前面的例子。
```js
const viewEngine = app.get('view engine')
```
```output
ejs
```

## app.use()

`app.use()`在express中主要是用來作為middleware綁定的函式
```js
app.use(logger('dev'));

app.use(express.json());

app.use(express.urlencoded({ extended: false }));

app.use(cookieParser());

app.use(express.static(path.join(__dirname, 'public')));

  

app.use('/', indexRouter);

app.use('/users', usersRouter);

  

// catch 404 and forward to error handler

app.use(function(req, res, next) {

  next(createError(404));

});

  

// error handler

app.use(function(err, req, res, next) {

  // set locals, only providing error in development

  res.locals.message = err.message;

  res.locals.error = req.app.get('env') === 'development' ? err : {};

  

  // render the error page

  res.status(err.status || 500);

  res.render('error');

});
```

上面的程式碼是express中常見會用到use的方式，前面像是`logger`、`express.json`、`express.urlencoded`..等等，都會在請求連線進來時，先經過這些middleware的處理，再交由下面的路由處理。一旦發生錯誤則會丟到下方的錯誤處理去。

總結來說`app.use()`這個方式的主要用途有以下幾點

1. 綁定處理請求的middlware
2. 綁定請求路徑對應的router
3. 錯誤處理
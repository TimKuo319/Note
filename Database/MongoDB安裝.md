

1. 官網下載mongodb
2. 創建一個要作為資料庫的資料夾，
3. (windows)到`Program file`下找Mongdb....../...../bin 資料夾，往後可以從這裡直接指定，也可以至環境變數設定，讓mongod指令可以直接被使用
4. 若是想要透過指令操作mongodb，可以下載mongosh(不一定必要)。
5. 若是想要透過圖形化介面，可以在最初安裝mongodb時，連mongo compass一起安裝。



### 啟動 Server

```bash
mongod --dbpath="your path name" --port="port you want to use"
```



參考:
[Mongo/mongodb command not working and environment variable path is also set - Other MongoDB Topics - MongoDB Developer Community Forums](https://www.mongodb.com/community/forums/t/mongo-mongodb-command-not-working-and-environment-variable-path-is-also-set/186599)

[Windows MongoDB Shell Installation: 3 Easy Steps | Hevo (hevodata.com)](https://hevodata.com/learn/windows-mongodb-shell/)


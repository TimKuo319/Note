
zookeeper 主要是開在 2181 port。Client 主要就是透過這個 port 來與他進行通訊。而在 3.5 版以後， zookeeper 新增了一個管理、監控介面的 `admin_server`，用來監控 zookeeper 以及與他互動的 znode，預設是開在 8080 port。



```
```

## Reference

[ZooKeeper使用实践踩坑总结-CSDN博客](https://blog.csdn.net/J080624/article/details/102080621)
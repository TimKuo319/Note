
其實在官方文件 [redis - Official Image | Docker Hub](https://hub.docker.com/_/redis) 中就有基本的參數說明，但並沒有包含到要如何進行密碼設定，所以記錄一下這個操作。

主要就是在 image 後面加上 `--requirepass your-password`

```sh
docker run redis --requirepass your-password
```

## Reference

- [redis - Official Image | Docker Hub](https://hub.docker.com/_/redis)

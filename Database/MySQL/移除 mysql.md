
```sh
sudo apt-get remove --purge mysql*
```

如果只是單純透過 remove 的話，只能移除 package 本身，並不會移除到 package 的 configuration 檔案相關設定。

- `--purge` 參數，連 configuration 檔案一並刪除。
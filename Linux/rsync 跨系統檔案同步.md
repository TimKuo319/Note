
```sh
rsync -avz /path/to/source /path/to/destination
```

### Common rsync option

- `-a`
	- 歸檔模式，表示會遞迴複製目錄，並保留符號連結、檔案權限、時間戳記等屬性。
- `-v`
	- 細模式，會在輸出中顯示更多訊息。
- `-z`
	- 壓縮傳輸的檔案，在網路速度較慢時會提高效率。
- `--delete
	- 在目標目錄中刪除原始目錄中已不存在的檔案，使得兩者完全同步。
- `-P
	- 顯示進度條，並且允許斷點續傳。
- `-e`
	- 指定使用的遠端 shell，通常用 `ssh` 來進行遠端同步。


### 傳輸機制

- 預設會使用 ssh 去進行連線，但需要將 ssh 的相關資訊配置好。
	- Ex : 哪個 `host` 應該對應到哪一個 `.pem`

### 手動指定 .pem 的同步方式

在沒有先配置好 ssh 資訊的狀況下，也可以手動指定 `.pem` 檔案進行配置。

```sh
rsync -avz -e "ssh -i /path/to/private_key" /path/to/source ubuntu@18.176.54.151:/path/to/destination
```







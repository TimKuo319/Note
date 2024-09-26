
- docker stats 會 realtime 的列出 container 的資源使用量。

```sh
docker stats --format <container_id or name> "table {{.Name}}\t{{.CPUPerc}}\t{{.MemPerc}}"

```


- `--no-stream` : 僅輸出一次資料
- `--format` : 自訂輸出格式
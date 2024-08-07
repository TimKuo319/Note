和其他服務一樣，redis本身同樣也具備自己的設定檔案`redis.conf`，預設目錄位置會在`/etc/redis/redis.conf`，其中包含許多可以調整的屬性。

## How

redis本身雖然是儲存在記憶體中，但在redis的內容有進行更動時，他會將備份儲存到一個名為`dump.rdb`的檔案中，如果不希望redis自動進行快照備份的話，就需要將原本的內容註解掉。

原先`redis.conf`備份相關內容如下

```redis.conf
 Save the DB to disk.
#
# save <seconds> <changes> [<seconds> <changes> ...]
#
# Redis will save the DB if the given number of seconds elapsed and it
# surpassed the given number of write operations against the DB.
#
# Snapshotting can be completely disabled with a single empty string argument
# as in following example:
#
# save "" ----------------disable----------------------------
#
# Unless specified otherwise, by default Redis will save the DB:
#   * After 3600 seconds (an hour) if at least 1 change was performed
#   * After 300 seconds (5 minutes) if at least 100 changes were performed
#   * After 60 seconds if at least 10000 changes were performed
#
# You can set these explicitly by uncommenting the following line.
#
# save 3600 1 300 100 60 10000 ----------------enable---------------
```

其中

```sh
save ""
```

表示的是不進行redis snapshot，也就是不快照

```sh
save 3600 1 300 100 60 10000
```

而上面則是備份快照觸發的狀況

分別為
1. 3600秒內有1次變動就備份
2. 300秒內有100次變動就備份一次
3. 60秒內有10000次變動就備份一次

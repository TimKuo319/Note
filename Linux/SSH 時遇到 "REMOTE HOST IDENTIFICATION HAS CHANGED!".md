
會遇到這個問題是因為要連線的 host 的 SSH Key 已經更改，跟自己本機上紀錄的 SHA ID 不相同導致的。

所以會需要刪除掉本機上的對於該 host 的記錄，讓電腦重新識別這個 host 。

### 使用指令刪除

```sh
ssh-keygen -R <hostname>
```

### 手動刪除

先透過以下指令觀察 host 在 `~/.ssh/known_hosts` 中的行數，進到其中將這些行數刪除

```sh
ssh-keygen -F <hostname>
```

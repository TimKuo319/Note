---
date : 20230-05-16 23:16 Wed
alias : []
---

---
## .gitignore
在使用git時，專案中可能會存在一些敏感資訊是我們不想被推到github上的，像是伺服器IP位置、資料庫密碼等等。這時候就可以藉由.gitignore檔案來忽略這些敏感文件。

在local repository中新增`.gitignore檔案`，在windows中可藉由git bash輸入以下指令達成。
```sh
touch .gitignore
```

再用vim修改內容

```sh
# .gitignorefile

# the file you want to ignore
secret.js

# the directory you want to ignore
secret/
```

### 專案進行到一半才加入.gitignore?

如果在中途才加入.gitignore檔案，雖然在之後的push中，特定檔案都會被ignore，但先前push上去的檔案依舊會存在。

1. 可以先利用
```
git ls-tree --name-only --full-tree -r HEAD
```
查看哪些檔案已經被git追蹤。

2. 將所有檔案移除追蹤
```
git rm -r --cached .
```

3. 利用add來更新狀態，在這次的add中，因為.gitignore規則的緣故，特定檔案就不會被追蹤到cache中
```
git add .
```

4. 更新commit ，push到遠端。如此一來，遠端repository中的特定檔案就會因此被移除了。


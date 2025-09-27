---
tags:
  - Git
---

在個人進行專案開發時，對於有錯誤的版本想要進行 rollback 的時候，我們能夠透過`git reset` 來回到指定的版本。

但透過 `git reset` 的方式會導致 git 的歷史丟失，如果在 production 環境需要進行退版時使用，當為來排查錯誤版本歷史的時候可能就會比較麻煩。

這時使用 `git revert` 就會是比較適當的一個做法。`git revert` 本質上單純是針對指定的 commit 進行反轉，所以他實際上是在當前的分支上**再增加一個新的 commit 進行反轉**，這樣既能夠達成保留過去歷史，且又能夠針對有問題的版本先進行回朔，方便後續進行排查。

`git revert` 在使用上也很簡單，針對要使用的版本使用 revert 指令即可。

```bash
git revert head
```


## Reference

[Reset、Revert 跟 Rebase 指令有什麼差別？ - 為你自己學 Git | 高見龍](https://gitbook.tw/chapters/rewrite-history/reset-revert-and-rebase)
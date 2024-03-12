
---

假設今天在開發的過程中，repository內含有許多的分支。若發現某幾個分支中commit的內容不錯想要合併進來，但又不想將整個分支與當前分支合併，則可以使用`cherry-pick`指令

```bash
git cherry-pick <commit SHA-1 value> ...
```

利用這樣的方式就可以將其他分支中特定幾個commit給接合到當前分支中。

如果在將commit pick過來的過程中還不想要馬上合併，則可以再加上`--no-commit`的參數，先將commi內的檔案放到暫存區中。


在過去的git版本，checkout除了能夠用來切換分支之外，也能夠進行檔案的還原。而後續為了避免指令的混淆，分支的切換可以利用`switch`來替代，而檔案的還原也能夠用`restore`來處理。

當一個檔案被追蹤且未修改時，會處於`Unmodified`的狀態，這時當對該檔案進行編輯的時候，因為檔案被修改了，所以檔案狀態理所當然的會變成`Modified`的狀態。但在我們利用`git add`將這個修改的檔案加入暫存區(stage)前，這個檔案的變更紀錄目前只存在於工作目錄(worktree)，這時若是想要還原這個變更的話，就可以利用以下指令。

```bash
git restore --worktree <filename>
```

在使用`restore`指令時，他預設的模式會是還原worktree中的檔案或資料夾。也就是說，以下指令其實跟上面指令具有相同的作用
```bash
git restore <filename>
```

那如果今天檔案已經被加入到暫存區中變成stage的狀態了呢?只要在使用restore時，加上`--stage`的參數，就可以對暫存區的內容進行還原!

```bash
git restore --stage <filename>
```

假設今天在worktree剛修改完，又對於stage中的部分檔案內容不滿意想要進行修改的話。可以使用以下指令

```bash
git restore --worktree <filename> --stage <filename>
```

藉由兩個參數`worktree`、`stage`，來達成還原。
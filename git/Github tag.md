
###  建立版本標籤（Tag）

你可以在本地的 Git 存儲庫中使用以下指令建立一個新標籤：

```bash
git tag <tag-name>
```

例如，要為 `v1.0.0` 建立一個標籤：

```bash
git tag v1.0.0
```

如果你想為一個特定的提交建立標籤，你可以在指令中指定提交的 SHA 值：

```bash
git tag <tag-name> <commit-sha>
```

### 推送標籤到 GitHub

建立標籤後，你需要將它推送到 GitHub：

```bash
git push origin <tag-name>
```

要推送所有標籤：

```bash
git push origin --tags
```

###  刪除標籤

如果你想要刪除本地或 GitHub 上的標籤，可以執行以下指令：

- **刪除本地標籤：**

```bash
git tag -d <tag-name>
```

- **刪除 GitHub 上的標籤：**

```sh
git push origin --delete <tag-name>
```

### 查看標籤

你可以列出存儲庫中的所有標籤：

```sh
git tag
```

這將顯示所有已經創建的標籤。
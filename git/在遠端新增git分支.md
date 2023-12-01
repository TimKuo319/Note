
先在本地利用`git checkout來建立分支`
```shell
git checkout <branch_name>
```

建立好之後在push的時候重新在遠端repo上指定新的分支名

```shell
git push -u <remote_repo> <branch_name>
```

即可在遠端repo上新增分支
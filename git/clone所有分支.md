
##  透過fetch先抓取所有遠端分支資訊

```sh
git fetch -all
```

## 透過shell script複製遠端分支

```sh
for branch in $(git branch -r | grep -v '\->'); do  
    git branch --track ${branch#origin/} $branch  
done
```

+ $branch - 抓到的遠端分之名, ex:`origin/xxxxx/`
+ ${branch#origin/} - 去掉`origin/`前綴
	+ 會剩下`xxxxx`

```sh
git branch --track ${branch#origin/} $branch
```

指將本地端的分支對應到遠端上的分支，前面為本地分支，後為遠端分之支

## Reference

[How to Clone all Remote Branches in Git? - GeeksforGeeks](https://www.geeksforgeeks.org/how-to-clone-all-remote-branches-in-git/)

在`~/.ssh/`中加上`config` 

內容如下

```config
Host myserver
    HostName example.com
    User myusername
    Port 22
    IdentityFile ~/.ssh/id_rsa

Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github

```


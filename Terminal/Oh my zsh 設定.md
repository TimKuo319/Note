## 自動補全

透過 brew 安裝
```sh
brew install zsh-syntax-highlighting
```

在`.zshrc`中，新增一下指令

```.zshrc
# plugin-syntax-highlighting
source $(brew --prefix)/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
export ZSH_HIGHLIGHT_HIGHLIGHTERS_DIR=$(brew --prefix)/share/zsh-syntax-highlighting/highlighters
```

## 自動補全

透過 brew 安裝
```sh
brew install zsh-autosuggestions
```

`.zshrc` 加入
```sh
# plugin-autosuggestions
source $(brew --prefix)/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

## autojump

```sh
brew install autojump
```

`.zshrc` 檔案加入
```sh
[ -f $(brew --prefix)/etc/profile.d/autojump.sh ] && . $(brew --prefix)/etc/profile.d/autojump.sh
```
## Reference

+ https://www.eudora.cc/posts/220721
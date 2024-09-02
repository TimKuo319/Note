
### What is actions/checkout ?  Why we need it ?

在撰寫 github action 的 yml 檔案時，我們時常會使用別人撰寫好的 action 檔案，而 `actions/checkout` 就是其中之一。

因為 github action 本身是將定義好的 workflow 放到某一台機器上去執行，所以要執行各種五花八門的操作以前，至少也要將我們的 code 先放到那台機器中。而這也就是 `actions/checkout` 在幫我們做的事情。他會將我們的 code 放到 runner(`跑 workflow 的機器`)。

### 程式碼來源

直接實際來看一個例子，以下是一個 github action 的 yml 檔案。這個檔案的觸發條件有兩個，一個是在 `push 到 main branch 的時候`，另外一個是`發 pr 到 main branch 的時候`。


所以當今天有一個從 `developer branch` 發向 `main branch` 的 PR 時，checkout 實際上會是將`來源跟目標結果合在一起，生成一個暫存的結果`，並將這個結果放到 runner 中，透過這樣將合併的 code 進行 CI workflow 的流程來先確保合併後的程式碼會是沒有問題的。

同理，當遇到 `push` 事件的時候，checkout 出來的結果也會是 `push` 完成的結果。

```yml
name: CI Pipeline

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build-and-push-backend:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
		
//....
```

當然以上都是 `actions/checkout` 的預設行為，`actions/checkout` 本身還有其他的參數可以去做使用，詳細內容可以參考`actions/checkout`的 repo [actions/checkout: Action for checking out a repo](https://github.com/actions/checkout)

## Reference

[actions/checkout: Action for checking out a repo](https://github.com/actions/checkout)


- [ ] 放操作圖片



### Secret vs Variable

在 security 的設定中，可以看到有 `secret` 與 `variable` 兩個選項可以去進行設定。

`secret` 主要是用來儲存一些敏感訊息，像是 `api key` 、`db password` 等等。使用方式大致如下。

```yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to server
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}

```


`variable` 主要是用來放一些環境變數，像是 `environment`、`app_name`、`build_version` 等等。

```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build application
        run: ./build.sh
        env:
          ENVIRONMENT: ${{ vars.ENVIRONMENT }}

```

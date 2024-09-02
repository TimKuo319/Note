#CICD

在撰寫 github action 的 yml 檔案時。可能會有某個 workflow 依賴於另外一個 workflow 的狀況，像是 CI 跑完才跑 CD 等等。

這時候就可以透過 `workflow_run` 這個事件來解決。使用範例大致如下

```yaml
name: CD Pipeline

on:
  workflow_run:
    workflows: ["CI Pipeline"]
    types:
      - completed

jobs:
  deploy-to-ec2:
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    runs-on: ubuntu-latest
    steps:
      - name: SSH to EC2 and deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{secrets.EC2_HOST}}
	...
```

這是一個 CD 的 pipeline，他會在 `CI Pipeline` 這個 workflow `成功執行完` 之後才執行。

### 坑

其實以上的事件觸發還以一個條件，那就是當 `CI Pipeline` 的來源是 `default branch`(通常是 main 或者是 master) 的時候才會觸發 `CD Pipeline`

假設說今天我發了一個從 `develop` 到 `main`  的 PR，這個 PR 會觸發 `CI Pipeline` ，但因為來源是 `develop`，所以即使這個 `CI Pipeline` 成功執行完畢，卻依舊不會觸發到 `CD Pipeline` 。

一旦這個 PR 被 merge 進到 `main` 的時候，因為 PR 的 merge 本身會將一個 commit `push` 到 main，所以就會觸發 `CI Pipeline` ，而因為這時的 `CI Pipeline` 來源是 `main` ，所以在這個 `CI Pipeline` 成功執行完畢後就會觸發 `CD Pipeline` 。

## Reference

[continuous integration - How to use the GitHub Actions `workflow_run` event? - Stack Overflow](https://stackoverflow.com/questions/63343937/how-to-use-the-github-actions-workflow-run-event)






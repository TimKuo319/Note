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

假設說今天發了一個從 `develop` 到 `main`  的 PR，這個 PR 會觸發 `CI Pipeline` ，但因為來源是 `develop`，所以即使這個 `CI Pipeline` 成功執行完畢，卻依舊不會觸發到 `CD Pipeline` 。

一旦這個 PR 被 merge 進到 `main` 的時候，因為 PR 的 merge 本身會將一個 commit `push` 到 main，所以就會觸發 `CI Pipeline` ，而因為這時的 `CI Pipeline` 來源是 `main` ，所以在這個 `CI Pipeline` 成功執行完畢後就會觸發 `CD Pipeline` 。

### Key point

當你在 GitHub 上將一個 PR 合併到 `main` 分支時，實際上會發生以下的事情：

1. **PR 合併到目標分支**：
    
    - 當你合併 PR 時，PR 的變更會被整合（merge）到目標分支（這裡是 `main`）。這個過程會將 PR 的內容應用到 `main` 上，並生成一個新的提交（merge commit）。
2. **合併後的 `main` 分支狀態**：
    
    - 合併完成後，`main` 分支的狀態更新了，包含了來自 PR 的變更。因此，`main` 分支變成了最新狀態，並且這個最新狀態被視為推送或拉取操作的來源。
3. **CI Pipeline 的觸發與 checkout**：
    
    - 當你在 `main` 分支上進行了合併，GitHub 會將這個更新的 `main` 分支作為 `push` 事件的觸發對象。CI Pipeline 在這個時候會啟動，並且使用 `checkout` action 檢出的是更新後的 `main` 分支。
        
    - 雖然這次合併是由 PR 的內容引發的，但在合併完成後，`main` 分支本身就包含了這些變更。因此，CI Pipeline 在觸發時會檢出的是這個最新的 `main` 分支，而不是 PR 的原始分支。

### Summary

- `push` - 以 `push` 觸發的事件會是已更新後的結果為主，如果是 push 到 `main` ，則下面的 job 會以更新後的 `main` 作為來源

- `pull_request` - 以 `pull request` 觸發的事件，會先將來源 branch 與目標 branch 進行合併出一個暫存結果，下面的 jobs 都以這個結果執行後續的步驟。但這個時候的來源目標是 `pull_request` 的來源目標，以上面的例子來說就是 `develop` 


## Reference

[continuous integration - How to use the GitHub Actions `workflow_run` event? - Stack Overflow](https://stackoverflow.com/questions/63343937/how-to-use-the-github-actions-workflow-run-event)






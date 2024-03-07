---
date: 2024-03-07 21:38
---
---

# Why CICD

將build、test、deployment等等流程自動化，讓自己能夠更專注在寫code上。
以將ts專案部署在docker上來說，就會需要先經過build -> compose等步驟才可以重新啟動容器，進行程式碼測試，而為了減少手動輸入這些指令的情況，就可以透過CI來解決。

## Github action
github action

name: workflow名稱
on: 觸發的事件
jobs: 要做的事件，通常會平行執行，也可以指定順序
	runs-on: 運行環境
	steps: workflow真正要執行的指令
		uses: 利用別人寫好的action
		name: step的名稱


每一個job都會跑在自己的runner上


github marketplace -> 尋找別人或官方寫好的action
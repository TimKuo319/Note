---
date: 2024-03-07 21:38
---
---

# Why CICD

將build、test、deployment等等流程自動化，讓自己能夠更專注在寫code上。
以將ts專案部署在docker上來說，就會需要先經過build -> compose等步驟才可以重新啟動容器，進行程式碼測試，而為了減少手動輸入這些指令的情況，就可以透過CI來解決。

## Github-action basic component

- workflow 
	- 一個工作流，通常會寫成一個 yml 檔案定義在 `.github/workflows` 中，一個 workflow 中可定義很多個 `job`

- job
	- 一個 job 通常會有很多 step，每一個 step 執行特定指令，將他們串聯起來完成一個 job。每一個 job 都可以`在不同的 runner 上執行`

- step
	- 執行指令的最小單位

- runner
	- `實際執行` job 的機器。


## Reference

[Quickstart for GitHub Actions](https://docs.github.com/en/actions/writing-workflows/quickstart)
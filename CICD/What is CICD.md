#CICD 

### What is CICD

CICD 中，CI 指的是持續整合，CD 指的持續交付。

可以將 CI 想像成是我們合併 code 的一個過程。只是過程被自動化了，透過自動執行測試和建置( build ) 的過程，來減少時間與手動出錯的成本，還能達到程式碼品質的管理。

而 CD 則是將 CI 產出的結果，像是 `jar` `docker image` 等等，部署到開發或正式環境中。 

#### CI
- 主要在於確保程式碼品質與必要檔案的 build 。

	- 程式碼品質
	
		- 執行 formatter 確保程式碼格式
		
		- 執行單元測試 確保程式碼品質
		
	- 必要檔案建置
	
		- 像是打包出 java 的 `jar` 檔案，或是將 js 框架打包成 `/dist` 等 
		
		- 對於使用到 docker 的部分，image 的 build 也屬於 CI，有點像是將 dockerfile 打包成 docker image，跟程式語言一樣都是打包

#### CD
- 主要在於實現軟體的交付過程，能夠完成自動化部署。

## Reference

[資訊科普系列(9) — CI/CD. 什麼是CI/CD | by 倪榮駿 | MODA IT | Medium](https://medium.com/moda-it/ci-cd-5e575538ae7d)


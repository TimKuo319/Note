
- 有牽涉到 docker 時，build 路徑要選對

- 被加入 `.gitignore` 的檔案在 github action 中是不會被讀取到的
	- 因為在 github action 透過 `@checkout/v2` 的時候是去讀取 repo 中的程式碼，而被 ignore 掉的檔案不會出現在 repo 中。

- 對於像是環境變數等等需要在 settings 中的 `secret and variables` 中設定
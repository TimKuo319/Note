
## S3

1. 建立 AWS s3 bucket

2. 關閉 "封鎖所有公開存取"

3. 點選 -> "屬性"，滑到最下方 -> "靜態網站託管"，按下 "編輯" -> "啟用"

4. 撰寫許可權政策 -> ==why?==

5. 登入畫面進行測試即可

## CloudFront

- 點選建立分佈

- 選擇來源

- 建立成功後，輸入 cloudfront 給的網址就能夠進到指定來源

## CluodFront Domain 綁定

1. cloudfront -> 一般 -> 設定
	- 在其中將備用網域填寫為自己的 domain name(==why?==)

2. 在 route53 中點選自己的網域 -> 建立紀錄 -> A 記錄 -> 勾選別名 -> 選擇 cloudfront 端點，即可將域名根目錄指向 cloudfront





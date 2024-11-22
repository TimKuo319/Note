
## 2-1

## todo

+ 先修改圖片路徑
+ 透過後端將圖片上傳到S3
	+ https://medium.com/@bilalazwar/uploading-images-to-aws-and-returning-urls-in-spring-boot-432f9bc7f7de
	
+ 修改	`adminController`中的邏輯
+ 修改舊有資料
	+ 將`db`中所有圖片前綴全部拿掉
	+ 將`uploads/`中所有圖片前綴全部拿掉
	+ ~~上傳`uploads/`資料夾內所有的圖片到S3

+ 將S3 bucket的位置作為前綴
	+ 可設為`環境變數`
+ 在資料從db撈回來的時候，讓前綴加上檔名，再回傳給前端，讓前端能成功參考到s3網址


## S3

+ 建立bucket時，點選開放所有公開存取
+ 但僅僅是允許公開存取還不夠，重點在於公開存取的的人能`做到什麼事`，前面的允許只是，讓他們能有做事情的`門檻`

+ 接著在bucket的許可，透過撰寫json格式，來允許`公開存取使用者`能存取bucket

```json

{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Principal": "*",
   "Action": "s3:GetObject",
   "Resource": "arn:aws:s3::: stylish-matcha /*"
  }
 ]
}
```

## Cloudfront

+ 點選cloudFront後建立分佈
+ 選擇好cloudfront來源
+ 依照指示建立cloudfront
+ 等待部署後，cloudfront即可指向s3


## 2-2 

## RDS

+ 依照aws rds建立流程
+ 將rds設定為連接至ec2運算資源，並選取自己現有的ec2

+ 透過以下指令先連接到rds中建立schema以及新增資料
```sh
mysql -h <rds_url> -u<user_name> -p <databaseName>
```

+ 將`applicatoin.property`檔案上傳到跟`.jar`檔案同一個資料夾下，讓相同屬性覆蓋掉`.jar`檔內的屬性


## Reference

[Spring Tips: Configuration](https://spring.io/blog/2020/04/23/spring-tips-configuration)

https://balian-ear.medium.com/aws-codepipeline-s3-access-denied-status-code-403-399fe7194c75


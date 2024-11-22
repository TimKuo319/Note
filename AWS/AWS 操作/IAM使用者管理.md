
+ IAM user
	+ 實際的使用者帳號
+ IAM role
	+ 對role設定權限，限制role可以去作什麼事情。role可以套用到`user`跟`機器上`，基本上就是`authorization`的概念

+ IAM identity center



## 建立role

1. 先建立role
2. 選取role對應的類型
	+ service
	+ account
	+ ....
3. 允許role的權限
4. 建立完成

## 對ec2 instance套用IAM role

+ 到ec2畫面
+ 選好ec2後，選擇安全性
+ 在安全性中設定IAM role

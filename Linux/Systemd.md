
## Usage

常用於管理和啟動系統中的各種服務。服務可以是後台運行的procss，像是`webserver`, `database`等

## Add service to systemd

1. 在`/etc/systemd/system/`目錄下放入想要的服務, 以`.service`結尾
```sh
sudo vim /etc/systemd/system/<your-service>.service
```

2. 加入`service`配置

```sh
[Unit]
Description=My Web Server
After=network.target mysql.service
Requires=mysql.service

[Service]
ExecStart=/usr/bin/java -jar /path/to/your/webserver.jar
Type=simple
User=ec2-user

[Install]
WantedBy=multi-user.target
```


+ `Unit`代表的是service的`metadata`
	+ `Description` - 服務簡短說明
	+ `After` - 定義服務的啟動順序
		+ 以上面程式碼來說，server會在網路服務啟動後才執行
	+ `Requires` - 表示當前服務依賴於某服務
		+ 以上面程式碼來說，代表當前服務依賴於mysql.service，所以如果mysql.service沒啟動的話，server也不會啟動
+ `Service`
	+ `ExecStart` - 啟動服務時要執行的指令
	+ `Type` - 通常都是simple
	+ `User` - 指定運行該服務的用戶
+ `Install`
	+ `WantedBy` - 表示服務的目標
		+ 以上面程式碼來說，代表會在多用戶模式下啟動


After vs Required
WantedBy
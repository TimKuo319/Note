## Mysql 安裝

1. 更新套件相關資訊
```sh
sudo apt-get update
```

2. 安裝mysql

```sh
sudo apt install mysql-server
```

3. 安全性配置

```sh
sudo mysql_secure_installation
```

因為 `mysql_secure_installation` 預設會設定 `root` 帳號密碼，但是在 ubuntu 安裝上，並不會預先去配置 root 帳號，所以要手動設定。

4. 先進到 `mysql`

```sh
sudo mysql
```

因為 `mysql_secure_installation` 會創建一個匿名使用者，所以以上的指令可以成功。

5. 進到 `mysql` 後透過 `ALTER User` 來更改權限

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

預設的密碼 policy 是`最少8個字元`，包含`大寫、小寫、特殊符號`


### 查看密碼政策

```mysql
SHOW VARIABLES LIKE 'validate_password%';
```


## 僅安裝 Mysql client

```sh
sudo apt install mysql-client
```


## Reference

[How To Install MySQL on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)

[[教學][Ubuntu 架站] 在 Ubuntu 20.04 上安裝 MySQL Server | 優程式](https://ui-code.com/archives/293)
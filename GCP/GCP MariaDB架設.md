---
date: 2024-01-09 10:52 Tue
---
---

在建立完虛擬機器及在其中安裝完`MariaDB`後。輸入以下指令

```sh
vim /etc/mysql/mariadb.conf.d/50-server-cnf
```

進到檔案中後，修改bind-address為`0.0.0.0`。

接著再到google cloud內設定防火牆規則。

![[GCP_firewall.png]]

參考:
[在Google Cloud Platform 的虛擬機中建立 MySQL | 程式Q馬 (medium.com)](https://medium.com/%E5%B0%8F%E5%B0%8F%E7%9A%84%E7%A8%8B%E5%BC%8F%E7%A2%BC/%E5%9C%A8gcp-%E4%B8%AD%E7%9A%84%E8%99%9B%E6%93%AC%E6%A9%9F%E4%B8%AD%E5%BB%BA%E7%AB%8B-mysql-1b08b23ec833)
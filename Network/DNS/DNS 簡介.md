
## What is DNS, why we need it ?
機器在網路上是透過 IP 來作為自己獨一無二的位置，然而因為數字相對於語言難記憶，所以我們通常會使用一個`域名`去稱呼我們網站，而 DNS 在做的事情就是`域名與IP的轉換`

## Component in DNS

### DNS Resolver
 DNS Resolver 的提供者通常是 ISP，本身並不存在 domain 相關的資料，而是記錄著 `Root Name Server` 的位置，會透過去向 Root Name Server 詢問 domain 的相關資料，一步步的得到 domain name 的對應 IP

### Root Name Server
紀錄著 `TLD(Top-Level Domain)  Name Server` 的位置，在接收到 DNS Resolver 的請求後，會給他對應 TLD server 的位置，讓他去詢問更細節的 domain 相關資訊

### TLD Name Sever 
>和Root名稱伺服器類似，TLD Name Server 也沒有直接保存 example.com 的IP地址。但它知道要去哪裡找負責該域名的名稱伺服器，稱為 Domain 名稱伺服器

### Domain Name Server
實際持有 domain 對應 IP 的伺服器，會回應給 DNS Resolver 機器實際的 IP，讓 DNS Resolver 將 IP 交給瀏覽器去對實際的機器做請求。


*註： Root Name Server 及 TLD Name Server 主要由 ICANN 管理*

## Reference
- [域名系統（DNS）101—網址的小旅行. Domain Name System, DNS 101 | by YH Yu | 後端新手村 | Medium](https://medium.com/%E5%BE%8C%E7%AB%AF%E6%96%B0%E6%89%8B%E6%9D%91/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%B5%B1-dns-101-7c9fc6a1b8e6)


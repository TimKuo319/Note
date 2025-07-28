---
tags:
  - AWS
---

在 [[IAM User and IAM Role]] 中我們知道可以透過 IAM role 將權限套在 AWS 資源上，讓這些資源有特定權限去執行某些行為。ex : ec2 存取 s3 等

將 IAM role 套在 AWS 資源上的好處在於，相較於直接將與 AWS 申請的憑證或 key 放在 ec2 instance 中允許權限，一但遇到機器遭駭的情況，使用 IAM role 的方式只需要撤銷該資源的角色，而直接將 key 放在其中的做法則需要直接重新換 key。因此 IAM role 會是比較推薦的做法。


##  How to build IAM role

1. 進到 AWS 後點選 IAM，從側邊欄選取 role 點進去後就可以點選上方的 create role
2. 建立 trusted policy，可以選擇 AWS 常見的 use case 或自訂
3. 下一步則是選則 permission policy。建立完後即新增好一個 IAM role 
4. 到需要使用的資源賦予他這個 IAM role 即可。

## Trusted Policy vs Permission Policy 

在前面提到的操作步驟中可以看到建立 IAM Role 需要建立兩個 policy，那他們各自在 IAM role 設定中的用途是什麼呢？

Trusted Policy 定義了誰有資格扮演這個角色，以常見的設定來說可能會限制只有 ec2、s3 等資源能辦理這些角色，在 Trusted Policy 的設定中通常會用 Principal 來定義這個部分。

Permission Policy 則是定義了 IAM role 能做到哪些事情，AWS 本身也提供了很多預設的 Permission Policy 可以使用，像是 AdminstratorAcess、AmazonS3FullAccess 等，如果想要自訂也可以點選 sidebar 的 Policy 自訂自己需要的政策。


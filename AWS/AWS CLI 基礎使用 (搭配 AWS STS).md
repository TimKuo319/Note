
## Why

為了在本機也能夠模擬 ec2 環境，可以透過 aws cli 來協助進行環境建立，儲存 aws 的各種憑證、key 等等，這樣在本機就可以模擬雲端環境，也可以保持兩邊程式碼相同，不用改來改去

### Installation

```sh
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

- [Install or update to the latest version of the AWS CLI - AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### 設定環境變數

>[!danger]
>不能使用 root 進行設定，因為使用 root 操作已經被 aws 視為高風險！

- 透過 AWS CLI 進行設定

```sh
aws configure
```

- 輸入後會需要填寫四個資訊
	1. Access Key ID
	2. Secret Access Key
	3. Region name
	4. output format

前面兩個要先到帳號下 -> 安全憑證進行申請。`region name` 的話則是依照要使用的區域填寫，格式像是 `ap-northeast-1`，輸出格式可以選擇 `json`

輸入完畢後，透過以下指令確認。

```sh
aws sts get-caller-identity
```

### STS

- 因為 STS 是去要一個臨時的憑證，所以會需要先在本地透過前一步 [設定環境變數](###設定環境變數) 來先配置好帳號的憑證。

#### IAM Role 允許 STS

畫面點擊 -> 許可 -> 建立內嵌政策 -> JSOＮ

然後開始進行政策撰寫，主要是允許該使用者在這個資源上使用 STS

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "sts:AssumeRole",
			"Resource": "arn:aws:iam::<account_id>:role/SecurityGroupChanger"
		}
	]
}
```

#### 資源信任關係設定

接著要到目標資源去建立信任關係，允許這個資源被某個 IAM Role 或 IAM User 使用

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<acouunt_id>:user/admin",
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

#### 透過 STS 拿取憑證

```sh
aws sts assume-role \
    --role-arn arn:aws:iam::123456789012:role/EC2ServiceRole \
    --role-session-name rootSession

```

有出現畫面就代表成功了



## To learn

- [ ] 信任關係

- [ ] STS
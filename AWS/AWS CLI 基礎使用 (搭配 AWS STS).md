
## Why

為了在本機也能夠模擬 ec2 環境，可以透過 aws cli 來協助進行環境建立，儲存 aws 的各種憑證、key 等等，這樣在本機就可以模擬雲端環境，也可以保持兩邊程式碼相同，不用改來改去

### Installation

```sh
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

- [Install or update to the latest version of the AWS CLI - AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### 設定環境變數

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

### STS

- 因為 STS 是去要一個臨時的憑證，所以會需要先在本地透過前一步 [設定環境變數](###設定環境變數) 來先配置好帳號的憑證。

```sh
aws sts assume-role \
    --role-arn arn:aws:iam::123456789012:role/EC2ServiceRole \
    --role-session-name rootSession

```

`arn` 從 aws 的 console 拿

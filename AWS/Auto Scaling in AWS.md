---
tags:
  - AWS
  - Backend
---
## What is `Auto Scaling`, why we need it

Auto Scaling 是一種讓可以在 instance 達到特定條件時，讓 AWS 自動進行 instance 的增加 / 縮減來符合當前的情況，以下會介紹 Auto Scaling 大致上的設定步驟及設定過程中常使用到的名詞。 

## Steps

1. 直接建立 AMI 或依照現有的 EC2 instance 建立 AMI

2. 透過 AMI 建立 Launch Template -> 這裡的 template 就是 auto scaling 會建立出來的樣子
	- 在建立 Launch Template 的時候會看到有選項 "**請勿包含啟動範本**" 指的意思是指透過手動輸入的或其他方式提供啟動參數
	
	- 在 "**進階詳細資訊的選項**" 中的最下方會有 **user data** 欄位可以進行設定，這裡可以輸入 script 讓 instance 在**第一次被啟動時執行(重啟不算)** 
	
	- 其他的 SG 或網路設定就類似於 EC

3. 透過 Launch Template 去新增 auto scaling group
	- 選取啟動範本
	
	- 網路選項
		- [ ] 至少要選取兩個 AZ
		
	- 選取設定好的 load balancer
		- load balancer 在建立時可以選取對內或對外，依個人需求調整
		
	- Health Check
		- 運作狀態檢查寬限期
			- 在 ASG 啟動新的 instance 後，會過了這個時間才對 instance 進行檢查。目的在於機器完成啟動後才進行檢查，避免觸發不必要的擴展 / 縮減  

4. 設定群組大小與擴展
	- 設定 ASG 的大小、最大數量、最小數量等等

	- 設定擴展策略
		- 讓 ASG 在遇到不同指標時能夠針對機器進行擴展 / 縮減

	- [ ] 執行個體維護政策
## Terminology

### 1. 核心概念與名詞解釋

#### 基本組件

- **Auto Scaling Group (ASG)**: 管理一組相同設定的 EC2 實例集合
- **Launch Template**: 定義實例啟動配置的模板（AMI、實例類型、安全群組等）
	- Launch Template 在進行更新時會直接創建新的版本，在建立 ASG 時針對同一個 Template 可以選擇不同的版本 

- **Launch Configuration**: 較舊的實例配置方式，現在建議使用 **Launch Template**
- **Target Group**: ALB/NLB 中管理後端實例的邏輯分組
- **Load Balancer**: 分散流量到多個實例（ALB、NLB、CLB）

#### 容量管理

- **Desired Capacity**: 期望的實例數量
- **Min Size**: 最少實例數量
- **Max Size**: 最多實例數量
- **Current Capacity**: 目前實際運行的實例數量

#### 實例狀態

- **InService**: 實例正常運行並接收流量
- **Pending**: 實例正在啟動中
- **Terminating**: 實例正在終止中
- **Standby**: 實例暫停服務但保持運行
- **OutOfService**: 實例無法接收流量

### 2. Scaling Strategy

#### Target Tracking Scaling 

自動維持指定的目標值

- `ASGAverageCPUUtilization`: ASG 平均 CPU 使用率
- `ASGAverageNetworkIn`: ASG 平均網路輸入
- `ASGAverageNetworkOut`: ASG 平均網路輸出
- `ALBRequestCountPerTarget`: 每個目標的 ALB 請求數

#### Step Scaling  

依照 CloudWatch 警報觸發，可設定多個階梯

```bash
# 範例：CPU > 70% 時增加 2 個實例
aws autoscaling put-scaling-policy \
    --policy-type StepScaling \
    --step-adjustments '[{
        "MetricIntervalLowerBound": 0,
        "ScalingAdjustment": 2
    }]'
```

### 2.3 Simple Scaling（簡單擴展）

最基本的擴展方式，一次調整固定數量

bash

```bash
# 範例：增加 1 個實例
aws autoscaling put-scaling-policy \
    --adjustment-type ChangeInCapacity \
    --scaling-adjustment 1
```

### 3. 重要設定參數

#### 3.1 Cooldown（冷卻時間）

- **Default Cooldown**: ASG 層級的全域冷卻時間（預設 300 秒）
- **Scale Out Cooldown**: Scale Out 後的冷卻時間
- **Scale In Cooldown**: Scale In 後的冷卻時間
- **用途**: 防止頻繁的擴展/縮減操作

#### 3.2 Health Check（健康檢查）

- **EC2 Health Check**: 檢查實例系統狀態
- **ELB Health Check**: 檢查負載平衡器目標健康狀態
- **Grace Period**: 實例啟動後等待多久才開始健康檢查（預設 300 秒）

#### 3.3 Termination Policy（終止策略）

決定 Scale In 時要終止哪個實例：

- `Default`: 先均衡 AZ，再選最舊的實例
- `OldestInstance`: 終止最舊的實例
- `NewestInstance`: 終止最新的實例
- `OldestLaunchTemplate`: 終止使用最舊 Launch Template 的實例
- `ClosestToNextInstanceHour`: 終止最接近下一個計費小時的實例

#### 3.4 Instance Warmup（實例暖機時間）

新實例被納入指標計算前的等待時間，避免未就緒的實例影響擴展決策

### 進階功能

#### Instance Refresh

批次更新 ASG 中的實例

```bash
aws autoscaling start-instance-refresh \
    --auto-scaling-group-name my-asg \
    --preferences '{
        "MinHealthyPercentage": 50,
        "InstanceWarmup": 300
    }'
```
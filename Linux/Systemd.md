
+ [systemd](##systemd是什麼)
+ [為什麼需要systemd](##為什麼需要systemd)
## systemd 是什麼

+ 在電腦開機的過程中，記憶體載入kernel後，第一隻執行的程式

+ `SysVinit`
	+ 傳統linux使用的版本
+ `Upstart` 
	+ Canonical公司開發的，但在ubuntu15.04後也改為systemd
+ `Systemd`


## 為什麼需要 systemd

由於 systemd 可以自訂服務相依性的檢查，因此如果 B 服務是架構在 A 服務上面啟動的，那當你在沒有啟動 A 服務的情況下僅手動啟動 B 服務時， systemd 會自動幫你啟動 A 服務喔！這樣就可以免去管理員得要一項一項服務去分析的麻煩。

## systemd 架構

+ `systemd`透過`unit`的概念來表示和管理各種系統資源及服務
	+ `.service` - 管理系統服務
	+ `.target` - 管理系統狀態
	+ `.socket` - 通訊相關
	+ `.mount` - file system掛載相關
	+ `.timer` - 定時器，用於特定時間執行任務
	+ `.path` - 監控文件或目錄的狀態變化

## service設定檔案位置

systemd 將過去所謂的 daemon 執行腳本通通稱為一個服務單位 (unit)，而每種服務單位依據功能來區分時，就分類為不同的類型 (type)。基本的類型有包括系統服務、資料監聽與交換的插槽檔服務 (socket)、儲存系統狀態的快照類型、提供不同類似執行等級分類的操作環境 (target) 等等。設定檔都放置在底下的目錄中：

- `/usr/lib/systemd/system/`：每個服務最主要的啟動腳本設定，有點類似以前的 /etc/init.d 底下的檔案；
- `/run/systemd/system/`：系統執行過程中所產生的服務腳本，這些腳本的優先序要比 /usr/lib/systemd/system/ 高！
- `/etc/systemd/system/`：管理員依據主機系統的需求所建立的執行腳本，執行優先序最高


## systemctl指令 

- start：立刻啟動後面接的 unit
- stop：立刻關閉後面接的 unit
- restart：立刻關閉後啟動後面接的 unit，亦即執行
- stop 再 start 的意思
- reload：不關閉後面接的 unit 的情況下，重新載入設定檔，讓設定生效
- enable：設定下次開機時，後面接的 unit 會被啟動
- disable：設定下次開機時，後面接的 unit 不會被啟動
- status：目前後面接的這個 unit 的狀態，會列出有沒有正在執行、開機預設執行否、登錄等資訊等！
- is-active：目前有沒有正在運作中
- is-enabled：開機時有沒有預設要啟用這個 unit

```sh
sudo systemctl status stylish
```

放筆記截圖


## journalctl 查看服務的log

+ `-r` 從最後面顯示
+ `-u` 指定服務
+ `-f` 即時顯示

```sh
journalctl -r -u nginx.service
```


## Unit file 結構


當會使用指令操作套件之後，也可以學習如何將自身開發的服務也變成由 systemd 管理的應用。配置文件大致架構有三大項：

- [Unit]： unit 本身的說明，以及與其他相依 daemon 的設定，包括在什麼服務之後才啟動此 unit 之類的設定值；
- [Service], [Socket], [Timer], [Mount], [Path]..：不同的 unit type 就得要使用相對應的設定項目。我們拿的是 sshd.service 來當範本，所以這邊就使用 [Service] 來設定。 這個項目內主要在規範服務啟動的腳本、環境設定檔檔名、重新啟動的方式等等。
- [Install]：這個項目就是將此 unit 安裝到哪個 target 裡面去的意思！


以`amazon-cloudwatch-agent.service`舉例

```sh
[Unit]
Description=Amazon CloudWatch Agent
After=network.target

[Service]
Type=simple
ExecStart=/opt/aws/amazon-cloudwatch-agent/bin/start-amazon-cloudwatch-agent
KillMode=process
Restart=on-failure
RestartSec=60s

[Install]
WantedBy=multi-user.target
```


## service參數

- **Type**：指定服務的啟動類型。常見的值有 `simple`、`forking`、`oneshot`、`dbus`、`notify` 和 `idle`。這個設定會影響 systemd 如何管理服務和認為它何時已經啟動成功。
- 執行命令相關：等號右邊直接寫 shell 指令
    - **ExecStart**：定義啟動服務時執行的命令。多看幾個例子就會了：
        
        ```
        ExecStart=/usr/bin/java -jar /home/ubuntu/stylish-0.0.1-SNAPSHOT.jar
        ```
        
        ```
        ExecStart=/opt/aws/amazon-cloudwatch-agent/bin/start-amazon-cloudwatch-agent
        ```
        
    - **ExecStartPre** 和 **ExecStartPost**：分別定義在 `ExecStart` 命令之前和之後要執行的命令。這些可用於準備服務啟動的環境或進行啟動後清理。
        
    - **ExecStop**：定義停止服務時執行的命令。如果未設定，systemd 會使用 SIGTERM 信號停止服務。
        
    - **ExecReload**：定義當服務收到重新加載配置的請求時執行的命令。
        
- **Restart**：定義在服務異常終止時 systemd 應如何處理重啟。常見的值包括 `no`、`always`、`on-success`、`on-failure`、`on-abnormal`、`on-watchdog`、`on-abort` 和 `unless-stopped`。
    - no：不自動重新啟動，預設值。
    - always：無論服務由於什麼原因終止都會重新啟動。**不過 `systemctl stop` 停止後並不會重啟。**
    - on-failure：在遭遇特定類型的錯誤終止時應該被重新啟動。
        - 正常使用 systemctl stop 不會觸發 `Restart=on-failure`。
        - 使用 `kill` 發送 SIGTERM （終止信號，默認信號）或 SIGINT （中斷信號）時不會觸發 `Restart=on-failure`。
    - on-success：僅當服務正常終止且退出狀態為 0 時重啟。
- **RestartSec**：定義在嘗試重啟服務之前等待的時間（以秒計）。預設 `RestartSec` 值通常是 100 毫秒（0.1 秒）。這個快速重啟的預設設置通常足以應對大多數情況，但在某些情況下，可能需要更長的延時來讓一些資源得到釋放，或等待一個外部條件成熟（例如，等待網絡恢復正常）。
- **TimeoutStartSec**, **TimeoutStopSec**, 和 **TimeoutSec**：
    - 分別定義啟動、停止和通用超時的時間限制。如果服務在指定時間內沒有成功啟動或停止，systemd 將會嘗試強制終止它。
- **Environment**：設置在服務中可用的環境變數。
- **WorkingDirectory**：設定服務啟動時的工作目錄。
- **User** 和 **Group**：指定運行服務的用戶和群組。這可以用於限制服務的權限，提高安全性。



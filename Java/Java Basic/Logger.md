
## System.out.println vs Logger

+ `System.out.println`
	+ Pros
		+ 能夠快速輸出
		+ 無須額外配置
	+ Cons
		+ 不能==輕易控制輸出的級別、格式或目的地==
		+ 無法輕易追蹤或過濾

+ `Logger`
	+ Pros
		+ 支援多種輸出級別(`TRACE`、`DEBUG`、`INFO`、`WARN`、`ERROR`)，可以有效選擇不同重要性的訊息
	+ Cons
		+ 需要額外配置、學習成本較高

## Logger

### SLF4J

+ 作為抽象層，提供通用的日誌API，而不實現日誌紀錄。
+ 可以將他想像成是各種日誌套件的`轉接器`
	+ 各種套件
		+ logback
		+ log4j2

### Logback

+ spring boot預設的logger套件


### How to use logger


```java

import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
  
@RestController  
public class LoggingController {  
  
    Logger logger = LoggerFactory.getLogger(LoggingController.class);  
  
    @RequestMapping("/login")  
    public String index(){  
        logger.trace("A trace message");  
        logger.debug("A debug message");  
        logger.info("A info message");  
        logger.warn("A warn message");  
        logger.error("A error message");  
  
        return "Check out output!";  
    }  
}
```


+ `getLogger` - 用於創建一個Logger instance
	+ 通常傳入當前class
	+ 也可傳入字串名稱

### 將log儲存成檔案

在`application.properties`中設定log的匯出路徑
```sh
logging.file.name=logs/app.log
```


### Logger level

以下是從最高到最低的level，將logger level的全線設的越高，則輸出==越詳細==

+ TRACE
	+ 常用於開發階段、追蹤程式的執行流程
+ DEBUG
	+ 用於DEBUG
+ INFO
	+ 紀錄系統正常運行狀態
	+ 舉例
		+ ==用戶登入成功、重要操作完成等訊息==
+ WARN
	+ 有可能出現問題的狀況、但不影響系統運作
	+ 舉例
		+ ==被棄用的API、資源即將耗盡==
+ ERROR
	+ 記錄影響功能或操作的錯誤，但不會導致應用程序崩潰
	+ 舉例
		+ ==數據庫連接失敗、重要業務流程中的異常==
+ FATAL
	+ 致命錯誤（並非所有框架都包含此級別）
	+ 記錄導致應用程序崩潰或無法繼續運行的嚴重錯誤
	+ 舉例
		+ ==嚴重的資源耗盡、系統崩潰==

### 設定logger level

一樣是在`application.properties`中進行設定

```sh
logging.level.root=TRACE
```


### 自訂義 logger格式

1. 透過`application.properties`
```sh
# 控制台日誌格式
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %logger{36} - %msg%n

# 文件日誌格式
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
```

2. 透過logback.xm
```xml
<configuration>
    <!--    設置變數，將檔案位置設為./logs-->
    <property name="LOG_PATH" value="./logs" />
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <!--用ThresholdFilter過濾ERROR以上級別才顯示在terminal上~-->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <!-- 印出Log的格式 -->
            <!-- %d{YYYY-MM-dd HH: mm:ss.SSS} log 時間 -->
            <!-- %thread 執行緒名字 -->
            <!-- %highlight 顯示高亮顏色 -->
            <!-- %-5level log級別且顯示5個字，靠左對齊 -->
            <!-- %logger log的名字 -->
            <!-- %msg log訊息 -->
            <pattern>%d{YYYY-MM-dd HH:mm:ss.SSS} [%thread] %highlight(%-5level) %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <!--输出到文件-->
    <appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 輸出文件的位置，以每一天做切割-->
            <fileNamePattern>${LOG_PATH}/logback.%d{yyyy-MM-dd}.log</fileNamePattern>
            <!--保留30天的歷史紀錄 -->
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <!--設定WARN以上級別才需要輸出至檔案-->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>WARN</level>
        </filter>
        <!-- 也可以在這裡設置log level -->
		    <logger name="org.springframework" level="TRACE" />
		    <logger name="com.example" level="TRACE" />
        <encoder>
            <!--Log的格式-->
            <pattern>%d{HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
        <!--把appender加上去-->
        <appender-ref ref="console" />
        <appender-ref ref="file" />
    </root>
</configuration>
```

+ 常用的格式化符號：
	- `%d{yyyy-MM-dd HH:mm:ss}`: 日期和時間
	- `%thread`: 線程名
	- `%-5level`: 日誌級別，用 5 個字符右對齊
	- `%logger{36}`: Logger 名，最長 36 個字符
	- `%msg`: 日誌消息
	- `%n`: 換行符
	- `%color()`: 為不同級別的日誌設置顏色（僅控制台）
## Reference

[iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10248974)
[【Backend】別在只用 DEBUG 來處理 log，你該知道的 logging level 間的差異？ | by Zam Huang | Medium](https://zamhuang.medium.com/linux-%E5%A6%82%E4%BD%95%E5%8D%80%E5%88%86-log-level-216b975649a4)
[using Loggers](https://rural-woolen-15d.notion.site/using-Loggers-051bca1b62a54bbc81198f4dbcf6be2e#56f5781385634a3b9b3dd69417fc56ac)
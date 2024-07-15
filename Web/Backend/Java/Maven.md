
## Archetype

+ 用來快速生成新的Maven檔案結構
+ 添加參數時需要使用雙引號(`""`)來包住參數，否則會報錯(`under windows`)

```sh
mvn archetype:generate -B "-DgroupId=com.teamtreehouse" "-DartifactId=file-spy"
```


## pom.xml
## Lifecycle

+ clean - delete target directory
+ default
	+ validate
	+ compile
	+ test
	+ package 
	+ verify
	+ install
	+ deploy
+  site
## Java編譯

1. Java為了實現跨平台的關係，所以會先將我們撰寫的`.java檔`編譯成`.class檔`
2. `.class檔案`中包含的是`bytecode`，這些`bytecode`再交由JVM(`Java Virtual Machine`)執行，轉換為各作業系統適用的`machine code`。

## JAR

+ JAR通常是將編譯好的class檔案打包成一個壓縮檔案
	+ 易於分發與部署
	+ 支持壓縮
	+ 可以直接執行
		+ 透過`java -jar`

## Package Files to JAR

1.  `clean` - 先清除掉target folder(內含各種class檔案)
	+ 因為maven編譯後的`.class`會被放在target中，如果沒有先clean掉target folder，其中可能會引用到過去的`.class`檔案或是使用到過去的`.jar`檔案
2.  `package` - 將專案打包成`.jar` 
3.  `install` - 執行整個maven default lifecycle，並產生出`.jar`檔案，會放在==local的maven repository==，供其他本地的檔案做引用
4.  JAR file should be in target folder

## Deployment strtegy

1. 切換至project 的root directory，並使用`mvn spring-boot run`
	+ 通常會在開發環境使用，用於快速測試
	
2. 透過前面[Package Files to JAR](<##Package Files to JAR>)先打包成jar檔案，再利用`java -jar filename.jar`來啟動專案
	+ 通常會在正式環境中使用

## Maven repository
## Reference

官方文件:  https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html


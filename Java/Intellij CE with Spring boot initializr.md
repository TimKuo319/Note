
在 Intellij CE 中，沒有像付費版一樣內建 spring starter 的功能，所以需要透過去搜尋 spring boot initializr 來生成 `pom.xml` 或 `.gradle` 檔案來引入套件。

首先先點擊 New Project > Java，建立出一個 Java 專案。

接著到網路上搜尋 spring boot starter，選擇 project 使用的 build tool，language 以及 spring boot 的版本後填寫 project metadata。包含以下欄位

- `group` : 通常會是組織或公司的代表 domain 反著填寫
- `artifact` : 通常代表專案的名稱
- `name` : 專案名稱
- `description`:專案描述
- `packagename`:基本上就是 `group` 加上 `name`

最後再從右邊的 dependency 中選擇需要的套件。選擇好之後點選下方的 explore 來拿到 `.gradle` 或 `pom.xml` 在複製到專案中即可(通常會在根目錄）。

## Reference

- [iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10375547)

- [Spring Initializr Reference Guide](https://docs.spring.io/initializr/docs/0.4.x/reference/htmlsingle/#initializr-documentation-about)

- [How to Start Spring Boot Project in Community Edition of IntelliJ IDEA](https://www.youtube.com/watch?v=eZdQjgH7lyA)

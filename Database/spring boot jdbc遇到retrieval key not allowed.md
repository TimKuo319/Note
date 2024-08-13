
在`application.property`後方加上`allowPublickeyRetrieval

```xml
spring.datasource.url=jdbc:mysql://localhost:3306/stylish?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true

```


## What is mapper, and why we need to use it ?

在透過 spring boot 進行開發的過程中，我們很常會需要進行物件的轉換。最常發生這個狀況的地方會是在 `service` 及 `repository` 之間，對於從 db 撈回來的資料，我們可能會想要將他從 `entity` 轉換為只具備某些特定欄位的 response，或是收到 request 之後，想要將他轉換轉換成 `entity`。

以上的過程如果手動處理，可能就會在 service 層透過大量的 getter、setter 去達成，會讓 service 層變得比較冗長．而這就是 mapper 發揮作用的地方。     

## pom.xml 設定

```xml

<properties>
    <org.mapstruct.version>1.5.5.Final</org.mapstruct.version>
    <lombok.version>1.18.30</lombok.version>
</properties>

<dependencies>
    <!-- MapStruct -->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
    
    <!-- 如果有使用 Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
        <scope>provided</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <source>17</source>
                <target>17</target>
                <annotationProcessorPaths>
                    <!-- MapStruct processor -->
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- 如果使用 Lombok，需要加這兩個 -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>${lombok.version}</version>
                    </path>
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok-mapstruct-binding</artifactId>
                        <version>0.2.0</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```

>[!warning]
>在進行 mapstruct 安裝的時候與 lombok 一樣，只在 `dependencies` 中新增 mapstruct 是不夠的，還需要在下方 configuration 加上 mapstruct 的 annotation processor。讓 spring boot 能夠搭配 proceesor 去處理。
> 
>否則可能會遇到 mapper 無法注入到 spring boot IoC 中的問題。

## Usage 

### Basic

mapper 在使用上滿簡易的。最主要是透過宣告一個 interface，並在他上方加上 `@Mapper` ，並在其中進行 function 的宣告，mapper 就會在編譯期間產生出 function 的實作，進而達到物件之間的轉換。

```java
@Mapper(componentModel = "spring")  
public interface UserMapper {  
  
    UserDto toDto(User user);  
  
  
    @Mapping(target = "id", ignore = true)  
    @Mapping(target = "createdAt", ignore = true)  
    @Mapping(target = "updatedAt", ignore = true)  
    User toEntity(CreateUserDto userDto);  
}
```

在進行轉換時，如果有遇到不需要進行轉換的欄位，可以透過 `@Mapping` 搭配上 `ignore`，去忽略。在不寫的狀況下沒有 mapping 到的欄位預設不會填入，但如果有加上 annotation 就可以讓 mapping 時哪些欄位被忽略掉更清楚。
### Advanced

遇上有關聯的 entity 時可能會遇到以下的狀況。

```java
@Mapping(target = "cusId", source = "customer.cusId)
AccountResponseDto toDto(Account account);
```

- [ ] mapper vs rowmapper 

## Reference

- [使用MapStruct簡化物件轉換程式碼](https://www.tpisoftware.com/tpu/articleDetails/2443)







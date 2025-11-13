
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


- [ ] 基礎用法筆記

- [ ] 遇上有關聯的實體時如何做 mapping

- [ ] mapper vs rowmapper 






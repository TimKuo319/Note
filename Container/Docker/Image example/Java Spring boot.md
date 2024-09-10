
```Dockerfile 
FROM maven:3.8.5-openjdk-17 AS builder  
  
WORKDIR /app  
  
COPY pom.xml .  
  
COPY src ./src  
  
RUN mvn clean package  
  
  
FROM eclipse-temurin:17.0.12_7-jre-jammy  
  
WORKDIR /app  
  
EXPOSE 8080  
  
COPY --from=builder /app/target/Wits-Project-Calendar-0.0.1-SNAPSHOT.jar .  
  
CMD ["java", "-jar", "Wits-Project-Calendar-0.0.1-SNAPSHOT.jar"]
```

要注意的是，目前 OpenJDK 的 image 已經 ==Deprecated== ，在其官方文件中有指出其他替代方案。可以參考 [openjdk - Official Image | Docker Hub](https://hub.docker.com/_/openjdk) 因此上面選擇的是 `eclipse-temurin` 的版本


## Reference

[openjdk - Official Image | Docker Hub](https://hub.docker.com/_/openjdk)
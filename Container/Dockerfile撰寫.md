---
date: 2024-02-12 Tue 15:28
---
---

## Outline
+ [Format](##Format)
	+ [From](###From)
	+ [ENV](###ENV) 
	+ [RUN](###RUN)


## Format 

```dockerfile
FROM ruby:3.1.2-alpine  
ENV AUTHOR=guo
  
RUN apk add --update --no-cache \  
build-base \  
curl  
  
WORKDIR /app  
  
COPY . .  
  
RUN gem install bundler:2.3.19 && \  
bundle install -j4 --retry 3 && \  
bundle clean --force && \  
find /usr/local/bundle -type f -name '*.c' -delete && \  
find /usr/local/bundle -type f -name '*.o' -delete && \  
rm -rf /usr/local/bundle/cache/*.gem  
  
EXPOSE 3000  
  
CMD ["bundle", "exec", "ruby", "whoami.rb", "-p", "3000", "-o", "0.0.0.0"]
```

### From 

用來指定image的基底，如果是依照nodejs撰寫的就指定nodejs的映像檔等等。

### ENV

容器形成後的環境變數 -> 以上面的dockerfile來說，當容器啟動後，容器內會有一環境變數AUTHOR，其值為guo。

### RUN

終端機執行的指令，只要是能被該作業系統執行的指令都可以放在RUN中。
因為一個指令代表的就是一個image layer，太多指令會造成image layer混亂，像上面的寫法就會只有一層image layer。

### WORKDIR

設定工作目錄，會在container中創建該資料夾並且移動到資料夾內，使得往後的指令都在該資料夾內下執行。

### COPY

前面是file source，後面是target。上面的寫法會將當前資料夾複製到容器中當前資料夾下。
### EXPOSE

預設開啟的port。

### CMD

容器的啟動指令。當Image成為容器的時候第一個執行的指令。


## Reference

[4.9 Dockerfile 內容解析 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/images/explain-dockerfile)


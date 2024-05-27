---
date: 2024-02-12 Mon 15:28
---
---

## Outline
+ [Format](##Format)
	+ [From](###From)
	+ [ENV](###ENV) 
	+ [RUN](###RUN)
	+ [VOLUME](###VOLUME)
	+ [WORKDIR](###WORKDIR)
	+ [COPY](###COPY)
	+ [EXPOSE](###EXPOSE)
	+ [CMD](###CMD)
+ [撰寫順序](##撰寫順序)
+ [多階段建置](##多階段建置)
+ [Reference](##Reference)
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

### FROM

用來指定image的基底，如果是依照nodejs撰寫的就指定nodejs的映像檔等等。

### ENV

容器形成後的環境變數 -> 以上面的dockerfile來說，當容器啟動後，容器內會有一環境變數AUTHOR，其值為guo。

### RUN

終端機執行的指令，只要是能被該作業系統執行的指令都可以放在RUN中。
因為一個指令代表的就是一個image layer，太多指令會造成image layer混亂，像上面的寫法就會只有一層image layer。

### WORKDIR

設定工作目錄，會在container中創建該資料夾並且移動到資料夾內，使得往後的指令都在該資料夾內下執行。

### VOLUME
VOLUME後通常會接一個絕對路徑，其代表的意思是，在啟動容器時沒有指定volume的情況下，docker會`自動建立volume`，並將該volume指向容器內指定的這個絕對路徑。以下面的例子來說，就是會將volume指向容器內的`/var/lib/mysql`這個路徑

```dockerfile
VOLUME /var/lib/mysql
```

### COPY

前面是file source，後面是target。上面的寫法會將當前資料夾複製到容器中當前資料夾下。
### EXPOSE

預設開啟的port。

### CMD

容器的啟動指令。當Image成為容器的時候第一個執行的指令。

## 撰寫順序

若是docker file中的 docker image layer 上層重新建置，則儘管後面的image layer不變，docker依舊會重新bulid出新的image layer，而==不會觸發cache機制==，所以我們應該要將越不會變動的指令放在上方。

順序大致如下
+ FROM
+ ENV -> option(看變動程度決定位置)
+ EXPOSE
+ WORKDIR
+ RUN  -> 可能會需要為容器安裝新的package
+ COPY -> 因為當程式碼變動後，bulid時計算出來的sha id會不同，所以變動頻率通常最大


## 多階段建置

```dockerfile
FROM alpine:3.16.2 AS builder # 建置階段  
RUN echo 'Builder' > /example.txt # 建置階段  
  
FROM alpine:3.16.2 AS tester # 測試階段  
COPY --from=builder /example.txt /example.txt # 測試階段  
RUN echo 'Tester' >> /example.txt # 測試階段  
  
FROM alpine:3.16.2 # 最終階段  
COPY --from=tester /example.txt /example.txt # 最終階段CMD [ "cat", "/example.txt" ] # 最終階段
```

多階段建置，multi-stage build，目的在於縮小最終image的大小，提升image的效能以及縮短build過程的時間。

依照上面的dockerfile的範例，可以將image分為多個stage進行bulid，將每一個*FROM*視為一個階段，以第一個階段來說，會在example.txt中寫入Builder這個字，而到第二個階段時，透過`--from=builder`來取得上一個階段的結果，將上一個階段的example.txt內容複製一份到example.txt，最後再繼續傳遞到第三階段，最後第三階段的結果才會是生成的image結果。

我們可以透過這樣的特性，在建置階段的時候安裝所需的套件，但並不將它傳遞到最後做使用，像是tsc、ts-node等等，可以大大減少最終image的大小。



## Reference

[4.9 Dockerfile 內容解析 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/images/explain-dockerfile)
[4.10 建置映像檔 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/images/building)
[4.11 重新整理 Dockerfile 的執行順序 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/images/reorder-dockerfile)
[4.12 多階段建置映像檔 | 不可不知的 Docker 開發部署實戰筆記 | Robert Chang](https://docker.robertchang.me/images/multiple-building)
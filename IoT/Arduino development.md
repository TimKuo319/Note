---
date: 2024-03-15 22:16 Fri
---
---

# Arduino IDE

在課堂上進行IoT開發所使用的就是arduino IDE，透過arduino IDE來撰寫程式並且將這些程式燒錄到開發板上，使得開發板能執行我們想要的功能。

## 檔案系統(sketch book)

Arduino IDE的檔案系統跟大多數的IDE一樣，他的名字叫做==sketch book==，透過sketch book可以瀏覽有哪些`.ino`檔案。但比較不同的地方是，==每一個.ino檔案都需要有一個與他同名的資料夾==，能夠使這個.ino檔被arduino IDE正常使用。但詳細原因沒有去深究。

## Boards manager與Library manager

在開發的過程我們會需要依照開發板的不同去下載不同板子所需要的package，讓IDE能支援到開發板的功能，所以會需要透過==boards manager==來下載對應的board。而library manager也相同，以上課的例子來說，因為Arduino Nano Ble Sense Rev2上裝有多種感測器，其中像是HS300x系列的溫度傳感器想要透過arduino ide來操作話，就需要去安裝感測器對應的library才能夠做使用。


## Structure of .ino file

基本的`.ino`檔案內會有==void setup()==以及==void loop()==，這兩個函式。而他們所做的事其實也跟他們的名字一樣

+ *void setup()*
	+ 開發板在執行程式時，會先去執行setup()
	+ 內容主要是一些事前工作，像是指定腳位，設定baud rate等等
	+ 在每一個啟動階段，只會被執行一次
+ *void loop()*
	+ 無限的去重複執行loop()內的內容，以至於讓開發板能夠去執行我們想要做的事情
	+ 在loop()內可以直接使用在setup()中設定好的變數

## Serial port

在程式碼中我們可能會看到像是以下的程式
```C++
Serial.begin(9600);
Serial1.begin(115200);
```

以上課的狀況為例，我們透過Arduino Nano BLE Sense Rev2以USB的方式連接到電腦上，再將S7678S(LoRa模組)的tx/rx與Nano的tx/rx對接，透過==UART==的方式來在兩者之間傳遞訊息。

而上面程式碼中的`Serial.begin`是在設定序列阜的baud rate，在經過查詢一些資料後發現比較新的開發板的Serial通常都會指的是與電腦連接的序列阜(通常是用USB連接)。所以`Serial.begin(9600)`就是在設定電腦與Nano溝通的baud rate。

至於後面的`Serial1`就是指開發板上的tx/rx腳位，他們在開發板上透過UART來傳遞訊息給連接上的裝置。而會知道是`Serial1`是因為這是依照開發板規格得到的。像一些有較多Serial port的開發板，就可能有Serial2、Serial3等等，這時候就要依照官方的文件去檢查對應的部分。


## Reference

[loop() - Arduino Reference](https://www.arduino.cc/reference/en/language/structure/sketch/loop/)


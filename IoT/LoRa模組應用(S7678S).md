---
date: 2024-03-08 22:27
---
---

# 名詞解釋

+ UART
	+ 一種非同步的收發傳輸器，硬體。
	+ 將資料透過==串列通訊==進行資料傳輸
+ COM port
	+ 一種serial port，用來讓電腦識別裝置
+ Baud rate
	+ ==UART用來設定資料的傳輸速率==，速率越高，耗電量越大。
	+ 決定了UART裝置(在課堂狀況中是EK板)，跟連接裝置(在課堂狀況中是電腦)之間的資料傳輸速率。
+ Channel
	+ LoRa模組之間通訊的頻率
	+ 每個通道都有自己的頻率
	+ 假設有通道1~8
		+ 開發板利用channel 1 uplink訊息的時候，LoRa gateway也要有相同頻率的channel 1才能夠接收到。反之亦然。
	+ uplink跟downlink時利用的channel==不一定會相同==


三者之間的關係，以上課實作的狀況來說。

1. 透過USB將EK板接上電腦。
2. 進行termite設定
	1. 選取==com port==，這個地方的com port就是用來==辨識==EK板這個裝置。
	2. 設定Baud rate，決定資料在EK板跟電腦之間的傳輸速率。
3. sip reset重置開發板
	1. termite將指令透過UART傳遞給EK板
	2. EK板執行指令
4. mac join abp
5. 透過termite進行uplink
	

# Uplink及Downlink是如何運作的

## Uplink

1. 透過mac uncf 2 30e2指令進行uplink
	+ 參數可見==S7678S==指令手冊
2. EK板透過UART讀取指令並執行這個uplink指令
3. 透過天線將資料以LoRa packet的方式發送出去
4. 在接收範圍內的LoRa gateway接收到LoRa packet
5. LoRa gateway將資料傳輸到指定的MQTT server
	+ 以gateway的MAC address作為topic的一部分放到MQTT server上
6. 我們以MQTT client去subscribe該topic，取得想要的訊息。

## Downlink

1. 在MQTT client publish資料
	+ 資料內攜帶了終端裝置(Arduino開發板)的MAC address
2. MQTT server接收到資料
3. 傳遞回去給LoRa gateway
4. LoRa gateway依照MAC address在終端裝置開啟rx window的時候將資料傳遞給終端裝置
5. 在EK板透過UART將資料傳遞給termite顯示在螢幕上



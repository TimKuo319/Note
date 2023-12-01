
影像辨識用

channel
tensor


### Simplification 1
---
原因: 在使用全連接層的情況下，辨識圖片的參數會太多，可能會造成Overfitting(過度學習)的情況，可以將每個神經元指定到特定的區域(Receptive field)，該神經元只要負責該區域即可

#### Receptive-Field
用來作為給神經元辨識pattern的區塊，其大小稱為`kernel size`依照長及寬決定，假設選了3x3的pixel作為receptive field，則kernel size為`3x3`。每個Receptive field通常會有一組神經元來關注。

*Typical setting*

#### Stride
位移量，可以視為一個receptive field向垂直或水平方向跨了幾步後，會形成一個新的receptive field。

超範圍補0

### Simplificaion 2
---
Situation: 當重要特徵出現在不同receptive field，是否需要在每個receptive field都放置處理該重要特徵的neuron呢?

Solution: 否，只要將其中一個負責該特徵的neuron辨識的參數，透過`共享參數`的方式，交給負責不同區域的neruon即可。

#### Filter
共享同一組參數的神經元被稱作同一個filter，因此會有多組不同的filter。

#### Feature map
多個filter掃過圖片後，產生的數字群。

#### Pooling - 池化
藉由提取圖片中的特徵值來縮小圖片，進而減少運算量。常見代表`Max-Pooling`


## Summary - 何謂CNN

在神經網路上加入[Receptive-Field](#Receptive-Field)及參數共享概念的層稱為卷積層，而具備卷積層的神經網路則稱為CNN(Convolutional Nerual Network)

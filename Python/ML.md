
python曲面圖
ipywidgets
學習率
梯度下降(gradient descent) -> optimizer的其中一種

#### One-hot encoding : 
用來處理類別型特徵的一種方式，例如將衣服種類、國家等等轉換為數值供模型使用。

#### HyperParameter(超參數)

#### Train loss
在訓練過程中，每個epoch會取一小部分(batch)的資料作為輸入，並藉由神經網路進行向前傳播(Forward Propagation)來產生出預測的輸出結果，接著利用先前定義好的損失函數來計算訓練損失，接著再利用反向傳播(Back Propagation)來回傳損失對模型參數和梯度(?)的影響，進而達到優化模型的效果
#### Validation loss
將一小部分的訓練資料作為驗證集，在每一個epoch的特定時間點，帶入這些資料來驗證模型對於未知數據的處理情況。可以作為是否有產生過度學習的重要標準。

*註: 並不會被反向傳播回去做調整，而是用來衡量模型泛化程度的一個數據*

#### VGG網路優缺點


#### 如何解析Train loss及Val loss分布

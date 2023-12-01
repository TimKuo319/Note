
## 針對所有VM的設定方式

進到GCP畫面後，選擇`Compute engine`，從側邊欄中選擇中繼資料(metadata)，可以看到有新增`中繼資料`以及`安全殼層金鑰`，選擇`安全殼層金鑰`後，新增公鑰即可。

參考:[[實作]建立GCE並且使用SSH方式連線至GCE - JIAYI LEE - Medium](https://joylee850327.medium.com/%E5%AF%A6%E4%BD%9C-%E5%BB%BA%E7%AB%8Bgce%E4%B8%A6%E4%B8%94%E4%BD%BF%E7%94%A8ssh%E6%96%B9%E5%BC%8F%E9%80%A3%E7%B7%9A%E8%87%B3gce-4ac04809fa3e)


## 針對單一VM的設定方式

選擇完`Compute engine`之後，從VM執行個體中`點擊想要設定的VM`，之後會出現關於該VM instance的詳細資料，往下滑會看到`安全殼層金鑰`，若是想要新增的話就點擊上方的編輯按鈕，在此處編輯即可新增SSH key，貼上自己的公鑰就可以完成。


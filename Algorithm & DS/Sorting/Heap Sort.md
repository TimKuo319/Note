
- heap 種類
	- min heap
		- parent node 一定比 chlid node 小
	- max heap
		- parent node 一定比 child node 大


heap sort 主要分為兩個步驟

1. 建立 heap
	 - 從 n/2 也就是最小的最右邊子樹開始檢查，過程持續透過 heapify 檢查到 root

2. heapify
	- 在每一次取出值後，先將 root 與 最右子樹葉節點交換，並將 heap size - 1。
	- 再重複做 heapify

## Reference

- [Day21:[排序演算法]Heap Sort - 堆積排序法 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10266206)

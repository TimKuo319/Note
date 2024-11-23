---
tags:
  - DataStructrue
---
## Collection 

Collection 是除了 Map 以外其他集合類的 root interface。主要有以下三種集合：

- List
- Set
- Queue

我們常見的各種資料結構 (Map 除外) 就是透過去實作這些介面來對應到不同的特性。以 List 來說，就有 ArrayList、LinkedList、Vector 等不同的實作。透過 Collections 來統一這些 interface 的行為，讓集合類標準化。

以下是一些整理後常用的類別

## List

- ArrayList
- LinkedList
- Vector

### 常見使用方法

- `add(E e)` - 加入元素到尾端
- `remove(int index)` - 移除指定位置元素
- `remove(Object o)` - 移除第一個與指定對象相等的元素
- `size()` - 取得 list 元素數量
- `get(int index)` - 取得指定 index 的元素
- `contains(Object o)` - 判斷 list 中是否存在某元素
## Set

- HashSet
- LinkedHashSet

### 常見使用方法

- `add(E e)` - 加入元素到集合
- `remove(Object o)` - 移除指定對象相等的元素
- `size()` - 取得 set 元素數量
- `get(int index)` - 取得指定 index 的元素
- `contains(Object o)` - 判斷 list 中是否存在某元素
- `claer()` - 清空集合中的元素

## Queue

- LinkedList
- PriorityQueue
- ArrayDequeue

### 常見使用方法

- `add(E e)` - 加入元素到 queue 最後，如果失敗就拋出異常
- `poll()` - 移除並返回 head 元素，queue 為空時回傳 null
- `remove` -  移除並返回 head 元素，queue 為空時拋出異常
- `peek()` - 返回 head 元素，但不移除

## Map

- HashMap 
- LinkedHashMap
- TreeMap

### 常見使用方法

- `put(K key, V value)`
- `get(Object key)`
- `remove(Object key)`

## 特殊類別

- Stack
	- Stack 是繼承自 Vector 的子類別
	
### 常見使用方法

- `push(E e)` - 新增元素到 stack 中
- `pop()` - 移除並返回 top 元素，stack 為空時拋出異常
- `peek()` - 返回 top 元素，但不移除

## Reference

- [Java集合简介 - Java教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/java/collection/quick-start/index.html)
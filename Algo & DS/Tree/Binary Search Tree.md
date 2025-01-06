
## 定義


>1. 對於根節點，左子樹中所有節點的值 < 根節點的值 < 右子樹中所有節點的值。
>2. 任意節點的左、右子樹也是二元搜尋樹，即同樣滿足條件 `1.` 。

## BST 的操作

### 查詢

就如同 binary search 一樣，透過去比較節點的值，一次篩選掉一半的可能性，時間複雜度為 O(logn)

### 插入

1. 先透過 binary search 找到節點要插入的位置，當走訪到 null 的時候就退出迴圈。
	- 因為插入的新節點一定會是葉節點，所以只會在最下層
2. 透過一個 pre 變數來記錄先前節點的位置，輔助完成節點的插入。

```java
/* 插入節點 */
void insert(int num) {
    // 若樹為空，則初始化根節點
    if (root == null) {
        root = new TreeNode(num);
        return;
    }
    TreeNode cur = root, pre = null;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != null) {
        // 找到重複節點，直接返回
        if (cur.val == num)
            return;
        pre = cur;
        // 插入位置在 cur 的右子樹中
        if (cur.val < num)
            cur = cur.right;
        // 插入位置在 cur 的左子樹中
        else
            cur = cur.left;
    }
    // 插入節點
    TreeNode node = new TreeNode(num);
    if (pre.val < num)
        pre.right = node;
    else
        pre.left = node;
}
```

### 刪除

刪除的話則相對複雜一些。

當要刪除的節點只有 0 / 1 個子節點時，同樣是透過 pre 去記錄前一個節點的位置，並直接將 pre 的左子樹或右子樹接上其存在的節點即可。

如果是兩個子節點的情況，為了符合 BST **左子樹 < 根節點 < 右子樹** 的定義，當根節點被刪除的時候，應該要拿 *左子樹最大值或右子樹最小值來* 來當根節點，實際上並不是真正的刪除根節點的位置，而是**拿葉節點的值覆蓋根節點的值，並刪除葉節點**。

```java
/* 刪除節點 */
void remove(int num) {
    // 若樹為空，直接提前返回
    if (root == null)
        return;
    TreeNode cur = root, pre = null;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != null) {
        // 找到待刪除節點，跳出迴圈
        if (cur.val == num)
            break;
        pre = cur;
        // 待刪除節點在 cur 的右子樹中
        if (cur.val < num)
            cur = cur.right;
        // 待刪除節點在 cur 的左子樹中
        else
            cur = cur.left;
    }
    // 若無待刪除節點，則直接返回
    if (cur == null)
        return;
    // 子節點數量 = 0 or 1
    if (cur.left == null || cur.right == null) {
        // 當子節點數量 = 0 / 1 時， child = null / 該子節點
        TreeNode child = cur.left != null ? cur.left : cur.right;
        // 刪除節點 cur
        if (cur != root) {
            if (pre.left == cur)
                pre.left = child;
            else
                pre.right = child;
        } else {
            // 若刪除節點為根節點，則重新指定根節點
            root = child;
        }
    }
    // 子節點數量 = 2
    else {
        // 獲取中序走訪中 cur 的下一個節點
        TreeNode tmp = cur.right;
        while (tmp.left != null) {
            tmp = tmp.left;
        }
        // 遞迴刪除節點 tmp
        remove(tmp.val);
        // 用 tmp 覆蓋 cur
        cur.val = tmp.val;
    }
}
```


## In-Order traversal 具備順序

因為 BST 的結構關係，當我們以 In-Order 的方式去走訪樹的時候就會得到一個排序好的結果。

## Usage

1. DB Index
2. 資料流的有序儲存

## Reference

- [7.4   二元搜尋樹 - Hello 演算法](https://www.hello-algo.com/zh-hant/chapter_tree/binary_search_tree/)

- [iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10322678)

BST Map 和 BST Set 指的是底層透過 BST 實作的 Map 和 Set，這種實作的優勢在於，透過 BST 的特性能夠讓插入、刪除、查找的平均時間複雜度為 O(log n) 。

雖然說是透過 BST 來實作，但因為在樹的極端狀況下，BST 就會退化成 linked list 而導致效率變差，因此通常會使用**自平衡**的 BST 像是 **AVL樹** 或 **紅黑樹**等，來確保時間複雜度維持在 O(log n)

以 Java 來說，Map 的其中一種實作 **TreeMap** 就是屬於 BST map 的一種。 


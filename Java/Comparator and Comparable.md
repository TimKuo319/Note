---
tags:
  - Java
---

在 java 中，有 `Comparator` 以及 `Comparable` 兩種介面。兩者乍看之下都是在比較物件之間的關係，但使用場景略有不同。


## Comparable

`Comparable` 本身是屬於 `java.lang` 中的介面，主要是透過物件直接實作 `Comparable` 介面來讓物件本身具有一個自然的排序方式。

以下面的方式來說，`Task` 物件本身實作了 `Comparable` 介面，並且覆寫了 `compareTo` 的方法，讓 `Task` 物件本身具備一個自然比較的方式。

而對於 `Collections.sort` 這個方法來說，如果只有傳入一個參數(也就是要排序的目標的話)，就可以直接依照 `compareTo` 所定義的方式去進行排序 

```java
class Task implements Comparable<Task> {
    String name;
    int priority;

    public Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }

    @Override
    public int compareTo(Task other) {
        return Integer.compare(this.priority, other.priority); // 按優先級升序
    }

    @Override
    public String toString() {
        return "Task{name='" + name + "', priority=" + priority + "}";
    }
}

public class ComparableExample {
    public static void main(String[] args) {
        List<Task> tasks = new ArrayList<>();
        tasks.add(new Task("Task1", 3));
        tasks.add(new Task("Task2", 1));
        tasks.add(new Task("Task3", 2));

        Collections.sort(tasks); // 自然排序（升序）
        System.out.println(tasks);
    }
}
```

## Comparator

`Comparator` 介面本身屬於 `java.util` 中的一個介面，相對於 `Comparable` 需要在要比較的類別中直接進行覆寫，且只能針對一種屬性進行比較。`Comparator` 擁有更大的彈性。

從 


## Reference

[Java 中 Comparator 和 Comparable 的区别-阿里云开发者社区](https://developer.aliyun.com/article/1593685)

[使用PriorityQueue - Java教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/java/collection/priority-queue/index.html)
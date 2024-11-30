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

從 [Comparable](##Comparable) 中的程式碼可以發現，`compareTo` function 只需要另外一個參數，是透過當前物件和另外一個物件比較來達到比對的效果。而這樣的寫法會需要在物件內直接進行比較邏輯撰寫，未來要修改時要會需要改動物件內部的程式碼。

而 `Comparator` 本身的話則是在外部定義比較的邏輯，透過覆寫 `compare` ，可以定義不同屬性的排序方式，只要在要比較的地方傳入對應的 comparator 即可。

```java
import java.util.Comparator;

// 定義 Task 類別
class Task {
    private String name;
    private int priority;

    public Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }

    public String getName() {
        return name;
    }

    public int getPriority() {
        return priority;
    }

    @Override
    public String toString() {
        return "Task{name='" + name + "', priority=" + priority + "}";
    }
}

// 實作 Comparator 介面，根據名稱排序
class TaskNameComparator implements Comparator<Task> {
    @Override
    public int compare(Task t1, Task t2) {
        return t1.getName().compareTo(t2.getName());
    }
}

// 主程式
public class ComparatorExample {
    public static void main(String[] args) {
        List<Task> tasks = new ArrayList<>();
        tasks.add(new Task("TaskC", 3));
        tasks.add(new Task("TaskA", 1));
        tasks.add(new Task("TaskB", 2));

        // 使用 TaskNameComparator 進行排序
        tasks.sort(new TaskNameComparator());

        // 輸出排序後的結果
        for (Task task : tasks) {
            System.out.println(task);
        }
    }
}

```



>[!info]
>在 Java 中，`Comparator` 介面的 `compare(T o1, T o2)` 方法用於比較兩個物件的順序，其返回值決定了這兩個物件在排序時的相對位置：
>- **負數**：表示 `o1` 應該排在 `o2` 之前。
>- **零**：表示 `o1` 和 `o2` 的順序相同。
>- **正數**：表示 `o1` 應該排在 `o2` 之後。


## Summary

總結來說，可以將兩者的比較歸類為以下的表格

| 特性       | `Comparable`                                   | `Comparator`                                        |
| -------- | ---------------------------------------------- | --------------------------------------------------- |
| **所在套件** | `java.lang`                                    | `java.util`                                         |
| **排序方式** | 定義物件的**自然排序**                                  | 定義物件的**自訂排序**                                       |
| **實作位置** | 由物件類別自行實作，需修改原始碼                               | 由外部定義，無需修改物件類別                                      |
| **方法**   | `compareTo(T o)`                               | `compare(T o1, T o2)`                               |
| **使用場景** | 當類別有固定的排序需求                                    | 當需要多種排序方式或無法修改類別時                                   |
| **實作數量** | 每個類別只能有一個 `compareTo` 實作                       | 可為同一類別定義多個 `Comparator` 實作                          |
| **範例**   | `class MyClass implements Comparable<MyClass>` | `class MyComparator implements Comparator<MyClass>` |

## Reference

[Java 中 Comparator 和 Comparable 的区别-阿里云开发者社区](https://developer.aliyun.com/article/1593685)

[使用PriorityQueue - Java教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/java/collection/priority-queue/index.html)
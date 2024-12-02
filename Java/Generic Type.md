
## Generic Class and Generic Member

```java

public class Box<T> {
    private T content;

    public void setContent(T content) {
        this.content = content;
    }

    public T getContent() {
        return content;
    }
}

```


上面的方法中，透過在class後面加上`<T>`來宣告這是一個泛型類別

```java
Box<String> stringBox = new Box<>();
stringBox.setContent("Hello");
String content = stringBox.getContent();

Box<Integer> integerBox = new Box<>();
integerBox.setContent(123);
Integer number = integerBox.getContent();
```

## Generic method

```java

public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}

```

透過在回傳值前面加上`<T>`來表示這是一個Generic method


## Bounded Type Parameters

如果希望限制泛型的範圍，可以透過`extends`來加上限制，下面的寫法限制了要實例化`processNumber`這個class的話，使用的型別只能是`Number`的子類別，像是`Integer`、`Double`、`Float`等等

```java
public <T extends Number> void processNumber(T number) {
    System.out.println(number.doubleValue());
}

```

`extends`同樣適用於介面，如果要指定類型必須實作某個類別，寫法也是用`extends`

```java

public interface Printable {
    void print();
}

public <T extends Printable> void processPrint(T printable) {
    printable.print();
}

```

## Comparison

有
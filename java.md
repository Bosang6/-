### 单元测试

安装依赖

```
junitxxx.jar
hamcrest-core-xx.jar
```

通过@Test来进行单元测试

```java
@Test
public void test() {
     System.out.println("Test");
}
```

#### 使用注意事项

- 所在的类（JUnitTest类）必须是public，非抽象的，包含唯一的无参构造器。

- @Test标记的方法必须是public，非抽象，非静态，void无返回值，（）无参数的。

- Scanner在没有经过特殊处理是，可能存在无法读取键盘信息问题。

  解决方法：

  在IDEA中，Help -> 编辑自定义虚拟机选项，添加一行代码

  ```
  -Deditable.java.test.console=true
  ```

  重启后即可修复问题。

#### 单元测试模板



## 新特性

### Lambda表达式

箭头操作符 (→)

- 左边：形参列表。

- 右边：lambda体，实现接口类中，需要重写的方法体。

```
（lambda形参列表） -> lambda方法体
```

#### 无参数形式

Runnable接口实现run方法

```java
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello world!");
    }
};
r1.run();
```

在Lambda表达式中，将多余的东西删除

```java
Runnable r2 = () -> {
    System.out.println("Hello world!");
};
r2.run();
```

#### 有参数形式

Comparator比较类

```java
Comparator<Integer> comparator = new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return Integer.compare(o1, o2);
    }
};
```

Lambda半删形式

```java
Comparator<Integer> comparator1 = (o1,o2) -> {
    return Integer.compare(o1, o2);
};
```

全删形式

```java
Comparator<Integer> comparator1 = (o1,o2) -> Integer.compare(o1, o2);
```

注意：删除大括号必须要把return一起删除，否则报错。

### 函数式接口

Functional Interface,只能拥有一个待实现的方法。

```java
public interface MyFunctionalInterface {
    void myMethod();
}
```

只有为函数式接口提供实现方法的类才可以使用lambda表达式。

```java
MyFunctinalInterface m = () -> {
	方法体
};

m.myMethod();
```

#### 四大核心函数式接口

- Consumer 消费型接口

  ```java
  Consumer<T>   void accpt(T t)
  ```

- Supplier 供给型接口

  ```java
  Supplier<T>   T get() //返回一个T
  ```

- Function 函数型接

  ```java
  Function<T,R>  R apply(T t)  //返回一个R
  ```

- Predicate 判断型接口

  ```java
  Predicate<T>   boolean test(T t)  //返回一个布尔值
  ```

  
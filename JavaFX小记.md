## Lab1

### fxml文件

在添加一个标签时，请务必添加它的导入包

```xml
<?import javafx.scene.text.Text?>

<Vbox shuxing="...">

  <Text fx:id="xxx"></Text>  // 记得导包！！！
  
</Vbox>
```

### Controller.java

在fxml内声明了一个标签，可以标签为变量类型定义变量，**记得一定要导包**！！

```java
import javafx.scene.text.*;

public class Controller {
  private Text xxx;  				// 标签为变量类型， fx:id 为变量名
}
```

### fxml调用Controller的函数

在fxml内，一定会要以#号来调用

```java
<?import javafx.scene.control.Button?>

<Button text="genera" onAction="#insertClick"/>

------------------------------------------------------
  
protected void insertClick(){
  ...
}
```

## Lab2

### initialize方法

该方法时Initializable接口的一部分，在FXML文法加载后自动调用initialize方法,用于初始化控制器类的状态。在该方法内部，可以执行所有的初始化任务，比如设置UI组件的初始状态，绑定关系，加载数据，添加事件监听器等。

### textProperty

是一个用于UI控件的属性，返回一个StringProperty对象，可用于实现绑定数据和监听文本值变化

```java
private TextField = infoText

public void initialize(){
  infoText.textProperty().bind(nameField.textProperty()); //只能在后者输入，前者只能显示相同信息
}
```

textProperty允许添加监听器来监听infoText值的变化

#### addListener监听器

我们可以为textProperty添加一个监听器（Listener），可以监控infoText的变化，保存旧值和新值

```java
textField.textProperty().addListener((observable, oldValue, newValue) -> {
	System.out.println("旧值：" + oldValue + "新值：" + newValue);
});
```

#### bind绑定器

bind方法可以将一个属性绑定到另外一个属性上。

单向绑定：属性1绑定属性2，当属性1发生变化时，属性2也会随着变化，但是属性2变化的话，属性1无变化

双向绑定：bindBidirectional方法，属性之前彼此影响双发的值，即有一方发生改变，另外一方也会随之变化

绑定接触：unbind方法

```java
@FXML
private Text infoText1;
private Text infoText2;

infoText1.textProperty().bind(infoText2);//infoText1单向绑定infoText2，在1改变时，2也会随之改变
```



#### StringExpression动态字符串

StringExpression是一个可动态变化的字符串，允许开发者执行动态字符串操作，比如连接、子字符串提取等。

StringExpression的操作是可绑定的，可以通过监听器来查看值的变化，确保显示最新的值

与常规String的区别：

- String 不是动态的，即声明过后在堆空间中存在并且无法改变，只能通过新new一个String对象来更新想要的内容，而原String仍然会保留在堆中，等待GC来回收

代码演示

```java
import javafx.beans.binding.StringExpression;
import javafx.beans.binding.Bindings;//用于 Bindings.concat 拼接动态字符串 StringExpression

TextField textField1 = new TextField("Hello");
TextField textField2 = new TextField("World");

StringExpression combinedText = Bindings.concat(textField1.textProperty(), " ", textField2.textProperty());

combinedText.addListener((observable, oldValue, newValue) -> {
    System.out.println("Combined Text: " + newValue);
});
```

#### ReadOnlyBooleanWrapper

该类提供一种机制，创建一个**布尔类型**的属性，该属性只能由创建者或内部修改。同时向外部暴露，使得外部可以通过监听器来读取属性值，但不能直接修改它。

```java
import javafx.beans.property.ReadOnlyBooleanWrapper;
private ReadOnlyBooleanWrapper BooleanValue = new ReadOnlyBooleanWrapper(false); //设定初始值为false

BooleanValue.set(true); //在内部可通过set方法来改变布尔值
```

#### visibleProperty

通过查看布尔值来判断是否执行操作。

```java

```


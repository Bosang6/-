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

03-26 Lab课，待补充

textProperty()

```java
public void initialize(){
  infoText.textProperty().bind(nameField.textProperty());
}
```

```java
import javafx.beans.binding.Bindings;
Binding.concat("" + "")
```

```
new ReadOnlyBooleanWrapper(false);
```


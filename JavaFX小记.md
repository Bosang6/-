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

在fxml内，一定要以#号来调用

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

## 菜广教学

### JavaFX应用程序的基本结构

![image-20240410211352747](C:\Users\a1173\AppData\Roaming\Typora\typora-user-images\image-20240410211352747.png)

#### 应用程序入口

入口类需要继承Application类，并实现start方法。

在main中，需要调用Application.lunch方法，该方法会自动调用start方法。

```java
import javafx.application.Application; //导包

public class MyApplication extends Application {	//继承Application抽象类
    @Override
    public void start(Stage stage) throws IOException {	//实现start方法
        FXMLLoader fxmlLoader = new FXMLLoader(MyApplication.class.getResource("hello-view.fxml"));		//获取fxml布局信息
        Scene scene = new Scene(fxmlLoader.load(), 320, 240);	//新建一个窗口
        stage.setTitle("Es1.3");				//窗口标题
        stage.setScene(scene);					//将场景布局到窗口内
        stage.show();							//显示窗口
    }

    public static void main(String[] args) {
        launch();					//main方法调用launch方法，该方法会调用start方法
    }
}
```

#### Application类的三大重写方法

在JavaFX中，入口类中需要重写三个方法

- start ：用于建立窗口以及窗口内的场景和布局
- init ：初始化内容，例如数据库连接等。
- stop ：在应用程序关闭时调用该方法。 

执行顺序：init → start → stop

#### 事件绑定

```java
Button button = new Button("按钮名字");			//新建一个按钮
BorderPane pane = new BorderPane(button);		//BorderPane布局，将button引入的窗口内

button.setOnClick( e -> {						//为按钮绑定一个回调函数
	getHostServices().showDocument("univr.it");		//通过getHostServices方法，打开系统默认的浏览器，通过showDocument方法，来决定进入哪个网站
});
```

### Stage窗口设定

#### 标题设定

在start方法中，通过stage.setTitle方法来设定窗口名。

```java
stage.setTitle("我是窗口名");
```

#### 窗口图标

通过getIcons()add()方法来设置应用程序图标。

```java
stage.setIcons().add(new Image("图标路径"));
```

#### 窗口大小变化

通过setResizeable方法来设定窗口是否可以自定义拖拽大小，默认情况下为true。

```java
stage.setResizeable(false);
```

#### 窗口风格设置

##### 默认风格

在默认风格情况下，窗口栏拥有

- 图标
- 窗口名
- 最小化/放大/关闭 栏

默认风格可以手动设置也可以不设置。

```java
stage.initStyle(StageStyle.DECORATED);
```

##### 无窗口风格

无窗口框，当没有内容时啥也不显示

```java
stage.initStyle(StageStyle.UNDECORATED);
```

##### 标题关闭风格

只有标题和关闭窗口按钮

```java
stage.initStyle(StageStyle.UTILITY);
```

#### 窗口模式

在应用程序打开多个窗口时，窗口之间存在多种关系，例如：A窗口和B窗口同时存在，B窗口由A窗口开启，在B窗口没有被关闭时，无法对A窗口进行操作。

##### 模式设置

```java
stage.initModality(Modality.模式);
```

模式：

- NONE：默认模式，多个窗口之间可以随意控制
- APPLICATION_MODAL：当该窗口被打开时，用户只能操控该窗口，无法操作别的窗口。

- WINDOW_MODAL：与APPLICATION模式不同，允许操作别的子窗口，不允许父窗口进行操作。

#### 父窗口设定

```
stage.initOwner(FatherStage);
```

stage窗口为FatherStage的子窗口

#### 自定义关闭按钮

通过自定义关闭按钮，增加与用户的交互性。

例如：在用户点击关闭按钮时，在默认情况下，系统会直接关闭窗口。我们可以通过自定义按钮，在用户点击关闭按钮时，提示用户是否需要关闭。

步骤：

- 消费（取消）默认关闭

  取消操作系统退出的动作

```
Platform.setImplicitExit(false);
```

- 为关闭按钮提供一个新的事件函数

```java
state.setOnCloseRequest(event ->{
	event.consume();	//取消关闭窗口动作
  Alert alert = new Alert(Alert.AlertType.CONFIRMATION);	//新建一个提示弹窗
  alert.setTitle("退出程序");	//弹窗标题
  alert.setHeaderText(NULL);
  alert.setContentText("确认退出程序？");	//弹窗内容
  
  Optional<ButtonType> result = alert.showAndWait();	//为弹窗设定两个按钮，并且等待用户点击按钮，返回到result
  if(result.get() == ButtonType.OK){ //当返回值为确认时
    Platform.exit();	//退出程序
  }
});

```

#### close方法

此方法只会关闭窗口，但应用程序仍然在执行。

```java
stage.close();
```

### Scene场景

通过Scene类来创建一个场景，参数为：

- 布局： 可以是FXML布局文件，也可以是javaFX类中自带的类的实例
- 长度
- 高度

```java
Scene scene = new Scene(布局，长度，高度);
```

#### 场景改变鼠标图片

```java
scene.setCursor(new ImageCursor(new Image(图片url地址)));
```

通过在不同的场景切换鼠标图片，在游戏领域非常有用。

### Node类

Node类是一个抽象类，在JavaFX中，所有的组件（控件）都继承于Node类。该类提供了多种对场景控制的属性，例如坐标等。

#### 常用的控制属性

建立一个视图控件

```java
Label label = new Label("Hello World!");
```

我们可以为label实例赋予许多场景控制属性。

- setLayoutX(int) ：X轴坐标
- setLayoutY(int) ：Y轴坐标
- setStyle("-fx-....")：与CSS一样，但需要在前面添加-fx-
- setPreWidth(int)：宽度设置
- setPreHeight(int)：高度设置
- setAlignment(Pos.位置)
- setVisible(boolean)：是否可见
- setblendMode()：混合模式，叠加部分会混合，用于图画软件
- setOpacity(int)：可见度设置
- setRotate(int度数)：旋转
- setTranslateX(int)：平行平移
- setTranslateY(int)：纵向平移

### UI控件的属性绑定和监听器

在JavaFX中，属性绑定和监听由Property接口来完成。其内部含有一个bind方法来绑定。

- bind：单向绑定
- bindBidirctional：双向绑定
- Unband/unbindBidirctianl：解除绑定

#### 属性绑定

例：场景与窗口的大小绑定

在start方法中，建立一个场景，该场景内有一个黑色边框，白色实心的圆。

```java
public void start(Stage stage) throws Exception {
  	AnchorPane root = new AnchorPane();
    Scene scene = new Scene(root,500,500); 
    Circle circle = new Circle();
    circle.setCenterX(250);
    circle.setCenterY(250);
    circle.setRadius(100);
    circle.setStroke(Color.BLACK);
    circle.setFill(Color.WHITE);	
  
    root.getChildren().add(circle);

    stage.setScene(scene);
    stage.show();
}
```

该园不会随着窗口的大小变化而改变大小，这不是我们所要的。我们希望该圆也会随着窗口的大小变化而变化。

我们可以将圆的XY坐标绑定窗口的坐标

```java
circle.centerProperty().bind(scene.widthProperty().divide(2));
circle.centerProperty().bind(scene.heightProperty().divde(2));
```

#### 监听器

```java
xxxProperty().addListener(new ChangeListener<number>() {
	@Override
	public void changed(ObservableValue<? extends Number> observable, Number oldValue, Number newValue){
		...
	}
});
```

addListener方法需要一个监听器实例作为参数，该参数实例含有一个必须要重写的changed方法，该方法含有三个参数：

- Observale：被监听的对象属性
- oldValue：属性旧值
- newValue：属性新值

可以以lambda表达式来书写，使得更加可读

```java
xxxProperty().addListener((observable, oldValue, newValue) -> {
	System.out.println("旧值：" + oldValue + "新值：" + newValue);
});
```

监听器和绑定的使用因情况而论，当业务逻辑相对简单时，可以使用绑定方法，而当业务逻辑比较繁琐时，例如一个操作将会触发一些类的操作时，监听器更为方便。

### 事件驱动编程

当用户在应用界面进行操作时，每个操作事件需要对应一系列的事件处理函数或事件处理对象来完成操作。

例如：点击一个按钮完成登陆操作。

一旦一个用户对应用程序做出某个操作，应用程序会自动调用操作所对应的事件函数，并给函数传递一个**Event对象**。

#### Event对象

该对象包含了很多信息，例如鼠标的坐标等。通过

```java
getSource();
```

此方法可以获取event对象的事件元，即触发该事件的UI组件。

#### 事件处理函数

事件处理函数通常以setOnXXX形式出现，该事件函数需要new一个EventHandler<XXXEvent>对象，该对象需要重写handle方法，且唯一。这意味着我们可以通过lambda表达式来完成。

例：按钮触发

匿名类形式重写

```java
button.setOnAction(new EventHandler<ActionEvent>(){
	@Override
	public void handle(ActionEvent event){
		lable.setLayoutY(lable.getLayout() - 5);
	}
});
```

lambda形式重写

```java
button.setOnAction( event -> lable.setLayoutY(lable.getLayout() - 5));
```

#### 文件拖拽获取绝对路径

```java
TextField textField = new TextField();
textField.setLayoutX(500);
textField.setLayoutY(350);

textField.setOnDragOver(event -> {
  event.acceptTransferModes(TransferMode.ANY);  //当将文件拖拽到textField时，会显示一个拖拽图标
});

textField.setOnDragDropped(event -> { 	//当松开鼠标时
  Dragboard dragboard = evenet.getDragborad(); //获取容器
  if(dragboard.hasFiles()){ //检查容器内是否有文件
    String path = dragboard.getFiles().get(0).getAbsolutePath(); //获取容器内的第一个文件的绝对路径
    textField.setText(path); //显示在textField内
  }
});
```


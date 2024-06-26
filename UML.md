### 用例 Use case

关系：

- 关联：实线
- 包含：include 虚线指针
- 扩展：extend 虚线指针
- 泛化：generalizzation

关联：

在主要使用者至少要和系统中的一个用例交互，需要用实线连接，也称为关联。

包含：

在一个特定的用例被触发后才触发，成为称为包含。

即基本用例被触发时，触发包含用例

例如：在客户登录时，验证密码才会被触发。

基本用例用虚线指针指向包含用例。

扩展：

在基本用例被触发时，扩展用例有时候会触发，有时候不会触发。只有在特定条件被满足后才会触发。

扩展用例用虚线指针指向基本用例。

泛化：继承

通用用例（General Use Case）具有多个特殊用例（Specialized Use Cases）。

例：付款方式，可以通过储蓄账户或信用账户来付款。付款为通用用例，每个账户为特殊用例。

特殊用例以实线和空心箭头指向通用用例。

### 时序图 Diagramma Sequenziale

- 参与者：小人表示
- 对象：矩形表示

建立完参与者和对象后，分别为他们添加一条向下的虚线，即为生命线。

在请求一个动作时，需要以实现连接，为请求消息

在返回一个消息时，需要虚线连接，为返回消息

在对象之间的虚线添加一条激活框，空心矩形。框住第一条消息和最后一条消息，意味着对象在哪个期间活跃。

Opt: 类似于if

alt：类似于if else

### 活动图 Diagramma Attività

展示了一个活动到另外一个活动的控制流程，显示系统的活动顺序。

- 实心圆形：表示活动的开始

  一个活动中只有一个开始节点

- 圆角矩形：动作节点，表示需要执行的任务。

  是一个原子性操作

- 空心棱形：决策节点，类似于if语句

  ​		也可以是合并节点，合并不同条件的分支，并执行同一个后续流程

- 实心粗线：fork，指的是一个活动后，多个分支异步执行一些活动，实心粗线前记得添加一个决策点空心棱形

  ​		   join, 指的是在某个地方等待所有异步执行的流程执行完毕后，再合并成同一个流程执行 

- 实心圆外再圈一圈：结束节点，一个流程允许存在多个结束节点



### 类之间的关系

继承关系 eredità

![image-20240610094932211](https://raw.githubusercontent.com/Bosang6/Pic_Go_Save/main/picgo/202406100949389.png)

关联 associazione

![image-20240610095057710](https://raw.githubusercontent.com/Bosang6/Pic_Go_Save/main/picgo/202406100950910.png)

聚合 aggregazione

整体可独立的部分

![image-20240610095234925](https://raw.githubusercontent.com/Bosang6/Pic_Go_Save/main/picgo/202406100952149.png)

乌龟时龟群可独立的部分

组合 composzione

![image-20240610095343239](https://raw.githubusercontent.com/Bosang6/Pic_Go_Save/main/picgo/202406100953360.png)

当父类被拆除时，子类也将不存在

多重关系

![image-20240610095512167](https://raw.githubusercontent.com/Bosang6/Pic_Go_Save/main/picgo/202406100955392.png)

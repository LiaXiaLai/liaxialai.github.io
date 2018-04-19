---
title: 设计模式 - 结构型模式
date: 2018-03-23 12:52:43
tags: 
  - 敏捷开发
---

结构型模式的目的是将类或者对象结合在一起形成更大的结构，就如搭积木一样，通过简单的积木组合形成复杂的、功能更加强大的结构。

结构型模式大致分为类结构型模式和对象结构型模式。

* 类结构型模式：关心类的组合，有多个类可以组合成一个更大的系统，一般只会存在继承关系和实现关系。
* 对象型模式：关心类与对象的组合，通过关联关系使得在一个类中定义另一个类的实例对象，然后通过该对象调用其他方法。根据「合成复用原则」，尽量使用关联关系来替代继承关系，因此结构型模式大部分采用对象型模式。

对象型模式主要的包含有：

* 适配器模式
* 桥接模式
* 组合模式
* 装饰模式
* 外观模式
* 享元模式
* 代理模式

# 适配器模式

![Screen Shot 2018-03-24 at 11.08.32 PM.png](https://i.loli.net/2018/03/24/5ab66a06634a2.png)

我国的标准电压为 220V，而 MacBook 不需要这么高的电压支持。于是，都会附赠一个电压适配器，将 220V 电压转换成合适自己的电压。适配器模式的目的亦是如此，将第三方接口转换成自己所需要的形式。

## 1. 模式动机

如果两个类需要配合工作，然后它们的接口又不兼容的时候，需要一个适配器来将两者的接口进行适配，使得两个类能配合工作。

## 2. 模式定义

适配器模式，又称为「包装器」，将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以在一起工作。

## 3. 模式结构

* Target：目标抽象类
* Adapter：适配器类
* Adaptee：适配者类
* Client：客户类

对于适配器模式来说，分为：类适配器和对象适配器。

对象适配器：

![Adapter.jpg](https://i.loli.net/2018/03/24/5ab66c5cbeca1.jpg)

类适配器：

![Adapter_classModel.jpg](https://i.loli.net/2018/03/24/5ab66ee63095f.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 适配器模式
 * @author: Keating Smith
 * @create: 2018-03-24 23:20
 **/
public class AdapterPattern {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request();
    }
}

/**
 * 适配器的父类
 */
interface Target {
    // 声明所有适配的公共方法
    void request();
}

/**
 * 被适配的类
 */
class Adaptee {
    void specificRequest() {
        // 业务代码
    }
}

/**
 * 适配器，
 */
class Adapter implements Target {
    private Adaptee adaptee;

    Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        // 相关适配的代码部分
        adaptee.specificRequest();
    }
}
```

## 5. 模式优点

* 将目标类和适配者类解耦。
* 增加了类的透明性和复用性。
* 灵活性和扩展性都非常好。

## 6. 模式缺点

* 类适配：对于不支持多重继承的语言，一次最多只能适配一个适配器类，而且目标抽象类只能为抽象类。
* 对象适配器：置换适配器者类的方法不是很容易。

## 7. 适用环境

* 系统需要使用现有的类，而这些类的接口不符合系统的需要。
* 想要建立一个可以重复使用的类。

# 8. 模式应用

Sun 公司公开的 Java 数据库连接工具 JDBC。JDBC 给出一个客户端通用的抽象接口，每一个具体数据库引擎的 JDBC 驱动软件都是一个介于 JDBC 接口和数据库引擎之间的适配器软件。

# 桥接模式

在跨平台图像浏览软件中，能够在 Windows、Linux、macOS 上显示 PNG、GIF、BMP 等格式的图像文件。系统首先会将各种格式的文件解析为像素矩阵 Matrix，然后将像素矩阵显示在屏幕上，在不同的操作系统中可以调用不同的绘制函数来绘制图像。

在图像显示软件中，分为了文件格式处理和系统环境处理，两者分离，通过不同的组合达到想要的目的。

## 1. 模式动机

对于有两个及两个以上的变化维度，应当降低维度，使各自的变化互不影响，降低类之间的耦合，减少代码编写量。

## 2. 模式定义

桥接模式，又称为「接口模式」，将抽象部分与它的实现部分分离，使它们可以独立的变化。

## 3. 模式结构

* Abstraction：抽象类
* RefinedAbstraction：扩充抽象类
* Implementor：实现类接口
* ConcreteImplementor：具体实现类

![Bridge.jpg](https://i.loli.net/2018/03/24/5ab674e989af6.jpg)

## 4. 代码实例

```java
//像素矩阵类：辅助类，各种格式的文件最终都被转化为像素矩阵，不同的操作系统提供不同的方式显示像素矩阵
class Matrix {
    //此处代码省略
}

//抽象图像类：抽象类  
abstract class Image {
    protected ImageImp imp;

    public void setImageImp(ImageImp imp) {
        this.imp = imp;
    }

    public abstract void parseFile(String fileName);
}

//抽象操作系统实现类：实现类接口  
interface ImageImp {
    public void doPaint(Matrix m);  //显示像素矩阵m  
}

//Windows操作系统实现类：具体实现类  
class WindowsImp implements ImageImp {
    public void doPaint(Matrix m) {
        //调用Windows系统的绘制函数绘制像素矩阵  
        System.out.print("在Windows操作系统中显示图像：");
    }
}

//Linux操作系统实现类：具体实现类  
class LinuxImp implements ImageImp {
    public void doPaint(Matrix m) {
        //调用Linux系统的绘制函数绘制像素矩阵  
        System.out.print("在Linux操作系统中显示图像：");
    }
}

//Unix操作系统实现类：具体实现类  
class UnixImp implements ImageImp {
    public void doPaint(Matrix m) {
        //调用Unix系统的绘制函数绘制像素矩阵  
        System.out.print("在Unix操作系统中显示图像：");
    }
}

//JPG格式图像：扩充抽象类  
class JPGImage extends Image {
    public void parseFile(String fileName) {
        //模拟解析JPG文件并获得一个像素矩阵对象m;  
        Matrix m = new Matrix();
        imp.doPaint(m);
        System.out.println(fileName + "，格式为JPG。");
    }
}

//PNG格式图像：扩充抽象类  
class PNGImage extends Image {
    public void parseFile(String fileName) {
        //模拟解析PNG文件并获得一个像素矩阵对象m;  
        Matrix m = new Matrix();
        imp.doPaint(m);
        System.out.println(fileName + "，格式为PNG。");
    }
}

//BMP格式图像：扩充抽象类  
class BMPImage extends Image {
    public void parseFile(String fileName) {
        //模拟解析BMP文件并获得一个像素矩阵对象m;  
        Matrix m = new Matrix();
        imp.doPaint(m);
        System.out.println(fileName + "，格式为BMP。");
    }
}

//GIF格式图像：扩充抽象类  
class GIFImage extends Image {
    public void parseFile(String fileName) {
        //模拟解析GIF文件并获得一个像素矩阵对象m;  
        Matrix m = new Matrix();
        imp.doPaint(m);
        System.out.println(fileName + "，格式为GIF。");
    }
}
```

## 5. 模式优点

* 分离抽象接口及其实现部分。
* 提高了系统的可扩充性。
* 实现细节对客户透明，对用户隐藏实现细节。

## 6. 模式缺点

* 桥接模式的引入会增加系统的理解与设计难度。

## 7. 适用环境

* 需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系。
* 系统需要对抽象化角色和实现化角色进行动态耦合。
* 一个类存在两个独立变化的维度，且这两个维度都需要扩展。
* 不希望使用继承或因为多层次继承导致类的个数急剧增加的系统。

## 8. 模式应用

Java 桌面软件的 GUI，在 Windows、macOS 和 Linux 上面的界面感受是不相同的。

# 装饰模式

如果大家购买了新房，并且是毛坯房，那必定会进行装修，对新房的装修并没有改变房屋是用来居住的本质，但是它可以使房子变得漂亮、更温馨、更实用、更能满足居家的需求。

## 1. 模式动机

装饰模式以对客户透明的方式动态地给一个对象附加上更多的责任，换而言之，客户端并不觉得对象在装饰前和装饰后有什么不同。装饰模式在不需要创造更多的子类的情况下，将对象的功能进行扩展。

## 2. 模式定义

装饰模式，又称为「油漆工模式」，动态地给一个对象增加一些额外的职责，就增加对象功能来说，装饰器模式比生成子类更加灵活。

## 3. 模式结构

* Component：抽象构件
* ConcreteComponent：具体构件
* Decorator：抽象装饰类
* ConcreteDecorator：具体装饰类

![Decorator.jpg](https://i.loli.net/2018/03/25/5ab7086a286b0.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 装饰器模式
 * @author: Keating Smith
 * @create: 2018-03-25 10:25
 **/
public class DeocratorPattern {
    public static void main(String[] args) {

        Component windows = new Window();
        Component blackDecorator = new BlackBorderDecorator(windows);
        blackDecorator.display();

    }
}

//抽象界面构件类：抽象构件类，为了突出与模式相关的核心代码，对原有控件代码进行了大量的简化
abstract class Component {
    public abstract void display();
}

//窗体类：具体构件类
class Window extends Component {
    public void display() {
        System.out.println("显示窗体！");
    }
}

//文本框类：具体构件类
class TextBox extends Component {
    public void display() {
        System.out.println("显示文本框！");
    }
}

//列表框类：具体构件类
class ListBox extends Component {
    public void display() {
        System.out.println("显示列表框！");
    }
}

//构件装饰类：抽象装饰类
class ComponentDecorator extends Component {
    private Component component;  //维持对抽象构件类型对象的引用

    public ComponentDecorator(Component component)  //注入抽象构件类型的对象
    {
        this.component = component;
    }

    public void display() {
        component.display();
    }
}

//滚动条装饰类：具体装饰类
class ScrollBarDecorator extends ComponentDecorator {
    public ScrollBarDecorator(Component component) {
        super(component);
    }

    public void display() {
        this.setScrollBar();
        super.display();
    }

    public void setScrollBar() {
        System.out.println("为构件增加滚动条！");
    }
}

//黑色边框装饰类：具体装饰类
class BlackBorderDecorator extends ComponentDecorator {
    public BlackBorderDecorator(Component component) {
        super(component);
    }

    public void display() {
        this.setBlackBorder();
        super.display();
    }

    public void setBlackBorder() {
        System.out.println("为构件增加黑色边框！");
    }
}
```

## 5. 模式优点

* 可以提供比继承更多的灵活性。
* 可以通过一种动态的方式来扩展一个对象的功能，通过配置文件可以在运行时选择不同的装饰器，从而实现不同的行为。
* 通过使用不同的具体装饰类以及这些装饰类的排列组合，可以创造很多不同行为的组合。
* 具体构件类与具体装饰类可以独立变化，符合「开闭原则」。

## 6. 模式缺点

* 装饰模式设计时将产生很多小对象，将增加系统的复杂度。
* 装饰模式比继承更加容易出错，排错也很困难。

## 7. 适用环境

* 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
* 需要动态地给一个对象增加功能，这些功能也能动态地被撤销。

## 8. 模式应用

在图形界面构件库的设计中，如窗体、文本框、列表框通常需要根据用户进行定制，比如带滚动条的窗体、带黑色边框的文本框等等。都采用了装饰模式。

# 外观模式

顾客去茶馆喝茶，假如自己泡茶的话，需要自行准备茶叶、茶具和开水。但是，我们也能直接跟茶馆服务人员说想要喝的茶，是铁观音、龙井？顾客不需要直接与茶叶、茶具、开水等有交互，整个泡茶的过程是由服务员来完成的，顾客只需要与服务员有交流即可。

## 1. 模式动机

在软件开发中，有时候为了完成一项比较复杂的功能，一个客户类需要和多个业务类交互，而这些需要交互的业务类经常作为一个整体出现。此时，需要一个类似服务员的角色，由它来负责好多个业务进行交互，而客户类只需要与该类交互即可。

## 2. 模式定义

外观模式，又称为「门面模式」，外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面。

## 3. 模式结构

* Facade：外观角色（类似于服务员）
* SubSystem：子系统角色（类似于茶叶、茶具、开水等过程）

![Facade.jpg](https://i.loli.net/2018/03/25/5ab70d562123e.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 外观模式
 * @author: Keating Smith
 * @create: 2018-03-25 10:46
 **/
public class FacadePattern {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.setSubSystemA(new SubSystemA());
        facade.setSubSystemB(new SubSystemB());
        facade.wrapOperation();
    }
}

/**
 * 子系统角色 A
 */
class SubSystemA {

    // 子系统角色 A 的业务方法
    public void operation() {
    }

}

/**
 * 子系统角色 B
 */
class SubSystemB {

    // 子系统 B 的业务方法
    public void operation() {
    }
}

/**
 * 外观角色，用于组合各个子系统
 */
class Facade {
    private SubSystemA subSystemA;
    private SubSystemB subSystemB;

    public void setSubSystemA(SubSystemA subSystemA) {
        this.subSystemA = subSystemA;
    }

    public void setSubSystemB(SubSystemB subSystemB) {
        this.subSystemB = subSystemB;
    }

    // 将子系统组合起来，完成一个整体的功能
    public void wrapOperation() {
        subSystemA.operation();
        subSystemB.operation();
    }
}
```

## 5. 模式优点

* 对客户屏蔽子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。
* 实现了子系统与客户之间的松耦合关系。
* 降低了大型软件系统中的编译依赖性，并简化了系统在不同平台之间的移植过程。
* 只提供了一个访问子系统的统一入口，并不影响用户直接使用子系统类。

## 6. 模式缺点

* 不能很好的限制客户使用子系统类。
* 在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或者客户端的源代码，违背了「开闭原则」。

## 7. 适用环境

* 当要为一个复杂子系统提供一个简单接口时可以使用外观模式。
* 客户程序与多个子系统之间存在很大的依赖性。
* 在层次化结构中，使用外观模式顶一个系统中每一层的入口，曾与层之间部之间产生联系，降低层之间的耦合度。

## 8. 环境应用

例如文件加密模块，具体流程包括：

1. 读取文件
2. 加密
3. 保存加密后的文件

读取文件和保存文件使用流实现，加密操作通过求模运算实现。这三个操作相互独立，可以将这三个操作的业务代码封装在三个不同的类中，然后通过统一的类来进行组合使用。

# 享元模式

假设在家中，我需要测量桌子的长度，然后我去买了一把尺子，过几天后，我又想测量沙发的长度，我不必再去购买尺子，因为家里已经有了一把尺子，直接使用就行了。

## 1. 模式动机

为了解决大量相似或相同的对象重复创建，造成了资源的浪费。

## 2. 模式定义

享元模式，又称为「轻量级模式」，运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象很相似，状态变化很小，可以实现对象的多次复用。

## 3. 模式结构

* Flyweight：抽象享元类
* ConcreteFlyweight：具体享元类
* UnsharedConcreteFlyweight：非共享 享元类
* FlyweightFactory：享元工厂类

![Flyweight.jpg](https://i.loli.net/2018/03/25/5ab71182c4503.jpg)

## 4. 代码实例

```java
import java.util.HashMap;

/**
 * @program: DesignPattern
 * @description: 享元模式
 * @author: Keating Smith
 * @create: 2018-03-25 11:03
 **/
public class FlyweightPattern {
    public static void main(String[] args) {
        FlyweightFactory factory = new FlyweightFactory();

        Flyweight flyweight = factory.getFlyweights("A");
        
        flyweight.operation();
    }
}

/**
 * 享元类
 */
class Flyweight {
    // 内部状态
    private String intrinsicState;

    public Flyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    // 业务代码
    public void operation() {
        // 具体业务代码
    }
}

/**
 * 工厂类
 */
class FlyweightFactory {
    private HashMap flyweights = new HashMap();

    // 从对象池中获取对象
    public Flyweight getFlyweights(String key) {
        // 如果对象池包含对象，直接取出来
        if (flyweights.containsKey(key)) {
            return (Flyweight) flyweights.get(key);
        } else {
            // 如果对象池没有该对象，先创建，放入对象池，再返回。
            Flyweight flyweight = new Flyweight("A");
            flyweights.put(key, flyweight);
            return flyweight;
        }
    }
}
```

## 5. 模式优点

* 极大减少了内存中对象的数量，使得相同对象或相似对象在内存中只保存一份。
* 外部状态相对独立，而且不会影响内部状态，可以在不同的环境中被共享。

## 6. 模式缺点

* 使得系统更加复杂，需要分离出内部状态和外部状态。
* 将享元对象的状态外部化，而读取外部状态使得运行时间变长。

## 7. 适用环境

* 一个系统具有大量相同或者相似的对象。
* 对象的大部分状态可以外部化。
* 需要维护一个存储享元对象的享元池，需要消耗资源，应当在多次重复使用享元对象才值得使用享元模式。

## 8. 环境应用

在文档中多次出现相同的图片，只需要创建一个图片对象。

# 代理模式

有一天，老板想要吃早餐，他不想亲自去，于是便委托助理去买早餐，助理买完早餐后，放到了老板面前，老板美滋滋的享受起了早餐。

此时，助理相当于是一个代理的作用，代替老板完成任务。

## 1. 模式动机

在某些情况下，一个客户不想或者不能直接引用一个对象，此时可以通过一个称之为「代理」的第三者来实现间接引用。代理对象可以在客户端和目标对象之间起到中介的作用。

## 2. 模式定义

代理模式，给某一个对象提供一个代理，并由代理对象控制原对象的引用。

![Proxy.jpg](https://i.loli.net/2018/03/25/5ab715ee66598.jpg)

## 3. 模式结构

* Subject：抽象主题角色
* Proxy：代理主题角色
* RealSubject：真实主题角色

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 代理模式
 * @author: Keating Smith
 * @create: 2018-03-25 11:23
 **/
public class ProxyPattern {
    public static void main(String[] args) {

        Proxy proxy = new Proxy();
        proxy.request();

    }
}

/**
 * 抽象主题
 */
abstract class Subject {
    // 业务方法
    abstract void request();
}

/**
 * 具体的主题
 */
class RealSubject extends Subject {


    @Override
    void request() {
        // 具体的业务实现方法
        System.out.printf("Real Subject: request");
    }
}

class Proxy extends Subject {
    private Subject realSubject = new RealSubject();

    /**
     * 在发出具体业务方法之前执行
     */
    private void preRequest() {
    }

    /**
     * 在具体业务方法执行之后执行
     */
    private void afterRequest() {
    }

    /**
     * 在执行具体的方法的时候，可以先执行准备工作，
     * 执行完具体的方法，执行后续的工作，
     * 客户只需要发出具体的目标，其余的都交给代理去执行。
     */
    @Override
    void request() {
        preRequest();
        realSubject.request();
        afterRequest();
    }
}
```

## 5. 模式优点

* 协调调用者和被调用者，一定程度上降低了系统的耦合度。
* 远程代理使得客户端可以访问在远程机器上的对象。
* 虚拟代理通过使用一个小对象来代表一个大对象。
* 保护代理可以控制真实对象的使用权限。

## 6. 模式缺点

* 有些类型的代理模式可能会造成请求的处理速度变慢。
* 实现代理需要额外的工作，有些代理模式实现起来非常复杂。

## 7. 适用环境

* 远程代理
* 虚拟代理
* Copy-on-Write 代理
* 保护代理
* 缓冲代理
* 防火墙代理
* 同步化代理
* 智能引用代理

## 8. 环境应用

VPN。
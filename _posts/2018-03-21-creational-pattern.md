---
title: 设计模式 - 创建型模式
date: 2018-03-21 00:08:33
tags: 
  - 敏捷开发 
---

创建型模式对类的实例化过程进行了抽象，能够软件模块中对象的创建和对象的使用分离,创建型模式隐藏了类的实例化细节，外界对这些对象只需要知道他们的共同接口，而不需要清楚具体的实现细节。

创建型模式主要分为：

* 简单工厂模式
* 工厂方法模式
* 抽象工厂模式
* 建造者模式
* 单例模式

# 简单工厂模式

假设，我们现在来到一个超市，现在遇到这种情况：

* 顾客 A 问超市里面的售货员，请问零食区在哪里？
* 售货员带领顾客 A 到了零食区；
* 然后，顾客 A 又问售货员，请问日用品区在哪儿？
* 售货员 A 又带领顾客 A 到了日用品区。

这样一个过程，就是简单工厂模式的一个运作过程。

## 1. 模式动机

向上述描述的一样，我们去超市购买东西，不需要记住具体的货物把放在哪里，只需要询问相关的售货员即可，他们会告诉我们。

简单工厂模式的理论也是如此，我们并不需要知道具体对象的名字，只需要知道表示该对象的参数，并提供一个调用方法，把参数传入该方法即可返回需要的对象。

## 2. 模式定义

简单工厂模式，又称为「静态工厂方法模式」，根据参数的不同返回不同类的实例对象。

## 3. 模式结构

* Factory：工厂角色（类似于上面的售货员）
* Product：抽象产品角色（类似于超市的货品的不同分区）
* ConcreteProduct：具体产品角色（类似于上面的零食区、日用品区等）

![20130711143612921.jpg](https://i.loli.net/2018/03/21/5ab1c14b149f6.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 简单工厂模式
 * @author: Keating Smith
 * @create: 2018-03-21 10:27
 **/
public class SimpleFactoryPattern {
    public static void main(String[] args) {

        Product product = Factory.getProduct("A");
        product.methodSame();
        product.methodDiff();

    }
}

abstract class Product {
    // 所有产品的公共业务方法
    public void methodSame() {
        // 公共方法的实现
    }

    // 声明抽象业务方法
    public abstract void methodDiff();
}

class ConcreteProductA extends Product {

    // 实现业务方法
    @Override
    public void methodDiff() {
    }
}

class ConcreteProductB extends Product {

    // 实现具体产品 B 的业务方法
    @Override
    public void methodDiff() {
    }
}

class Factory {
    public static Product getProduct(String arg) {
        Product product = null;
        if (arg.equalsIgnoreCase("A")) {
            product = new ConcreteProductA();
        } else if (arg.equalsIgnoreCase("B")) {
            product = new ConcreteProductB();
        }

        return product;
    }
}
```

## 5. 模式优点

* 工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例；简单工厂模式实现了对责任的分割，提供了专门的工厂类用于创建对象。
* 客户端无需知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可。
* 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，一定程度上提高了系统的灵活性。

## 6. 模式缺点

* 工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都会受到影响。
* 简单工厂模式会增加系统中类的个数，一定程度上增加了系统的复杂度和理解难度。
* 系统扩展难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多的时候，有可能造成工厂逻辑过于复杂，不利用系统的扩展和维护。
* 简单工厂模式使用了静态工厂方法，造成工厂角色无法形成基于继承的等级结构。

## 7. 适用环境

* 工厂类负责创建的对象少，
* 客户端只知道传入工厂类的参数，对于如何创建对象不关心。

## 8. 模式应用

### 8.1 JDK 类库

JDK 类库中广泛使用了简单工厂模式，比如工具类 `java.text.DateFormat`。

```java
public final static DateFormat getDateInstance();
public final static DateFormat getDateInstance(int style);
public final static DateFormat getDateInstance(int style,Locale
locale);
```

### 8.2 Java 加密技术

获取不同加密算法的秘钥生成器。

```java
KeyGenerator keyGen=KeyGenerator.getInstance("DESede");
```

创建密码器。

```java
Cipher cp=Cipher.getInstance("DESede");
```

# 工厂方法模式

假设我们现在仍然是去超市购买东西，遇到这种情况：

* 顾客 A 想要前台，
* 这时前台过来，询问需要什么帮助，
* 顾客 A 希望前台告诉他零食区在哪里，
* 前台领顾客 A 到了零食区；
* 顾客 A 到了零食区，希望售货员能对不同的零食进行说明，
* 这时，售货员便来。

## 1. 模式动机

就如上述的描述一样，我们去超市购买零食，但我们不知道零食区在哪里，但是没有关系，我们只需要知道前台就行了，前台的任务就是专门告诉顾客零食区的位置；到了零食区的时候，又需要售货员来对不同的零食进行说明。前台和售货员都是超市的服务人员，

在工厂方法模式中，不再提供一个统一的工厂类来创建所有的对象，而是针对不同的产品提供不同的工厂。

## 2. 模式定义

工厂方法模式，又称为「工厂模式」，工厂父类负责定义创建产品的公共接口，而工厂子类负责生成具体的产品对象。

## 3. 模式结构

* Product：抽象产品（类似于超市中不同的货物分区）
* ConcreteProduct：具体产品（类似于上面的具体的零食区等）
* Factory：抽象工厂（类似于超市里人员的统称）
* ConcreteFactory：具体工厂（类似于前台、售货员等具体的服务人员）

![20130712101002890.jpg](https://i.loli.net/2018/03/21/5ab1cba088b3e.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 工厂方法模式
 * @author: Keating Smith
 * @create: 2018-03-21 10:45
 **/
public class FactoryMethod {
    public static void main(String[] args) {
        Factory factoryA = new ConcreteFactoryA();
        Product productA = factoryA.factoryMethod();

        Factory factoryB = new ConcreteFactoryB();
        Product productB = factoryB.factoryMethod();
    }
}

abstract class Product {
    // 所有产品的公共业务方法
    public void medthodSame() {

    }

    // 声明抽象的业务方法
    public abstract void methodDiff();
}

class ConcreteProductA extends Product {

    // 实现产品 A 的业务方法
    @Override
    public void methodDiff() {

    }
}

class ConcreteProductB extends Product {

    // 实现产品 B 的业务方法
    @Override
    public void methodDiff() {

    }
}

interface Factory {
    // 声明工厂类的公共接口
    Product factoryMethod();
}

// 产品 A 的工厂类
class ConcreteFactoryA implements Factory {

    @Override
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

// 产品 B 的工厂类
class ConcreteFactoryB implements Factory {

    @Override
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}
```

## 5. 模式优点

* 工厂方法用来创建客户端需要的产品，用户只需要关心所需产品对应的工厂，无须关心创建西街，甚至无须知道具体产品类的类名。
* 能够使工厂自主确定创建何种产品对象。
* 使在系统加入新产品时，无须修改抽象工厂和抽象产品提供的接口，无须修改客户端，也无需修改其他的具体工厂和具体产品，而只需要添加一个具体工厂和具体产品就可以了。系统的扩展性提高，完全符合「开闭原则」。

## 6. 模式缺点

* 添加新产品时，需要编写具体的产品类，而且还要提供与之对应的具体工厂类，系统中类的个数成对增加，增加了系统的复杂度。
* 为了考虑系统的可扩展性，引入了抽象层，增加了系统的抽象性和理解难度。

## 7. 适用环境

* 一个类不知道它所需要的对象的类。
* 一个类通过子类来指定创建哪个对象。
* 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无须关系是哪一个工厂子类创建产品子类。

## 8. 模式应用

### 8.1 JDBC

JDBC 中的工厂方法。

```java
Connection conn=DriverManager.getConnection("jdbc:microsoft:sqlserver://loc
alhost:1433; DatabaseName=DB;user=sa;password=");
Statement statement=conn.createStatement();
ResultSet rs=statement.executeQuery("select * from UserInfo");
```

# 抽象工厂模式

让我们假设这种情况。

* 顾客需要去超市 A 购买了零食和日用品，
* 顾客觉得还可以去另外一家超市，
* 顾客又去超市 B 购买了零食和日用品。

## 1. 模式动机

就如上诉所说的一样，超市假设是工厂， 那么零食和日用品便是产品。

抽象工厂模式的具体工厂方法不再创建一种产品，而是创建一族产品。为了更好的理解抽象工厂模式，需要理解两个概念。

* 产品等级结构：产品的继承结构，如一个抽象类是货物，其子类有零食、日用品等，则抽象货物与具体的货物之间构成了一个产品等级结构。
* 产品族：产品族指由同一个工厂生产的，位于不同产品等级结构中的一组产品，如超市 B 的零食、超市 B 的日用品。

![20130713163008609.jpg](https://i.loli.net/2018/03/21/5ab27d25b2544.jpg)

抽象工厂模式可以提供多个产品对象，而不是单一的产品对象。

## 2. 模式定义

抽象工厂模式，又称为「kit 模式」，提供一个创建一系列相关或相互依赖对象的接口，而无需指定他们具体的类。

## 3. 模式结构

* AbstractFactory：抽象工厂（例如上述的超市）
* ConcreteFactory：具体工厂（例如超市 A，超市 B）
* AbstractProduct：抽象产品（例如超市中的货物）
* ConcreteProduct：具体产品（例如超市 A 中的零食，超市 B 中的零食）

![20130713163800203.jpg](https://i.loli.net/2018/03/21/5ab27e3722d47.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 抽象工厂模式
 * @author: Keating Smith
 * @create: 2018-03-21 23:47
 **/
public class AbstractFactoryPattern {
    public static void main(String[] args) {
        AbstractFactory factoryA = new ConcreteFactoryA();
        AbstractFactory factoryB = new ConcreteProductB();

        AbstractProductA productA1 = factoryA.createProductA();
        AbstractProductA productA2 = factoryB.createProductA();

        AbstractProductB productB1 = factoryA.createProductB();
        AbstractProductB productB2 = factoryB.createProductB();
    }
}

// 抽象产品 A
abstract class AbstractProductA {
}

// 抽象产品 B
abstract class AbstractProductB {
}

// 抽象工厂 A 的具体产品 A1
class ConcreteProductA1 extends AbstractProductA {
}

// 抽象工厂 B 的具体产品 A1
class ConcreteProductA2 extends AbstractProductA {
}

// 抽象工厂 A 的具体产品 B1
class ConcreteProductB1 extends AbstractProductB {
}

// 抽象工厂 B 的具体产品 B2
class ConcreteProductB2 extends AbstractProductB {
}

abstract class AbstractFactory {
    // 工厂创建产品 A 的方法
    public abstract AbstractProductA createProductA();

    // 工厂创建产品 B 的方法
    public abstract AbstractProductB createProductB();
}

class ConcreteFactoryA extends AbstractFactory {

    // 具体工厂 A 创建的产品 A
    @Override
    public AbstractProductA createProductA() {
        return new ConcreteProductA1();
    }

    // 具体工厂 B 创建的产品 B
    @Override
    public AbstractProductB createProductB() {
        return new ConcreteProductB1();
    }
}

class ConcreteProductB extends AbstractFactory {

    // 具体工厂 B 创建的产品 A
    @Override
    public AbstractProductA createProductA() {
        return null;
    }

    // 具体工厂 B 创建的产品 B
    @Override
    public AbstractProductB createProductB() {
        return null;
    }
}
```

## 5. 模式优点

* 抽象工厂隔离了具体类的生成，使得客户端并不需要知道什么被创建，实现了「高内聚，低耦合」的设计目的。
* 当一个产品族中的多个对象被设计成一起工作的时候，能够保证客户端始终只使用同一产品族中的对象。
* 添加新的具体工厂和产品族很方便，无需修改已有系统，符合「开闭原则」。

## 6. 模式缺点

* 在添加新的产品对象时，难以扩展抽象工厂来生产新种类的产品，因为在抽象工厂规定了所有可能被创建的产品集合。
* 开闭原则的倾斜性（添加新的工厂和产品族容易，添加新的产品等级结构麻烦。）

## 7. 适用环境

* 一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节。
* 系统中有多余一个的产品族，而每次只使用其中某一个产品族。
* 属于同一产品族的产品将在一起使用，这一约束必须在系统的设计中体现出来。
* 系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

## 8. 模式应用

在很多软件系统中需要更换主题界面，要求界面中的按钮、文本框、背景色等一起发生改变时，可以使用抽象工厂模式进行设计。

# 建造者模式

在游戏的一些场景中，比如游戏角色的设计，游戏角色拥有性别、脸型、服装、发型等因素，它们共同组成游戏角色。例如这种方式的，便是建造者模式。

## 1. 模式动机

在有些时候，我们需要创建一个复杂的对象，它是由多个组件共同构建而来的。但是我们并不需要知道复杂对象的内部组成方式，只需要知道所需要的建造者的类型即可。

## 2. 模式定义

建造者模式，又称为「生成器模式」，将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以有不同的表示方式。

## 3. 模式结构

* Builder：抽象建造者（表示创建游戏角色的过程）
* ConcreteBuilder：具体建造者
* Director：指挥者（指挥建造者）
* Product：产品角色（例如具体的游戏角色）

![1333532291_9501.gif](https://i.loli.net/2018/03/22/5ab3337a98add.gif)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 建造者模式
 * @author: Keating Smith
 * @create: 2018-03-22 12:40
 **/
public class BuilderPattern {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        Product product = director.construct();
    }
}

/**
 * 产品类
 */
class Product {
    private String partA; // 定义部件 A
    private String partB; // 定义部件 B
    private String partC; // 定义部件 C

    // 省略 Getter 和 Setter 方法
}


/**
 * 抽象建造者，定义公共的建造者方法
 */
abstract class Builder {
    protected Product product = new Product();

    public abstract void buildPartA();

    public abstract void buildPartB();

    public abstract void buildPartC();

    public Product getProduct() {
        return product;
    }
}

class ConcreteBuilder extends Builder {
    @Override
    public void buildPartA() {
        // 构建 part A 的具体过程
    }

    @Override
    public void buildPartB() {
        // 构建 part B 的具体过程
    }

    @Override
    public void buildPartC() {
        // 构建 part C 的具体过程
    }
}

/**
 * 指挥者
 */
class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    public void setBuilder(Builder builder) {
        this.builder = builder;
    }

    // 产品构建与组装方法
    public Product construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();

        return builder.getProduct();
    }
}
```

## 5. 模式优点

* 客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象。
* 用户使用不同的具体建造者即可得到不同的产品对象。
* 可以更加精细地控制产品的创建过程。
* 增加新的具体建造者无需修改原有类库的代码，指挥者类针对抽象编建造者类编程，系统扩展方便，符合「开闭原则」。

## 6. 模式缺点

* 建造者模式所创建的产品一般具有较多的共同点，如果产品之间差异性很大，则不适合使用建造者模式。
* 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现变化，导致系统变得庞大。

## 7. 适用环境

* 需要生成的产品对象有复杂的内部结构，这些产品对象通常包含多个成员属性。
* 需要生成的产品对象的属性相互依赖，需要指定生成顺序。
* 对象的创建过程独立于创建该对象的类。
* 隔离复杂对象的创建和使用。

## 8. 模式应用

很多游戏中，游戏角色包括人体、服装、装备等组成部分；地图包含天空、地面、背景等组成部分。

# 单例模式

假如在超市的结算口，一条出口应当只需要一位收银员，如果每一位顾客配备一位收银员，必定会造成人员的浪费。

## 1. 模式动机

单例模式，为了节约系统的资源，确保系统中某些类只有唯一实例。

## 2. 模式定义

单例模式，确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

## 3. 模式结构

* Singleon：单例

![1333305124_9327.gif](https://i.loli.net/2018/03/22/5ab338090daf0.gif)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 单例模式
 * @author: Keating Smith
 * @create: 2018-03-22 12:59
 **/
public class SingletonPattern {
    public static void main(String[] args) {
        SingleTon singleTon = SingleTon.getInstance();
    }
}

/**
 * 单例
 */
class SingleTon {
    // 唯一的实例对象
    private static SingleTon instance;

    public static SingleTon getInstance() {
        // 判断是否已经实例化
        if (instance == null) {
            return new SingleTon();
        } else {
            return instance;
        }
    }
}
```
## 5. 模式优点

* 提供了队唯一实例的受控访问。
* 系统内存中只存在一个对象，可以节约系统资源。
* 允许可变数目的实例。

## 6. 模式缺点

* 没有抽象层，因此单例类的扩展很难。
* 单例类的职责过重，一定程度上违背了「单一职责原则」。
* 滥用单例将带来一些负面问题，如导致共享连接池对象的程序过多而导致连接池溢出；由于面向对象的垃圾回收，可能导致实例化对象的长时间不被利用，会被系统销毁回收。

## 7. 适用环境

* 系统只需要一个实例对象。
* 客户调用的单个实例只允许使用一个公共访问点。
* 一个系统中要求类只有一个实例时。

## 8. 模式应用

数据库中具有自主编号主键的表可以由多个用户使用，但数据库只能有一个地方来分配下一个主键编号，否则会导致主键的重复，所以主键编号的生成器必须具有唯一性。
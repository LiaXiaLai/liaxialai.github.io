---
title: 设计模式 - 行为型模式
date: 2018-03-23 16:25:27
tags: 
  - 敏捷开发
---

行为型模式是对在不同的对象之间划分责任和算法的抽象化。

行为型模式不仅关心类和对象的结构，而且重点关注他们之间的相互作用。通过行为型模式，更加清晰地划分类与对象的职责。

行为型模式分为类行为型模式和对象行为型模式。

* 类行为型模式：动态地方式分配父类和子类的职责。
* 对象行为型模式：使用对象的聚合关联关系来分配行为，根据「合成复用原则」，系统中尽量使用关联关系来取代继承关系，因此对象行为型模式被广泛使用。

行为型模式分为：

* 命令模式
* 中介者模式
* 观察者模式
* 状态模式
* 策略模式

# 命令模式

![1366033382_9782.jpg](https://i.loli.net/2018/03/25/5ab78a9228546.jpg)

在装修完新房子的时候，一般会安装控制电器的开关。在购买开关的时候，我们并不知道它是用来控制什么电器的，也就是说开关和电器之间没有直接的关系，一个开关安装之后，可以用来控制电灯，也能用来控制风扇。开关与电器之间通过电线建立连接，系统的开关可以通过不同的电线来控制不同的电器。

## 1. 模式动机

在软件设计中，经常需要向某些对象发送请求，但是并不知道请求的接受者是谁，也不知道被请求的操作是哪个，只需在程序运行时指定具体的请求接收者即可。

## 2. 模式定义

命令模式，又称为「事务模式」，将一个请求封装成为一个对象，从而使我们可用不同的请求对客户进行参数化。

## 3. 模式结构

* Command：抽象命令类（类似于上述的开关电器这个动作）
* ConcreteCommand：具体命令类
* Invoker：调用者（类似于上述的开关）
* Receiver：接收者（类似于上述的电器）
* Client：客户类（执行操作的人）

![Command.jpg](https://i.loli.net/2018/03/25/5ab78c1fdae04.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 命令模式
 * @author: Keating Smith
 * @create: 2018-03-25 19:47
 **/
public class CommandPattern {
    public static void main(String[] args) {
        Command command = new ConcreCommand(new Receiver());
        Invoker invoker = new Invoker(command);

        invoker.call();
    }
}

/**
 * 命令角色
 */
interface Command {
    // 公共的业务方法
    public void execute();
}

class Invoker {
    private Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    // 调用命令
    public void call() {
        command.execute();
    }
}

/**
 * 接收者
 */
class Receiver {
    // 具体的命令
    public void action() {
        System.out.printf("action");
    }
}

/**
 * 具体的命令角色
 */
class ConcreCommand implements Command {
    // 接收者对象
    private Receiver receiver;

    public ConcreCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    // 对接收者对象方法的调用
    @Override
    public void execute() {
        System.out.printf("ConcreteCommand");
        receiver.action();
    }
}
```

## 5. 模式优点

* 降低系统的耦合度。
* 新的命令可以很容易地加入到系统中。
* 可以比较容易地设计一个命令队列和宏命令。
* 可以方便地实现对请求的 Undo 和 Redo。

## 6. 模式缺点

* 可能会导致某些系统有过多的具体命令类。

## 7. 适用环境

* 系统需要将请求调用者和请求接收者解耦。
* 系统需要在不同的时间指定请求、将请求排队和执行请求。
* 系统需要支持命令的撤销操作和恢复操作。
* 系统需要将一组操作组合在一起，即支持宏命令。

## 8. 模式应用

很多系统都提供了宏命令功能，例如 Unix 下的 shell 编程，可以将很多命令封装在一个命令对象中，只需要一条简单的命令即可执行一个命令序列。 

# 中介者模式

![1357651353_4861-2.jpg](https://i.loli.net/2018/03/25/5ab7932114caa.jpg)

界面组件之间存在比较敷在的关系，如果删除一个客户，需要在列表中删除对应的项，选择组合框中也要删除；如果增加一个客户，客户列表要增加一个客户，组合框也要增加一项。

## 1. 模式动机

![1357652393_2992-2.jpg](https://i.loli.net/2018/03/25/5ab7940879083.jpg)

对于一个模块而言，可能会由多个对象构成，而这些对象存在相互的引用，为了减少对象之间复杂的引用关系，使之成为一个松耦合的系统，需要一个中间类来进行相同级别对象的管理与引用。  

## 2. 模式定义

中介者模式，又称为「调停者模式」，用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显示地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

## 3. 模式结构

* Mediator：抽象中介者
* ConcreteMediator：具体中介者
* Colleague：抽象同事类
* ConcreteColleague：具体同事类

![Mediator.jpg](https://i.loli.net/2018/03/25/5ab794bc2629a.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 中介者模式
 * @author: Keating Smith
 * @create: 2018-03-25 20:23
 **/
public class MediatorPattern {
    public static void main(String[] args) {

        ConcreteMediator mediator = new ConcreteMediator();


        Button button = new Button();
        List list = new List();
        ComboBox comboBox = new ComboBox();
        TextBox textBox = new TextBox();

        button.setMediator(mediator);
        list.setMediator(mediator);
        comboBox.setMediator(mediator);
        textBox.setMediator(mediator);

        mediator.setButton(button);
        mediator.setList(list);
        mediator.setComboBox(comboBox);
        mediator.setTextBox(textBox);

        button.changed();
        list.changed();

    }
}

/**
 * 抽象中介者
 */
abstract class Mediator {
    public abstract void componentChanged(Component c);
}

class ConcreteMediator extends Mediator {
    // 定义各个同事对象
    private Button button;
    private List list;
    private ComboBox comboBox;
    private TextBox textBox;

    public void setButton(Button button) {
        this.button = button;
    }

    public void setList(List list) {
        this.list = list;
    }

    public void setComboBox(ComboBox comboBox) {
        this.comboBox = comboBox;
    }

    public void setTextBox(TextBox textBox) {
        this.textBox = textBox;
    }

    @Override
    public void componentChanged(Component c) {
        if (c == button) {
            System.out.printf("Button");
            list.update();
            comboBox.update();
            textBox.update();
        } else if (c == list) {
            System.out.printf("List");
            comboBox.select();
            textBox.setText();
        } else if (c == comboBox) {
            System.out.printf("ComboBox");
            comboBox.select();
            textBox.setText();
        }
    }
}

/**
 * 抽象同事
 */
abstract class Component {
    protected Mediator mediator;

    public void setMediator(Mediator mediator) {
        this.mediator = mediator;
    }

    public void changed() {
        mediator.componentChanged(this);
    }

    public abstract void update();
}

/**
 * 抽象同事：按钮
 */
class Button extends Component {
    @Override
    public void update() {
        // 按钮不产生交互
    }
}

/**
 * 抽象同事：列表
 */
class List extends Component {

    // 列表更新方法
    @Override
    public void update() {
        System.out.printf("List add");
    }

    public void select() {
        System.out.printf("Select List");
    }
}

/**
 * 抽象同事：组合框
 */
class ComboBox extends Component {
    @Override
    public void update() {
        System.out.printf("ComboBox add");
    }

    public void select() {
        System.out.printf("Select Combo");
    }
}

/**
 * 抽象同事：文本框
 */
class TextBox extends Component {
    @Override
    public void update() {
        System.out.printf("TextBox ");
    }

    public void setText() {
        System.out.printf("TextBox set");
    }
}
```

## 5. 模式优点

* 简化了对象之间的交互。
* 将各同事解耦。
* 减少子类生成。
* 可以简化各同事类的设计和实现。

## 6. 模式缺点

* 具体中介者类中包含了同事之间的交互细节，可能会导致具体中介者类非常复杂，使得系统难以维护。

## 7. 适用环境

* 系统中对象之间存在复杂的引用关系，产生的相互依赖关系结构混乱且难以理解。
* 一个对象由于引用了其他很多对象并直接和这些对象通信，导致难以复用该对象。
* 想要通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。
* 交互的公共行为，如果需要改变行为则可以增加新的中介者类。

## 8. 模式应用

MVC 架构中的控制器，Controller 作为一种中介者，它负责控制视图对象 View 和模型 Model 之间的交互。

# 观察者模式

![1341499237_8810.jpg](https://i.loli.net/2018/03/25/5ab79979a3979.jpg)

在十字路口，司机一般都会先观察红绿灯的情况，如果是绿灯，则继续前行；如果是红灯，则等待至绿灯之后，再继续执行。

这种情况下，司机则为观察者，红绿灯则为被观察者。

## 1. 模式动机

建立一种对象与对象之间的依赖关系，一个对象发生改变将自动通知其他对象，其他对象将想做出反应。在此，发生改变的对象称之为观察目标，而被通知的对象称为观察者，一个观察目标可以对应多个观察者，而且这些观察者之间没有相互的联系。

## 2. 模式定义

观察者模式，又称为「从属者模式」，定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并自动更新。

## 3. 模式结构

* Subject：目标
* ConcreteSubject：具体目标
* Oberser：观察者
* ConcreteOberser：具体观察者

![Obeserver.jpg](https://i.loli.net/2018/03/25/5ab79a7a1a15c.jpg)

## 4. 代码实例

```java
import java.util.ArrayList;

/**
 * @program: DesignPattern
 * @description: 观察者模式
 * @author: Keating Smith
 * @create: 2018-03-25 20:48
 **/
public class ObserverPattern {
    public static void main(String[] args) {
        TrafficLight trafficLight = new TrafficLight();
        Observer driverA = new DriverA();
        Observer driverB = new DriverB();

        trafficLight.register(driverA);
        trafficLight.register(driverB);

        trafficLight.notifyObservers();
    }
}

// 抽象观察类
interface Observer {
    public void drive();
}

/**
 * 具体观察者 司机 A
 */
class DriverA implements Observer {

    @Override
    public void drive() {
        // 编写具体的业务方法
    }
}

/**
 * 具体观察者 司机 B
 */
class DriverB implements Observer {
    @Override
    public void drive() {
        // 编写具体的业务方法
    }
}

abstract class Subject {
    // 定义一个集合，用来保存所有的观察者
    protected ArrayList<Observer> observers = new ArrayList<>();

    public void register(Observer observer) {
        observers.add(observer);
    }

    public void quit(Observer observer) {
        observers.remove(observer);
    }

    public abstract void notifyObservers();
}

class TrafficLight extends Subject {

    @Override
    public void notifyObservers() {
        for (Object driver : observers) {
            ((Observer) driver).drive();
        }
    }
}
```

## 5. 模式优点

* 可以实现表示层和数据逻辑层的分离，并定义了稳定的消息更新重传机制。
* 在观察目标和观察者之间建立一个抽象的耦合。
* 支持广播通信。
* 符合「开闭原则」。

## 6. 模式缺点

* 如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
* 如果观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
* 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了改变。

## 7. 适用环境

* 一个抽象模型有两个方面，其中一个方面依赖另一个方面。
* 一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。
* 一个对象必须通知其他对象，而并不知道这些对象是谁。
* 需要在系统中创建一个触发链，A 对象的行为将影响到 B 对象，B 对象的行为将影响 C 对象。

## 8. 模式应用

观察者模式在软件开发中应用非常广泛，如果电子商务网站可以在执行发送操作后给多个用户发送商品打折信息，团队战斗游戏中某队友牺牲将给所有成员提示等等。

# 状态模式

我们去银行办理业务的时候，如果我们需要取 200 块，柜台人员首先会查询余额，如果余额大于 200，则正常办理业务，如果小于 200 块的话，则不予以办理。

这便依赖于银行账户的状态，根据状态的不同，而产生不同的结果。

## 1. 模式动机

很多情况下，一个对象的行为取决于一个或多个动态变化的属性，这样的属性也叫做状态。当一个对象与外部事件产生互动时，其内部状态就会改变，从而使得系统的行为也随之发生改变。

## 2. 模式定义

状态模式，又称之为「状态对象」，允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。

## 3. 模式结构

* Context：环境类
* State：抽象状态类
* ConcreteState：具体状态类

![State.jpg](https://i.loli.net/2018/03/25/5ab7a161def68.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 状态模式
 * @author: Keating Smith
 * @create: 2018-03-25 21:18
 **/
public class StatePattern {
    public static void main(String[] args) {
        Account account = new Account("keating", 2000);

        account.deposit(1000);
        account.withdraw(2000);
    }
}

/**
 * 环境类：银行账户
 */
class Account {
    // 状态
    private AccountState state;
    // 开户名
    private String owner;
    // 账户余额
    private double balance = 0;

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public double getBalance() {
        return balance;
    }

    public void setState(AccountState state) {
        this.state = state;
    }

    public Account(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
        this.state = new NormalState(this);
    }

    // 存款
    public void deposit(double amount) {
        System.out.printf(this.owner + " deposit " + amount);
        // 调用状态对象的 deposit 方法
        state.deposit(amount);
        System.out.printf("Now balance is: " + this.balance);
        System.out.printf("Now state is: " + this.state.getClass().getName());
    }

    // 取款
    public void withdraw(double amount) {
        System.out.printf(this.owner + " withdraw " + amount);
        System.out.printf("Now balance is: " + this.balance);
        System.out.printf("Now state is: " + this.state.getClass().getName());
    }

    public void computeInterest() {
        // 计算利息
        state.computeInterest();
    }
}

/**
 * 抽象状态
 */
abstract class AccountState {
    protected Account account;

    public abstract void deposit(double amount);

    public abstract void withdraw(double amount);

    public abstract void computeInterest();

    public abstract void stateCheck();
}

/**
 * 具体状态：正常状态
 */
class NormalState extends AccountState {

    public NormalState(Account account) {
        this.account = account;
    }

    public NormalState(AccountState accountState) {
        this.account = accountState.account;
    }

    @Override
    public void deposit(double amount) {
        account.setBalance(account.getBalance() + amount);
        stateCheck();
    }

    @Override
    public void withdraw(double amount) {
        account.setBalance(account.getBalance() - amount);
        stateCheck();
    }

    @Override
    public void computeInterest() {
        System.out.printf("Normal");
    }

    @Override
    public void stateCheck() {
        if (account.getBalance() > -2000 && account.getBalance() <= 0) {
            account.setState(new OverdraftState(this));
        } else if (account.getBalance() == -2000) {
            account.setState(new RestrictState(this));
        } else if (account.getBalance() < -2000) {
            System.out.printf("Operation invalid");
        }
    }
}

/**
 * 透支状态
 */
class OverdraftState extends AccountState {
    public OverdraftState(AccountState accountState) {
        this.account = accountState.account;
    }

    @Override
    public void deposit(double amount) {
        account.setBalance(account.getBalance() + amount);
        stateCheck();
    }

    @Override
    public void withdraw(double amount) {
        account.setBalance(account.getBalance() - amount);
        stateCheck();
    }

    @Override
    public void computeInterest() {
        System.out.printf("compute Interest");
    }

    @Override
    public void stateCheck() {
        if (account.getBalance() > 0) {
            account.setState(new NormalState(this));
        } else if (account.getBalance() == -2000) {
            account.setState(new RestrictState(this));
        } else if (account.getBalance() < -2000) {
            System.out.printf("Operation invalid");
        }
    }
}

/**
 * 受限状态
 */
class RestrictState extends AccountState {
    public RestrictState(AccountState accountState) {
        this.account = accountState.account;
    }

    @Override
    public void deposit(double amount) {
        account.setBalance(account.getBalance() + amount);
        stateCheck();
    }

    @Override
    public void withdraw(double amount) {
        System.out.printf("Account invalid");
    }

    @Override
    public void computeInterest() {
        System.out.printf("Compute interest");
    }

    @Override
    public void stateCheck() {
        if (account.getBalance() > 0) {
            account.setState(new NormalState(this));
        } else if (account.getBalance() > -2000) {
            account.setState(new OverdraftState(this));
        }
    }
}
```

## 5. 模式优点

* 封装转换了规则。
* 枚举可能的状态，在枚举状态之前需要先确定状态种类。
* 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。
* 允许状态转换逻辑与状态对象合成一体，而不是具体某一个巨大的条件语句块。
* 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。

## 6. 模式缺点

* 增加系统类和对象的个数。
* 结构和实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。
* 对「开闭原则」的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源码，否则无法切换到新增状态；而且修改某个状态类的行为也需要修改对应类的源代码。

## 7. 适用环境

* 对象的行为依赖于它的状态并且可以根据它的状态改变而改变它的相关行为。
* 代码中包含大量与对象状态有关的条件语句。

## 8. 环境应用

状态模式在游戏中得以广泛使用，比如游戏角色的死亡，存活、任务完成状态等。

# 策略模式

在电影院购票的时候，通常会有三种购票方案：

* 学生票凭学生证享受 8 折优惠
* 年龄在 10 周岁以下的儿童享受一定减免
* VIP 用户除打着外，还能积分。

针对不同的用户，采取不同的定价策略。

## 1. 模式动机

完成一项任务，往往有很多种不同的方式，每一种方式称为一个策略，可以根据环境或者条件的不同选择不同的策略来完成该任务。

## 2. 模式定义

策略模式，又称为「政策模式」，定义一系列算法， 将每一个算法封装起来，并让它们可以互相替换。

## 3. 模式结构

* Context：环境类
* Strategy：抽象策略类
* ConcreteStrategy：具体策略类

![Strategy.jpg](https://i.loli.net/2018/03/25/5ab7ad980f7af.jpg)

## 4. 代码实例

```java
/**
 * @program: DesignPattern
 * @description: 策略模式
 * @author: Keating Smith
 * @create: 2018-03-25 21:59
 **/
public class StrategyPattern {
    public static void main(String[] args) {
        MovieTicket movieTicket = new MovieTicket(new StudentDiscount());

        movieTicket.setPrice(100);
        movieTicket.getPrice();
    }
}

/**
 * 环境类：电影票类
 */
class MovieTicket {
    private double price;
    // 抽象折扣类的引用
    private Discount discount;

    public MovieTicket(Discount discount) {
        this.discount = discount;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public void setDiscount(Discount discount) {
        this.discount = discount;
    }

    public double getPrice() {
        return discount.calculate(this.price);
    }
}

/**
 * 抽象策略类：折扣
 */
interface Discount {
    public double calculate(double price);
}

/**
 * 具体策略类：学生折扣
 */
class StudentDiscount implements Discount {

    @Override
    public double calculate(double price) {
        return price * 0.8;
    }
}

/**
 * 具体策略类：儿童折扣
 */
class ChildrenDiscount implements Discount {
    @Override
    public double calculate(double price) {
        return price - 10;
    }
}

/**
 * 具体策略类：VIP 折扣
 */
class VIPDiscount implements Discount {

    @Override
    public double calculate(double price) {
        return price * 0.5;
    }
}
```

## 5. 模式优点

* 提供了「开闭原则」的完美支持，用户可以在不修改原有系统的基础上选择，也可以灵活地增加。
* 提供了管理相关的方法。
* 提供了可以替换继承关系的办法。
* 可以避免使用多重条件转移语句。

## 6. 模式缺点

* 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。
* 将造成产生很多策略类，可以通过使用享元模式在一定程度上减少对象的数量。

## 7. 适用环境

* 在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。
* 一个系统需要动态地在几种策略中选择一种。
* 如果一个对象有很多行为，如果使用不恰当的模式，这些行为就只好使用多重的条件选择语句来实现。
* 不希望客户端知道复杂的、与策略相关的数据结构。

## 8. 环境应用

电影票打折方案。
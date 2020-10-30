[TOC]

# 介绍
设计模式（Design pattern）代表了软件开发的最佳实践，是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是通过试验和错误总结出来的。

设计模式分为三大类：
1. 创建型模式（Creational Patterns）：提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 关键字直接实例化对象。
    - 工厂模式（Factory Pattern）
    - 抽象工厂模式（Abstract Factory Pattern）
    - 单例模式（Singleton Pattern）
    - 建造者模式（Builder Pattern）
    - 原型模式（Prototype Pattern）
2. 结构型模式（Structural Patterns）：关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。
    - 适配器模式（Adapter Pattern）
    - 桥接模式（Bridge Pattern）
    - 过滤器模式（Filter、Criteria Pattern）
    - 组合模式（Composite Pattern）
    - 装饰器模式（Decorator Pattern）
    - 外观模式（Facade Pattern）
    - 享元模式（Flyweight Pattern）
    - 代理模式（Proxy Pattern）
3. 行为型模式（Behavioral Patterns）：关注对象之间的通信
    - 责任链模式（Chain of Responsibility Pattern）
    - 命令模式（Command Pattern）
    - 解释器模式（Interpreter Pattern）
    - 迭代器模式（Iterator Pattern）
    - 中介者模式（Mediator Pattern）
    - 备忘录模式（Memento Pattern）
    - 观察者模式（Observer Pattern）
    - 状态模式（State Pattern）
    - 空对象模式（Null Object Pattern）
    - 策略模式（Strategy Pattern）
    - 模板模式（Template Pattern）
    - 访问者模式（Visitor Pattern）


# 工厂模式
> [设计模式之工厂模式（factory pattern）](https://www.cnblogs.com/yssjun/p/11102162.html)

> [工厂模式](https://www.runoob.com/design-pattern/factory-pattern.html)

工厂模式是一种创建型模式，提供了一种创建对象的方式，但是不会暴露创建对象的逻辑。工厂模式可以分为简单工厂模式，工厂方法模式和抽象工厂模式。


## 简单工厂模式
该模式管理创建对象的方式最为简单，它只是在工厂中进行了一层简单的封装：根据接收的名字创建指定类型的对象。

UML类图如下：

![设计模式 简单工厂模式](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030101938.png?token=AE4F4YPMGOFXXC5LALFDEDS7TN4NG)

优点：1、要想创建一个对象，只要知道其名称就可以了。2、有一定扩展性，如果想增加一个产品，需要增加一个具体的实现类并在工厂中添加即可。

缺点：1、要想增加一个产品，需要增加一个具体的实现类，但是还需要修改工厂，违反了开闭原则。

代码示例：
```java
// 类型接口
public interface Phone {
    void call();
}
// 实现类
public class XiaomiPhone implements Phone {
    @Override
    public void call() {
        System.out.println("Xiaomi phone is calling ...");
    }
}
// 实现类
public class HuaweiPhone implements Phone {
    @Override
    public void call() {
        System.out.println("Huawei phone is calling ...");
    }
}
// 简单工厂
public class SimplePhoneFactory {
    public Phone makePhone(String phoneType) {
        if(phoneType.equalsIgnoreCase("Xiaomi")){
            return new XiaomiPhone();
        }
        else if(phoneType.equalsIgnoreCase("Huawei")) {
            return new HuaweiPhone();
        }
        return null;
    }
}
// 测试
public class Demo {
    public static void main(String[] arg) {
        SimplePhoneFactory factory = new SimplePhoneFactory();
        Phone xiaomiPhone = factory.makePhone("Xiaomi");
        xiaomiPhone.call()
        Phone huaweiPhone = factory.makePhone("Huawei");
        huaweiPhone.call();
    }
}
```


## 工厂方法模式
和简单工厂模式中工厂负责生产所有产品相比，工厂方法模式将生产具体产品的任务分发给具体产品的工厂，具体做法是创建一个抽象工厂接口，然后给每一个产品都创建一个具体工厂，由各自的工厂负责生产产品。

UML类图如下：

![设计模式 工厂方法模式](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030102212.png?token=AE4F4YN5PJGCM3UVWI7G5WK7TN4WY)

优点：1、工厂的任务更加具体。2、增加一种产品，只需要增加一个具体的实现类和对应的工厂，该工厂需要实现抽象工厂接口，符合开闭原则。

缺点：1、增加一个产品，都需要增加一个具体的实现类和对应的工厂，类的个数增加过快。

代码示例：
```
// 抽象工厂
public interface AbstractFactory {
    Phone makePhone();
}
// 实现类的工厂
public class XiaomiFactory implements AbstractFactory {
    @Override
    public Phone makePhone() {
        return new XiaomiPhone();
    }
}
// 实现类的工厂
public class HuaweiFactory implements AbstractFactory {
    @Override
    public Phone makePhone() {
        return new HuaweiPhone();
    }
}
// 测试
public class Demo {
    public static void main(String[] arg) {
        AbstractFactory xiaomiFactory = new XiaomiFactory();
        AbstractFactory huaweiFactory = new HuaweiFactory();
        Phone xiaomiPhone = xiaomiFactory.makePhone();
        xiaomiPhone.call();
        Phone huaweiPhone = huaweiFactory.makePhone();
        huaweiPhone.call();
    }
}
```

## 抽象工厂模式
上面两种模式不管工厂怎么拆分抽象，都只创建一种产品。

抽象工厂模式通过在`AbstarctFactory`接口中增加创建产品的接口，并在具体的子工厂中实现新加产品的创建，前提是子工厂支持生产该产品。如果不支持，子工厂可以什么也不做。

代码示例：
```java
// PC接口
public interface PC {
    void code();
}
// 实现类
public class XiaomiPC implements PC {
    @Override
    public void code() {
        System.out.println("Xiaomi PC is coding ...");
    }
}
// 实现类
public class HuaweiPC implements PC {
    @Override
    public void code() {
        System.out.println("Huawei PC is coding ...");
    }
}
// 抽象工厂接口
public interface AbstractFactory {
    Phone makePhone();
    PC makePC();
}
// 实现类的工厂
public class XiaomiFactory implements AbstractFactory{
    @Override
    public Phone makePhone() {
        return new XiaomiPhone();
    }
    @Override
    public PC makePC() {
        return new XiaomiPC();
    }
}
// 实现类的工厂
public class HuaweiFactory implements AbstractFactory{
    @Override
    public Phone makePhone() {
        return new HuaweiPhone();
    }
    @Override
    public PC makePC() {
        return new HuaweiPC();
    }
}
// 测试
public class Demo {
    public static void main(String[] arg) {
        AbstractFactory xiaomiFactory = new XiaomiFactory();
        AbstractFactory huaweiFactory = new HuaweiFactory();
        Phone xiaomiPhone = xiaomiFactory.makePhone();
        PC xiaomiPC = xiaomiFactory.makePC();
        Phone huaweiPhone = huaweiFactory.makePhone();
        PC huaweiPC = huaweiFactory.makePC();
    }
}
```


# 单例模式
> [序列化和反序列化的对单例破坏的防止及其原理](https://www.jianshu.com/p/02fb2a967d30)

> [单例模式详解--通过源码分析：反射及反序列化破坏单例原理及枚举式单例如果防止其破坏、readResolve()如何防止反序列化破坏单例以及spring容器式单例思想](https://blog.csdn.net/weixin_49176063/article/details/107299195)

保证一个类仅有一个实例，并提供一个访问它的全局访问点。创建单例对象有两种策略：一是立即创建，类加载时就创建单例对象，保证单例对象唯一；二是延迟创建，在需要使用时才创建，可以避免资源浪费。

## 饿汉式
饿汉式单例是指在类加载时就创建单例对象。类加载即初始化，由虚拟机保证线程安全。

代码如下所示：
```java
public class HungrySingleton {
    private HungrySingleton() {}
    private static final HungrySingleton instance = new HungrySingleton();

    public static HungrySingleton getInstance() {
        return instance;
    }
}
```

也可以使用静态代码块：
```java
public class HungrySingleton {
    private HungrySingleton() {}
    private static final HungrySingleton instance;
    
    static {
        instance = new HungrySingleton();
    }
    
    public static HungrySingleton getInstance() {
        return instance;
    }
}
```


### 序列化破坏单例
如果`HungrySingleton`实现了序列化接口`Serializable`，序列化和反序列化机制会破坏单例模式的规则。换句话说，序列化一个单例对象，然后对序列化得到的文件进行反序列化，得到的单例对象与原来的单例对象不同。

测试代码如下所示：
```java
// 实现序列化接口Serializable
public class HungrySingleton implements Serializable {
    // 代码不变
}
// 测试
public class TestSerializable {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("testFile"));
        oos.writeObject(HungrySingleton.getInstance());
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("testFile"));
        HungrySingleton newInstance = (HungrySingleton) ois.readObject();
        // 判断是否是同一个对象
        System.out.println(newInstance == HungrySingleton.getInstance()); // 输出false
    }
}
```

解决办法是添加一个`readResolve`方法，而且返回类型必须是`Object`，代码如下所示：
```java
public class HungrySingleton implements Serializable {
    // 其他代码相同
    private Object readResolve() {  // 返回类型必须是Object
        return instance;
    }
}
```

虽然`readResolve`方法解决了序列化破坏单例模式的问题，但是实际上，在反序列化调用`readObject`方法时，会先实例化出一个新的单例对象（`Object`类型），然后判断`HungrySingleton`是否实现了`readResolve`方法。如果实现了，那么这个新的单例对象不会被返回，而是被`readResolve`方法中返回的`Object`对象覆盖。


### 反射破坏单例
通过反射可以创建出多个单例对象，破坏了单例模式的规则。

测试代码如下所示：
```java
public class TestReflect {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException,
            InvocationTargetException, InstantiationException {
        Constructor cons = HungrySingleton.class.getDeclaredConstructor();
        cons.setAccessible(true); // 设置强制访问
        HungrySingleton newInstance = (HungrySingleton) cons.newInstance();
        // 判断是否是同一个对象
        System.out.println(newInstance == HungrySingleton.getInstance()); // 输出false
    }
}
```

解决办法是创建一个变量，用来统计单例的个数（初始为0），如果个数大于`0`之后再创建单例对象就抛出一个运行时异常，代码如下所示：
```java
public class HungrySingleton implements Serializable {
    private static int count = 0;
    private HungrySingleton() {
        synchronized (HungrySingleton.class) {
            if (count > 0) {
                throw new RuntimeException("创建了两个实例");
            }
            count++;
        }
    }
    // 其他代码相同
}
```

统计创建单例的个数并不会影响序列化和反序列化，不会因为反序列化得到了一个单例对象而抛出异常，因为反序列化不会执行构造函数。


## 懒汉式
懒汉式单例是指在第一次获取单例时才创建单例对象。

代码如下所示：
```java
public class LazySingleton {
    private LazySingleton() {}
    private static LazySingleton instance = null;

    public static LazySingleton getInstance() {
        if (null == instance) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

线程不安全，解决办法是用`synchronized`关键字修饰`getInstance`方法，虽然能够保证线程安全，但是会降低并发度。

如果`LazySingleton`实现了序列化接口`Serializable`，序列化和反序列化机制会破坏单例。解决办法同上。

通过反射可以创建出多个单例对象，破坏了单例模式的规则。解决办法同上。

测试代码如下所示：
```java
public class TestLazySingleton {
    public static void main(String[] args) throws IOException, ClassNotFoundException, NoSuchMethodException,
            IllegalAccessException, InvocationTargetException, InstantiationException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("testFile"));
        oos.writeObject(LazySingleton.getInstance());
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("testFile"));
        LazySingleton newInstance = (LazySingleton) ois.readObject();
        // 判断是否是同一个对象
        System.out.println(newInstance == LazySingleton.getInstance()); // 输出false

        Constructor cons = LazySingleton.class.getDeclaredConstructor();
        cons.setAccessible(true); // 设置强制访问
        LazySingleton newInstance2 = (LazySingleton) cons.newInstance();
        // 判断是否是同一个对象
        System.out.println(newInstance2 == LazySingleton.getInstance()); // 输出false
    }
}
```


## 双重锁机制
双重锁机制是懒汉式单例的升级版。

代码如下所示：
```java
public class DoubleCheckLockSingleton {
    private DoubleCheckLockSingleton() {}
    private static volatile DoubleCheckLockSingleton instance = null;
    
    public static DoubleCheckLockSingleton getInstance() {
        if (null == instance) {
            synchronized(DoubleCheckLockSingleton.class) {
                if (null == instance) {
                    instance = new DoubleCheckLockSingleton();
                }
            }
        }
        return instance;
    }
}
```

使用`synchronized`保证线程安全。

使用`volatile`保证可见性，以及避免指令重排序。如果不用`volatile`修饰，在新建对象（new关键字）那一行可能发生指令重排序。创建对象包括5个步骤：定位类的符号引用、如果类没有加载需要进行类加载、为对象分配内存、初始化对象头，执行`<init>`方法。指令重排序的问题是对象分配完内存可能还没有完成初始化，此时不为null，其他线程可能得到还没有初始化的单例对象。

如果`DoubleCheckLockSingleton`实现了序列化接口`Serializable`，序列化和反序列化机制会破坏单例。解决办法同上。

通过反射可以创建出多个单例对象，破坏了单例模式的规则。解决办法同上。

测试代码如下所示：
```java
public class TestDoubleCheckLockSingleton {
    public static void main(String[] args) throws IOException, ClassNotFoundException, NoSuchMethodException,
            IllegalAccessException, InvocationTargetException, InstantiationException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("testFile"));
        oos.writeObject(DoubleCheckLockSingleton.getInstance());
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("testFile"));
        DoubleCheckLockSingleton newInstance = (DoubleCheckLockSingleton) ois.readObject();
        // 判断是否是同一个对象
        System.out.println(newInstance == DoubleCheckLockSingleton.getInstance()); // 输出false

        Constructor cons = DoubleCheckLockSingleton.class.getDeclaredConstructor();
        cons.setAccessible(true); // 设置强制访问
        DoubleCheckLockSingleton newInstance2 = (DoubleCheckLockSingleton) cons.newInstance();
        // 判断是否是同一个对象
        System.out.println(newInstance2 == DoubleCheckLockSingleton.getInstance()); // 输出false
    }
}
```


## 静态内部类
静态内部类是一种懒汉式单例。在类中定义了一个静态内部类，由静态内部类负责创建和持有单例对象，保证了线程安全。

代码如下所示：
```java
public class StaticInnerClassSingleton {
    private StaticInnerClassSingleton() {}
    private static class Holder {
        public static final StaticInnerClassSingleton instance = new StaticInnerClassSingleton();
    }

    public static StaticInnerClassSingleton getInstance() {
        return Holder.instance;
    }
}
```

如果`StaticInnerClassSingleton`实现了序列化接口`Serializable`，序列化和反序列化机制会破坏单例。解决办法同上。

通过反射可以创建出多个单例对象，破坏了单例模式的规则。解决办法同上。

测试代码如下所示：
```java
public class TestStaticInnerClassSingleton {
    public static void main(String[] args) throws IOException, ClassNotFoundException, NoSuchMethodException,
            IllegalAccessException, InvocationTargetException, InstantiationException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("testFile"));
        oos.writeObject(StaticInnerClassSingleton.getInstance());
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("testFile"));
        StaticInnerClassSingleton newInstance = (StaticInnerClassSingleton) ois.readObject();
        // 判断是否是同一个对象
        System.out.println(newInstance == StaticInnerClassSingleton.getInstance()); // 输出false

        Constructor cons = StaticInnerClassSingleton.class.getDeclaredConstructor();
        cons.setAccessible(true); // 设置强制访问
        StaticInnerClassSingleton newInstance2 = (StaticInnerClassSingleton) cons.newInstance();
        // 判断是否是同一个对象
        System.out.println(newInstance2 == StaticInnerClassSingleton.getInstance()); // 输出false
    }
}
```


## 枚举
> [为什么我墙裂建议大家使用枚举来实现单例](https://blog.csdn.net/hollis_chuang/article/details/80728672)

枚举类可以认为是一种饿汉式单例。

代码如下所示：
```java
enum EnumSingleton {
    INSTANCE;
    
    public static EnumSingleton getInstance() {
        return INSTANCE;
    }
}
```

枚举类单例是比较优秀的实现单例模式的方式。线程安全，无法通过序列化以及反射来破坏单例。

测试代码如下所示：
```java
public class TestEnumSingleton {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("testFile"));
        oos.writeObject(EnumSingleton.getInstance());
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("testFile"));
        EnumSingleton newInstance = (EnumSingleton) ois.readObject();
        // 判断是否是同一个对象
        System.out.println(newInstance == EnumSingleton.getInstance()); // 输出true

        try {
            Constructor cons = EnumSingleton.class.getDeclaredConstructor(String.class, int.class);
            cons.setAccessible(true); // 设置强制访问
            EnumSingleton newInstance2 = (EnumSingleton) cons.newInstance(); // 抛出异常
            // 判断是否是同一个对象
            System.out.println(newInstance2 == EnumSingleton.getInstance());
        } catch (Exception e) {
            System.out.println("不允许通过反射创建对象");
            e.printStackTrace();
        }
    }
}
```

枚举防止反序列化破坏单例的原理是通过类名及类对象找到唯一的枚举类，所以不会产生多个实例。

枚举防止反射破坏单例的原因是JDK底层做了限制，当使用反射调用枚举类的构造器时，会抛出异常。

枚举实现单例虽有有很多优点，但是当单例数量比较多时却不方便管理。


# 装饰器模式
> [Java设计模式----装饰器模式](https://www.cnblogs.com/yxlaisj/p/10446504.html)

装饰器模式是一种行为型模式，可以给一个对象添加新的额外的功能，同时又不改变其结构。在装饰器模式中，需要创建一个装饰类，用来包装原有的类，并提供了额外的功能。

装饰器模式和代理模式很相似。装饰器模式专注于给被装饰对象添加额外的功能，并不判断客户的请求；而代理模式专注于对被代理对象的访问，可能会根据运行时的条件来判断是否转发客户的请求。

优点：1、装饰类和被装饰类可以独立发展，不会相互耦合。2、装饰模式是继承的一个替代模式，就增加功能来说，装饰器模式比继承更灵活。 

缺点：多层装饰比较复杂。

使用场景：扩展一个类的功能。 

实际例子：
- `synchronizedXXX`：用来装饰一个集合类，使之线程安全；
- `BufferedInputStream/BufferedOutputStream`：用来装饰一个输入/输出流，使之具有缓存功能；

代码示例：
```java
// 接口
interface Coffee {
    double getCost(); // 获取价格
    String getIngredients(); // 获取配料
}
// 简单咖啡
class SimpleCoffee implements Coffee {
    @Override
    public double getCost() {
        return 1;
    }

    @Override
    public String getIngredients() {
        return "Coffee";
    }
}
// 咖啡装饰器
abstract class CoffeeDecorator implements Coffee {
    protected final Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        decoratedCoffee = coffee;
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }

    public String getIngredients() {
        return decoratedCoffee.getIngredients();
    }
}
// 牛奶
class WithMilk extends CoffeeDecorator {
    public WithMilk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        double additionalCost = 0.5;
        return super.getCost() + additionalCost;
    }

    @Override
    public String getIngredients() {
        String additionalIngredient = "milk";
        return super.getIngredients() + ", " + additionalIngredient;
    }
}
// 糖
class WithSugar extends CoffeeDecorator {
    public WithSugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return super.getCost() + 1;
    }

    @Override
    public String getIngredients() {
        return super.getIngredients() + ", Sugar";
    }
}
// 测试
public class TestDeco {
    public static void main(String[] args) {
        Coffee c = new SimpleCoffee(); // 原味咖啡
        print(c);

        c = new WithMilk(c); // 加牛奶
        print(c);

        c = new WithSugar(c); // 加糖
        print(c);
    }

    public static void print(Coffee c) {
        System.out.println("============");
        System.out.println("花费: " + c.getCost());
        System.out.println("配料: " + c.getIngredients());
    }
}
```


# 享元模式
享元模式是一种结构型模式，主要用于减少创建对象的数量，以减少内存占用和提高性能。对于一个常用类（通常是不可变类），享元模式通常会创建并维护一定数量的该类对象，用于后续的的复用。如果想要创建的对象已经存在相同的或匹配的对象，则直接复用；如果没有相同的或匹配的对象，再创建新的对象。

主要解决：在有大量对象时，有可能会造成内存溢出，我们把其中共同的部分抽象出来，如果有相同的业务请求，直接返回在内存中已有的对象，避免重新创建。

何时使用：1、系统中有大量对象。2、这些对象消耗大量内存。3、需要缓冲池。

如何解决：用唯一标识码判断，如果在内存中有，则返回这个唯一标识码所标识的对象。

关键代码：用 HashMap 存储这些对象。

应用实例： 1、Java中的包装类，比如Boolean，Byte，Short，Integer，Long，Character。2、Java中的String table，如果有相同的对象则返回，如果没有则创建一个字符串保存在字符串缓存池里面。3、线程池，数据库连接池等。

优点：大大减少对象的创建，降低系统的内存，使效率提高。

举例：`Long`类会缓存-128~127之间的对象，在这个范围之间会重用缓存的对象，大于这个范围，才会新建对象。

相关代码如下所示：
```
public static Long valueOf(long l) {
    final int offset = 128;
    if (l >= -128 && l <= 127) { // will cache
        return LongCache.cache[(int)l + offset];
    }
    return new Long(l);
}
```

注意：
- Byte, Short, Long 缓存的范围都是 -128~127
- Character 缓存的范围是 0~127
- Integer的默认范围是-128~127
    - 最小值不能变
    - 最大值可以通过调整虚拟机参数`-Djava.lang.Integer.IntegerCache.high`来改变
- Boolean 缓存了 true 和 false



# 代理模式
> [狂神说Spring06：静态/动态代理模式](https://blog.csdn.net/qq_33369905/article/details/105828919)

> [Java的三种代理模式](https://www.cnblogs.com/leeego-123/p/10995975.html)

> [代理模式](https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/java/basic/java-proxy.md)

代理模式是一种结构性模式。代理模式给某一个目标对象（被代理对象）提供一个代理对象，并由代理对象控制对目标对象的访问。代理模式可以使得目标对象更加纯粹，公共的业务由代理对象来完成，实现了业务的分工。

根据代理对象创建的时期，代理模式可以分为静态代理模式和动态代理模式。
- 静态代理是在程序运行之前就创建了目标对象的代理类。
- 动态代理是在程序运行时通过反射机制创建目标对象的代理类。

代理模式如下图：

![设计模式 代理模式](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030102428.png?token=AE4F4YITUS2OG336F22HHZ27TN47K)

优点：1、职责清晰。2、高扩展性。

缺点：1、实现代理模式需要额外的工作，有些代理模式的实现非常复杂。2、由于在目标对象和客户之间增加了代理对象，可能会导致程序的处理速度变慢。

使用场景：1、保护代理。2、同步化代理。


## 静态代理模式
静态代理是在程序运行之前就创建了目标对象的代理类。在静态代理中，目标类与代理类需要实现相同的接口或者是继承相同的父类。

具体做法如下：

1. 创建一个接口，作为抽象角色，比如租房
```java
public interface Rent {
    public void rent();
}
```

2. 创建一个目标类，实现抽象角色接口，作为具体角色，比如房东
```java
public class Host implements Rent{
    public void rent() {
        System.out.println("房子租出去了");
    }
}
```

3. 创建一个代理类，要实现与目标类相同的接口，比如中介
```
public class Proxy implements Rent {
    private Host host;
    
    public Proxy(Host host) {
        this.host = host;
    }
 
    public void rent(){
        System.out.println("看房");  // 通常会有其他逻辑
        host.rent();
        System.out.println("交钱");
    }
}
```

4. 测试
```java
public class Test {
    public static void main(String[] args) {
        //房东要租房
        Host host = new Host();
        //中介帮助房东
        Proxy proxy = new Proxy(host);
        //去找中介租房子
        proxy.rent();
    }
}
```


## 动态代理模式
> [深入理解 Java 反射和动态代理 - 辞慾的文章 - 知乎](https://zhuanlan.zhihu.com/p/60805342)

> [【Java编程】一文彻底搞懂动态代理](https://zhuanlan.zhihu.com/p/52777197)

> [Java 动态代理作用是什么？ - bravo1988的回答 - 知乎](https://www.zhihu.com/question/20794107/answer/658139129)

动态代理是在程序运行时通过反射机制动态创建目标对象的代理类。

相比于静态代理：
1. 动态代理是在运行时动态生成代理类，并加载到JVM中的，而静态代理在编译之前就将代理类准备好。
2. 动态代理更加灵活，不需要必须实现接口，可以直接代理实现类，并且可以不需要针对每个目标类都创建一个代理类。在静态代理中，接口一旦新增加方法，目标对象和代理对象都要进行修改，这是非常麻烦的！

实现动态代理一般有两种方法：一是JDK自带的动态代理，二是CGLIB动态代理。

二者的不同点:
1. JDK动态代理只能只能代理实现了接口的类，而CGLIB可以代理未实现任何接口的类。
2. JDK动态代理生成的代理类本质上是`Proxy`的子类；CGLIB动态代理生成的代理类是被代理类的子类，代理类通常会重写父类的方法，所以父类和被重写的方法不能为`final`类型，而且父类还必须有空的构造方法，否则会报错。

二者的相同点: 都是使用字节码生成了新的代理类。

就二者的效率来说，大部分情况都是JDK动态代理更优秀，随着JDK版本的升级，这个优势更加明显。

很多知名的开源框架都使用到了动态代理，比如Spring中的AOP模块。在AOP模块中，如果目标对象实现了接口，则采用JDK动态代理，否则采用CGLIB动态代理。


### JDK动态代理
在JDK动态代理中，有两个重要的类（接口）：一个是`InvocationHandler`接口，另一个是`Proxy`类。

要实现JDK动态代理，需要创建一个类来实现`InvocationHandler`接口并重写`invoke`方法，该方法用来处理目标对象的方法调用。当代理对象调用目标对象的一个方法时，这个调用就会被转发到实现`InvocationHandler`接口的类的`invoke`方法上。

`InvocationHandler`接口源码：
```java
public interface InvocationHandler {
	public Object invoke(Object proxy, Method method, Object[] args)
	        throws Throwable;
}
```

参数说明：
- proxy：代理对象；
- method：所要调用目标对象的某个方法的Method对象
- args：所要调用目标对象的某个方法的参数

要实现动态创建一个代理对象，需要使用`Proxy`类，该类用来动态创建一个代理类，进而创建一个代理对象，使用代理对象调用目标对象的方法。

`Proxy`类提供了很多方法，最常用的是静态方法`newProxyInstance`：
```
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,  InvocationHandler h) throws IllegalArgumentException
```

参数说明：
- loader：类加载器，定义了由哪个类加载器对象来加载代理类，通常为目标对象的类的类加载器；
- interfaces：一组接口，使动态创建的代理类实现该组接口，保证代理对象可以调用接口中的方法，通常为目标对象的类实现的所有接口；
- h：实现了`InvocationHandler`接口的类的实例，用来处理目标对象的方法调用；


代码示例：
1. 创建一个接口
```java
public interface Subject {
    void hello(String str); // 没有返回值
    String bye();           // 有返回值
}
```

2. 创建一个真实类（目标类，被代理类）
```java
public class RealSubject implements Subject {
    public void hello(String str) {
        System.out.println("Hello, " + str);
    }

    public String bye() {
        System.out.println("Goodbye");
        return "Over";
    }
}
```

3. 创建一个类，需要实现InvocationHandler接口，用来处理目标对象的方法调用
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class SubjectInvocationHandler implements InvocationHandler {
    private final Object target;

    public SubjectInvocationHandler(Object target) {
        this.target = target;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method");
        Object result = method.invoke(target, args);
        System.out.println("After method");
        return result;
    }
}
```

4. 创建一个目标对象的代理对象，可以使用一个简单的代理工具类
```java
import java.lang.reflect.Proxy;

public class JdkProxyUtil {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                new SubjectInvocationHandler(target));
    }
}
```

5. 测试
```java
public class Test {
    public static void main(String[] args) {
        Subject subject = new RealSubject(); // 目标对象
        Subject proxy = (Subject) JdkProxyUtil.getProxy(subject); // 得到目标对象的代理对象

        System.out.println(proxy.getClass().getName()); // 代理对象是Proxy的子类
        proxy.hello("World");
        String result = proxy.bye();
        System.out.println("Result is: " + result);
    }
}
```


### CGLIB动态代理
> [cglib](https://github.com/cglib/cglib)

JDK动态代理最大的问题是只能代理实现了接口的类。为了解决这个问题，可以使用CGLIB动态代理机制。

CGLIB（Code Generation Library）是一个基于ASM的字节码生成库，它允许我们在运行时对字节码进行修改和动态生成。CGLIB通过继承方式实现代理，重写父类的公开方法，并在重写时进行增强。

在CGLIB动态代理中， 有两个核心类（接口）：一是`MethodInterceptor`接口，通常叫做方法拦截器，二是`Enhancer`类。

要实现CGLIB动态代理，需要创建一个类来实现`MethodInterceptor`接口并重写`intercept`方法，该方法用于拦截并增强被代理类的方法。

`MethodInterceptor`的源码如下所示：
```java
package net.sf.cglib.proxy;

import java.lang.reflect.Method;

public interface MethodInterceptor extends Callback{
    public Object intercept(Object obj, java.lang.reflect.Method method, Object[] args, MethodProxy proxy) throws Throwable;
}
```

参数说明：
- obj：被代理的对象（需要增强的对象）；
- method：被拦截的方法（需要增强的方法）；
- args：方法参数；
- methodProxy：用于调用原始方法；

要实现动态创建一个代理对象，需要使用`Enhancer`类来动态获取被代理类。当代理对象调用方法时，实际会调用`MethodInterceptor`的`intercept`方法。


代码示例：
1. 不同于JDK动态代理不需要额外的依赖，CGLIB实际是一个开源项目，如果要使用它的话，需要手动添加相关依赖。
```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.3.0</version>
</dependency>
```

2. 首先有一个真实类（目标类）
```java
public class RealSubject {
    public void hello(String str) {
        System.out.println("Hello, " + str);
    }

    public String bye() {
        System.out.println("Goodbye");
        return "Over";
    }
}
```

3. 创建一个类，需要实现`MethodInterceptor`接口，用来处理目标对象的方法调用
```java
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class RealSubjectMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        System.out.println("Before method");
        Object result = methodProxy.invokeSuper(o, args);
        System.out.println("After method");
        return result;
    }
}
```

4. 创建一个目标对象的代理对象，需要使用`Enhancer`类，可以使用一个简单的代理工具类
```java
import net.sf.cglib.proxy.Enhancer;

public class CglibProxyUtils {
    public static Object getProxy(Class<?> clazz) {
        Enhancer enhancer = new Enhancer();
        enhancer.setClassLoader(clazz.getClassLoader());
        enhancer.setSuperclass(clazz);
        enhancer.setCallback(new RealSubjectMethodInterceptor());
        return enhancer.create();
    }
}
```

5. 测试
```java
public class Test {
    public static void main(String[] args) {
        RealSubject realSubjectProxy = (RealSubject) CglibProxyUtils.getProxy(RealSubject.class);
        System.out.println(realSubjectProxy.getClass().getName());
        realSubjectProxy.hello("world");
        String res = realSubjectProxy.bye();
        System.out.println(res);
    }
}
```





# 观察者模式
> [观察者模式](https://www.runoob.com/design-pattern/observer-pattern.html)

> [简说设计模式——观察者模式](https://www.cnblogs.com/adamjwh/p/10913660.html)

> [JAVA提供的对观察者模式的支持(观察者模式)](https://blog.csdn.net/hbiao68/article/details/52682858)

观察者模式是一种行为型模式，又叫发布-订阅模式。观察者模式用于定义对象之间的依赖关系，当一个对象（目标对象）发生改变的时候，会自动通知它的所有依赖对象（观察者对象）并让他们进行更新。

UML图如下所示：

![设计模式 观察者模式](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030102511.png?token=AE4F4YNF6TGYMIMTO25BHGS7TN5CA)

关键代码：在抽象类里定义一个集合存放观察者。

优点：1、观察者和被观察者是抽象耦合的。2、建立一套触发机制。

缺点：1、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。2、如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。

实际例子：Spring事件驱动模型就是观察者模式很经典的一个应用。比如我们每次添加商品的时候都需要重新更新商品索引，就可以利用观察者模式来解决这个问题。

注意事项：1、Java中已经有了对观察者模式的支持：被观察类`Observable`和观察者接口`Observer`。2、避免循环引用。3、如果顺序执行，某一观察者错误会导致系统卡壳，一般采用异步方式。

观察者接口`Observer`的源码如下所示：
```java
// java.util.Observer
public interface Observer {
    void update(Observable o, Object arg);
}
```

观察者接口`Observer`只定义了一个方法，即`update`方法。当被观察者对象的状态发生变化时，被观察者对象的`notifyObservers`方法就会调用这一方法。


被观察者类`Observable`的源码如下所示：
```java
// java.util.Observable
public class Observable {
    private boolean changed = false;
    private Vector<Observer> obs; // 观察者

    public Observable() {
        obs = new Vector<>();
    }
    
    public synchronized void addObserver(Observer o) {
        if (o == null)
            throw new NullPointerException();
        if (!obs.contains(o)) {
            obs.addElement(o);
        }
    }

    public synchronized void deleteObserver(Observer o) {
        obs.removeElement(o);
    }

    public void notifyObservers() {
        notifyObservers(null);
    }

    public void notifyObservers(Object arg) {
        Object[] arrLocal;

        synchronized (this) {
            if (!changed)
                return;
            arrLocal = obs.toArray();
            clearChanged();
        }

        for (int i = arrLocal.length-1; i>=0; i--)
            ((Observer)arrLocal[i]).update(this, arg);
    }

    public synchronized void deleteObservers() {
        obs.removeAllElements();
    }

    protected synchronized void setChanged() {
        changed = true;
    }

    protected synchronized void clearChanged() {
        changed = false;
    }

    public synchronized boolean hasChanged() {
        return changed;
    }

    public synchronized int countObservers() {
        return obs.size();
    }
```

观察这类`Observable`用于被被观察者类继承，并且提供了公开的方法支持添加和通知观察者对象，比如：
- `setChanged`方法：设置被观察者的标记变量，表示被观察者的状态发生了变化；
- `notifyObservers`方法：通知所有登记过的观察者对象，并调用每个对象的`update`方法进行更新；

代码示例：
```java
// 被观察者
public class Watched extends Observable {
    private String data = "";

    public String getData() {
        return data;
    }

    public void setData(String data) {
        if (!this.data.equals(data)) {
            this.data = data;
            setChanged();
        }
        notifyObservers();
    }
}
// 观察者
public class Watcher implements Observer {
    public void update(Observable o, Object arg) {
        System.out.println("状态发生改变：" + ((Watched) o).getData());
    }
}
// 测试
public class Test {
    public static void main(String[] args) {
        Watched watched = new Watched();
        Observer watcher = new Watcher();
        watched.addObserver(watcher);
        watched.setData("start");
        watched.setData("run");
        watched.setData("stop");
    }
}
```


# 模板模式
> [模板模式](https://www.runoob.com/design-pattern/template-pattern.html)

模板模式是一种行为型模式。模板模式指的是在抽象类中定义一个操作的算法骨架（模板函数），而将算法的一些步骤（基本函数）延迟到子类中实现。模板方法使得子类在不改变一个算法的结构的前提下，能够定义该算法的某些特定步骤。

UML图如下所示：

![设计模式 模板模式](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030102926.png?token=AE4F4YM7DLL6W5AS5K2K5CS7TN5R6)

优点：1、封装不变部分，扩展可变部分。2、提取公共代码，便于维护。3、行为由父类控制，子类实现。

缺点：每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大。

使用场景：1、有多个子类共有的方法，且逻辑相同。2、重要的、复杂的方法，可以考虑作为模板方法。

实际例子：AQS。

代码示例：
```java
// 抽象类
public abstract class Game {
   abstract void initialize();
   abstract void startPlay();
   abstract void endPlay();

   public final void play(){
      initialize(); //初始化游戏
      startPlay(); //开始游戏
      endPlay(); //结束游戏
   }
}
// 实现类
public class Cricket extends Game {
   @Override
   void endPlay() {
      System.out.println("Cricket Game Finished!");
   }
 
   @Override
   void initialize() {
      System.out.println("Cricket Game Initialized! Start playing.");
   }
 
   @Override
   void startPlay() {
      System.out.println("Cricket Game Started. Enjoy the game!");
   }
}
// 实现类
public class Football extends Game {
   @Override
   void endPlay() {
      System.out.println("Football Game Finished!");
   }
 
   @Override
   void initialize() {
      System.out.println("Football Game Initialized! Start playing.");
   }
 
   @Override
   void startPlay() {
      System.out.println("Football Game Started. Enjoy the game!");
   }
}
// 测试
public class TemplatePatternDemo {
   public static void main(String[] args) {
      Game game = new Cricket();
      game.play();
      System.out.println();
      game = new Football();
      game.play();      
   }
}
```



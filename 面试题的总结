

# 面试题总结

## 下面一段代码来进行分析，i，k，j的值是什么？

```java
public void main(String[] args){
    int i = 1;
    i = i++;
    int j = i++;
    int k = i + ++i * i++ ;
    System.out.println(i+ "---" + j + "--- " + k);
}
```

其中 i = 4，j = 1, k =11。

![image-20220312155614019](D:\JavaProject\面试题总结Images\image-20220312155614019.png)



## 编写单例模式

第一种，使用静态变量直接创建对象

```java
/**
 *  饿汉式 一
 */
public class Singleton1 {
    public  static final  Singleton1 INSTANCE = new Singleton1();

    private Singleton1(){

    }

}

```

第二种，使用enum类来完成饿汉式单例模式

```java
public enum Singleton2 {
    INSTANCE;
}
```

第三种，使用静态代码块的方式来实现单例模式

```java
public class Singleton3 {

    public static final  Singleton3 INSTANCE;
    private String name;

    static{
        Properties properties = new Properties();
        try {
            properties.load(Singleton3.class.getClassLoader().getResourceAsStream("singletonProperties.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        INSTANCE = new Singleton3(properties.getProperty("name"));
    }
    private Singleton3(String name){

        this.name = name;
    }

    @Override
    public String toString() {
        return "Singleton3{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

第四种，采用线程不安全的方式来实现单例模式(懒汉模式)

```java
public class Singleton4 {

    private static Singleton4 INSTANCE ;
    private Singleton4(){

    }

    public static Singleton4 getInstance(){
        if(INSTANCE == null){
            INSTANCE = new Singleton4();
        }
        return INSTANCE;
    }
}


//测试类
@Test
    public void testSingleton4() throws ExecutionException, InterruptedException {
//        Singleton4 singleton1 = Singleton4.getInstance();
//        Singleton4 singleton2 = Singleton4.getInstance();
//
//
//        System.out.println(singleton1 == singleton2);
//        System.out.println(singleton1);
//        System.out.println(singleton2);


        Callable<Singleton4> singleton4Callable = new Callable<Singleton4>() {
            @Override
            public Singleton4 call() throws Exception {
                return Singleton4.getInstance();
            }
        };

        ExecutorService pool = Executors.newFixedThreadPool(2);
        Future<Singleton4> singleton4Future1 = pool.submit(singleton4Callable);
        Future<Singleton4> singleton4Future2 = pool.submit(singleton4Callable);

        Singleton4 singleton4 = singleton4Future1.get();
        Singleton4 singleton41 = singleton4Future2.get();

        System.out.println(singleton4 == singleton41);
        System.out.println(singleton4);
        System.out.println(singleton41);
    }
```

第五种，采用线程安全的懒汉式单例对象

```java
public class Singleton5 {

    private static Singleton5 INSTANCE ;
    private Singleton5(){

    }

    public synchronized static Singleton5 getInstance(){
        if(INSTANCE == null){
            INSTANCE = new Singleton5();
        }
        return INSTANCE;
    }
}
```

第六种，采用静态内部类的方式来实现单例模式

```java
public class Singleton6 {

    private Singleton6(){

    }

    public static Singleton6 getInstance(){
        return Inner.singleton6;
    }

    public static class Inner{
        private static Singleton6 singleton6 = new Singleton6();
    }
}
```

## 类初始化和实例初始化

```java
package com.haust.initFatherSon;

public class Father {
    private int i = test();
    private static int j = method();

    static {
        System.out.println("(1)");

    }
    Father(){
        System.out.println("(2)");

    }
    {
        System.out.println("(3)");
    }

    public static int method() {
        System.out.println("(5)");
        return 1;
    }

    public int test() {
        System.out.println("(4)");
        return 1;
    }
}

```

```java
package com.haust.initFatherSon;
/**
	super()
	属性实例化 、代码块按所在的顺序执行
	构造器
*/
public class Son extends Father{
    private int i = test();
    private static int j = method();

    static {
        System.out.println("(6)");

    }
    Son(){
        System.out.println("(7)");

    }
    {
        System.out.println("(8)");
    }

    public static int method() {
        System.out.println("(9)");
        return 1;
    }

    public int test() {
        System.out.println("(10)");
        return 1;
    }

    public static void main(String[] args) {
        Son s1 = new Son();
        System.out.println();
        Son s2 = new Son();
    }
}

```

```tex
(5)
(1)
(9)
(6)
(10)
(3)
(2)
(10)
(8)
(7)

(10)
(3)
(2)
(10)
(8)
(7)

```

**由父及自，静态先行**

类初始化过程
一个类要创建实例需要先加载并初始化该类
main方法所在的类需要先加载和初始化
一个子类要初始化需要先初始化父类
一个类初始化就是执行<clinit>()方法
<clinit>()方法由**静态类变量显示赋值代码和静态代码块组成类变量显示赋值代码和静态代码块代码从上到下顺序执行**<clinit>()方法只执行一次

①实例初始化就是执行<init>()方法
<init>()方法可能重载有多个，有几个构造器就有几个<init>方法
<init>()方法由**非静态实例变量显示赋值代码和非静态代码块、对应构造器代码组成**
**非静态实例变量显示赋值代码和非静态代码块代码从上到下顺序执行**，而对应构造器的代码最后执行
每次创建实例对象，调用对应构造器，执行的就是对应的<init>方法
<init>方法的首行是super()或super(实参列表)，即对应父类的<init>方法

哪些方法不可以被重写final方法
静态方法
private等子类中不可见方法对象的多态性

子类如果重写了父类的方法，通过子类对象调用的一定是子类重写过的代码非静态方法默认的调用对象是this
this对象在构造器或者说<init>方法中就是正在创建的对象

## 方法的参数传递机制

```java
package com.haust.argsTransmit;

import java.util.Arrays;

public class Exam {
    public static void main(String[] args) {
        int i = 1;
        String str = "hello";
        Integer num = 2;
        int[] arr = {1, 2, 3, 4, 5};
        MyData my = new MyData();
        change(i, str, num, arr, my);
        System.out.println("i =" + i);
        System.out.println("str = " + str);
        System.out.println("num = " + num);
        System.out.println("arr = " + Arrays.toString(arr));
        System.out.println("my.a = " + my.a);

    }
    public static void change(int j, String s, Integer n,int[] a, MyData m){
        j += 1;
        s += "world";
        n += 1;
        a[0] += 1;
        m.a += 1;
    }

}

class MyData{
    int a = 10;
}


i =1
str = hello
num = 2
arr = [2, 2, 3, 4, 5]
my.a = 11
```

- 方法的参数传递机制 
- String、包装类等对象的不可变性

## 递归和迭代

·方法调用自身称为递归，利用变量的原值推出新值称为迭代。·递归
·优点:大问题转化为小问题，可以减少代码量，同时代码精简，可读性好;·缺点:递归调用浪费了空间，而且递归太深容易造成堆栈的溢出。
·迭代
·优点:代码运行效率好，因为时间只因循环次数增加而增加，而且没有额外的空间开销;
·缺点:代码不如递归简洁，可读性好

```java
public class Teststep{
    @Test
    public void test(){
        long start = System.currentTimeMillis();
        system.out.println(f(40));//165580141
        long end = System.currentTimeMiLlis();
        system.out.println(end-start); //586ms
    }
//实现f(n):求n步台阶，一共有几种走法
    public int f(int n){
        if(n<1){
            throw new IllegalArgumentException(n +"不能小于1");
        }
        if(n==1 ll n==2){
            return n;
        }
        return f(n-2) + f(n-1);
    }
}

```

```java
public int loop(int n){
    if(n<1){
        throw new 1llegalArgumentException(n +"不能小于1");
    }
    if(n==1 ll n==2){
        return n;
    }
    int one = 2;//初始化为走到第二级台阶的走法
    int two = 1;//初始化为走到第一级台阶的走法
    int sum = 0;
    for(int i=3; i<=n; i++){
//最后跨2步＋最后跨1步的走法
        sum = two + one;
        two = one;
        one = sum;
    }
    return sum;
}

```

## 成员变量和局部变量

就近原则变量的分类
成员变量∶类变量、实例变量·局部变量
非静态代码块的执行∶每次创建实例对象都会执行方法的调用规则︰

调用一次执行一次

![image-20220313112820131](D:\JavaProject\面试题总结Images\image-20220313112820131.png)

**堆(Heap)**，此内存区域的唯一目的就是存放对象实例几乎所有的对象实例都在这
里分配内存。这一点在Java虚拟机规范中的描述是:所有的对象实例以及数组都要在堆上分配。
**通常所说的栈(Stack)**，是指虚拟机栈。虚拟机栈用于存储局部变量表等。局部变量表存放了编译期可知长度的各种基本数据类型(boolean、byte、char、short、int、float,long、double)、对象引用(reference类型，它不等同于对象本身，是对象在堆内存的首地址）。方法执行完，自动释放。
**方法区(Method Area）**用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

```java
public class Exam5 {
    static int s;
    int i;
    int j;
    {
        int i = 1;
        i++;
        j++;
        S++;
    }
    public void test(int j){//形参，局部变量,j
        j++;
        i++;
        S++;
    }
    public static void main(String[] args) {//形参，局部变量，args.
        Exam5 obj1 = new Exam5();//局部变量，obj1
        Exam5 obj2 = new Exam5();//局部变量，obj1obj1.test(10);
        obj1.test(20);
        obj2.test(30);
        System.out.println(obj1.i + "," + obj1.j + "," + obj1.s);
        System.out.println(obj2.i + "," + obj2.j + "," + obj2.s);
    }
}
//2 1 5
//1 1 5
```



### 局部变量与成员变量的不同

- 声明的位置
  - 局部变量:方法体{}中，形参，代码块{}中
  - 成员变量:类中方法外
    - 类变量:有static修饰
    - 实例变量:没有static修饰
- 修饰符
  - 局部变量: final
  - 成员变量: public、protected、private、final、static、volatile、transient
- 值存储的位置
  - 局部变量:栈
  - 实例变量:堆
  - 类变量:方法区
- 作用域
  - 局部变量:从声明处开始，到所属的}结束
  - 实例变量:在当前类中“this.”(有时this.可以缺省)，在其他类中“对象名.”访问
  - 类变量:在当前类中“类名.”(有时类名.可以省略),在其他类中“类名.”或“对象名.”访问
- 生命周期
  - 局部变量:每-一-个线程，每-一次调用执行都是新的生命周期
  - 实例变量:随着对象的创建而初始化，随着对象的被回收而消亡，每---个对象的实例变量是独立的
  - 类变量:随着类的初始化而初始化，随着类的卸载而消亡，该类的所有对象的类变量是共享的



### 当局部变量与XX变量重名时，如何区分？

- ①局部变量与实例变量重名
  - 在实例变量前面加“this.”
- ②局部变量与类变量重名
  - 在类变量前面加“类名.”

## SSM面试题，spring bean 作用域之间有什么区别？

在Spring中，可以在<bean>元素的scope属性里设置bean的作用域，以决定这个bean是单实例的还是多实例的。
默认情况下，Spring 只为每个在 IOC"容器里声明的bean创建唯一一个实例，整个IOC容器范围内都能共享该实例:所有后续的 getBean()调用和 bean引用都将返回这个唯一的bean实例。该作用域被称为singleton，它是所有bean的默认作用域。

![image-20220313114924908](D:\JavaProject\面试题总结Images\image-20220313114924908.png)

## SSM面试题，spring 常用数据库事务传播属性和事务隔离级别

事务的属性:
1.*propagation:用来设置事务的传播行为
事务的传播行为:一个方法运行在了一个开启了事务的方法中时，当前方法是使用原来的事务还是开启一个新的事务-Propagation.REQUIRED:默认值，使用原来的事务
-Propagation.REQUIRES_NEW:将原来的事务挂起，开启一个新的事务*

*2.*isolation:用来设置事务的隔离级别
-Isolation.REPETABLE_READ:可重复读，MySQL默认的隔离级别
-Isolation.REPETABLE_SMMITTED:读已提交，Oracle默认的隔离级别，开发时通常使用的隔离级别

spring中事务传播的七种类型

![image-20220313115439773](D:\JavaProject\面试题总结Images\image-20220313115439773.png)

隔离级别

![image-20220313120319604](D:\JavaProject\面试题总结Images\image-20220313120319604.png)

| Property                                                     | Type                                                         | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [value](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#tx-multiple-tx-mgrs-with-attransactional) | `String`                                                     | 指定要使用的事务管理器的可选限定符                           |
| [propagation](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#tx-propagation) | `enum`: `Propagation`                                        | 可选的传播设置                                               |
| `isolation`                                                  | `enum`: `Isolation`                                          | 可选的事务超时。仅适用于`REQUIRED`或的传播值`REQUIRES_NEW`   |
| `timeout`                                                    | `int` (in seconds of granularity)                            | 读写与只读事务。仅适用于`REQUIRED`或的值`REQUIRES_NEW`。     |
| `readOnly`                                                   | `boolean`                                                    | 必须导致回滚的异常类的可选数组。                             |
| `rollbackFor`                                                | Array of `Class` objects, which must be derived from `Throwable.` | 必须导致回滚的异常类的可选数组。.                            |
| `rollbackForClassName`                                       | Array of class names. The classes must be derived from `Throwable.` | 必须导致回滚的异常类名称的可选数组。                         |
| `noRollbackFor`                                              | Array of `Class` objects, which must be derived from `Throwable.` | 不得导致回滚的异常类的可选数组                               |
| `noRollbackForClassName`                                     | Array of `String` class names, which must be derived from `Throwable.` | 不得导致回滚的异常类名称的可选数组。                         |
| `label`                                                      | Array of `String` labels to add an expressive description to the transaction. | 事务管理器可以评估标签以将特定于实现的行为与实际事务相关联。 |

## SPring MVC如何解决请求乱码问题

```java
public class MyWebAppInitializer extends AbstractDispatcherServletInitializer {

    // ...

    @Override
    protected Filter[] getServletFilters() {
        return new Filter[] {
            new HiddenHttpMethodFilter(), new CharacterEncodingFilter() };
    }
}
```

配置CharacterEncodingFilter

Get请求的话可以使用修改web.xml

## 浅谈一下Spring MVC的工作流程

![image-20220313121957788](D:\JavaProject\面试题总结Images\image-20220313121957788.png)

首先用户发送请求，到达dispatcherServlet时，由dispatcherServlet调用处理器映射器找到对应的处理器。到达handleMapping处理器映射器时，映射器会返回HandlerExecutionChain，同时会激发handerIntecepter处理器拦截器。之后通过处理器适配器调用对应的处理器，处理器适配器会调用处理器controller来返回相对应的ModelView，返回的ModelView到达中央控制器后，中央处理器将视图发送到ViewResolver视图解析器，之后视图解析器将视图解析之后返回中央控制器。中央控制器渲染视图，最后将视图响应给用户。

## Mybatis中当前实体类中的属性名和表中的字段名不一样，怎么办？

1. 使用sql的时候给sql属性起别名
2. 在mybatis的配置文件中开启驼峰命名规则 mapUnderscoreToCamelCase
3. 在mapper映射文件中，自定义映射规则resultMap

## linux系统，常用服务类相关命令？

### service (centos6)

- 注册在系统中的标准化程序
- 有方便统一的管理方式（常用的方法)
  - service 服务名start
  - service服务名stop
  - service服务名restart
  - service服务名reload.
  - service服务名status
- 查看服务的方法/etc/init.d
- 服务名通过chkconfig命令设置自启动
- 查看服务 chkconfig --list|grep xXx
- chkconfig --level 5 服务名on.

运行级别runlevel(centos6)开机

- BIOS
- /boot
- init进程

运行级别
查看默认级别:vi /etc/inittab
运行级对应的服务
Linux系统有7种运行级别(runlevel):常用的是级别3和5

- 运行级别0:系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
- 运行级别1:单用户工作状态，root权限，用于系统维护，禁止远程登陆
- 运行级别2:多用户状态(没有NFS)，不支持网络
- 运行级别3:完全的多用户状态(有NFS)，登陆后进入控制台命令行模式·

- 运行级别4:系统未使用，保留
- 运行级别5:X11控制台，登陆后进入图形GUl模式
- 运行级别6:系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

### systemctl (centos7)

- 注册在系统中的标准化程序
- 有方便统一的管理方式（常用的方法)
  - systemctl start 服务名(xxxx.service)
  - systemctl restart 服务名(xxxx.service)
  - systemctl stop服务名(xxxx.service)
  - systemctl reload 服务名(xxxx.service)
  - systemctl status服务名(xxxx.service)
- 查看服务的方法/usr/lib/systemd/system
- 查看服务的命令
  - systemctl list-unit-files
  - systemctl --type service
- 通过systemctl命令设置自启动
  自启动systemctl enable service_name
- 不自启动systemctl disable service_name

## Git分支相关命令

- 创建分支
  - git branch<分支名>
  - git branch -v查看分支
- 切换分支
  - git checkout<分支名>
  - —步完成:git checkout -b <分支名>
- 合并分支
  - 先切换到主干git checkout master
  - git merge<分支名>
- 删除分支
  - 先切换到主干git checkout master
  - git branch -D<分支名>

![image-20220313175024965](D:\JavaProject\面试题总结Images\image-20220313175024965.png)

## Redis持久化问题

RDB
在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

备份是如何执行的
Redis会单独创建(fork)一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

rdb的优点。

- 节省磁盘空间
- 恢复速度快

rdb的缺点

- 虽然Redis在fork时使用了写时拷贝技术,但是如果数据庞大时还是比较消耗性能。
- 在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。

![image-20220313180012904](D:\JavaProject\面试题总结Images\image-20220313180012904.png)

AOF
以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件但不可以改写文件，Redis启动之初会读取该文件重新构建数据，换言之，Redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

AOF的优点

- 备份机制更稳健，丢失数据概率更低。
- 可读的日志文本，通过操作AOF稳健，可以处理误操作。

AOF的缺点

- 比起RDB占用更多的磁盘空间。

- 恢复备份速度要慢。
- 每次读写都同步的话，有一定的性能压力。
- 存在个别Bug，造成恢复不能。

![image-20220313180326622](D:\JavaProject\面试题总结Images\image-20220313180326622.png)

## Mysql什么时候建立索引

主键自动建立唯一索引
频繁作为查询条件的字段应该创建索引
查询中与其它表关联的字段，外键关系建立索引单键/组合索引的选择问题，组合索引性价比更高
查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度查询中统计或者分组字段

哪些情况不适合创建索引？

表记录太少
经常增删改的表或者字段
Where条件里用不到的字段不创建索引

过滤性不好的不适合建索引

## JVM垃圾回收机制

垃圾回收机制只存在堆空间中。元空间在JDK8之后被放入到堆中。但是常量池等等数据不进行垃圾回收。

垃圾回收主要分为两种，young区的minor gc，old区的full gc。基本不懂perm区(metadata)

垃圾回收机制主要有四种算法

- 复制算法
- 引用计数法
- 标记清除
- 标记压缩

复制算法常常出现在young中，将幸存者1区的数据复制到幸存者2区中(可逆转)。

引用计数法已经被淘汰了，因为可能会出现循环引用导致一直无法被回收。

标记清除和标记压缩是同时使用在old区的。

## redis在项目中的使用场景

| 数据类型 | 使用场景                                                     |
| -------- | ------------------------------------------------------------ |
| String   | 比如说，我想知道什么时候封锁一个IP地址。Incrby命令           |
| Hash     | 存储用户信息【ids name，age】 <br />Hset(kex.field.value)<br/>Hset(userKev.id,101)e<br/>Hset(userKev-name.admin)<br />Hset(userKev.age,23)<br/>----修改案例----<br />Hget(userKev.id)<br />Hset(userKey.id,102)e<br/>为什么不使用String 类型来存储Set(userKey,用信息的字符串)<br />Get(userKey)<br />不建议使用String |
| List     | 实现最新消息的排行，还可以利用List的push命令，将任务存在list集合中，同时使用另一个命令﹐将任务从集合中取出[pop]<br/>Redis—list数据类型来模拟消息队列。【电商中的秒杀就可以采用这种方式来完成一个秒杀活动】 |
| Set      | 特殊之处:可以自动排重。比如说微博中将每个人的好友存在集合(Set)中这样求两个人的共通好友的操作。我们只需要求交集即可· |
| Zset     | 以某一个条件为权重,进行排序。<br/>京东:商品详情的时候，都会有一个综合排名，还可以按昭价格进行排序 |

## Elasticsearch和solr的区别

背景:它们都是基于Lucene搜索服务器基础之上开发，一款优秀的，高性能的企业级搜索服务器。【是因为他们都是基于分词技术构建的倒排索引的方式进行查询】
开发语言: java语言开发
诞生时间:
Solr : 2004年诞生。Es: 2010年诞生。vEs更新【功能越强大】vv
区别:

- 当实时建立索引的时候，solr 会产生io阻塞，而es则不会，es查询性能要高于solr

- 在不断动态添加数据的时候，solr的检索效率会变的低下，而es则没有什么变化。

- Solr利用zookeeper进行分布式管理，而es自身带有分布式系统管理功能。
- Solr一般都要部署到web服务器上，比如tomcat。启动tomcat 的时候需要配置tomcat与solr的关联。【solr的本质是一个动态web项目】
- Solr支持更多的格式数据[xm，json,csv 等]，而es仅支持json文件格式。
- Solr是传统搜索应用的有力解决方案，但是es更适用于新兴的实时搜索应用。单纯的对已有数据进行检索的时候，solr效率更好，高于es
- solr官网提供的功能更多，而es本身更注重于核心功能，高级功能多有第三方插件。

![image-20220313182552171](D:\JavaProject\面试题总结Images\image-20220313182552171.png)



![image-20220313182620774](D:\JavaProject\面试题总结Images\image-20220313182620774.png)



## 单点登录的实现过程

单点登录，一处登陆多处使用

![image-20220313185003146](D:\JavaProject\面试题总结Images\image-20220313185003146.png)

Demo: 
参观动物园流程:检票员=认证中心模块·
1.我直接带着大家进动物园，则会被检票员拦住【看我们是否有门票】，没有[售票处买票登录=买票v
2．我去买票【带着票，带着大家一起准备进入动物园】检票员check【有票】Token=piao w
3.我们手中有票就可以任意观赏动物的每处景点。v京东:单点登录，是将token放入到cookie中的。
案例:将浏览器的cookie 禁用，则在登录京东则失败!无论如何登录不了! 

## 购物车实现过程

- 购物车跟用户的关系?
  - 一个用户必须对应一个购物车【一个用户不管买多少商品，都会存在属于自己的购物车中。】
  - 单点登录一定在购物车之前。
- 跟购物车有关的操作有哪些?
  - 添加购物车,
    - 用户未登录状态
    - 添加到什么地方?未登录将数据保存到什么地方?
      - Redis? ---京东
      - Cookie? ---自己开发项目的时候【如果浏览器禁用cookie】
    - 用户登录状态
      - Redis缓存中【读写速度快】。
        - Hash： hset（key，filed，value）
          - key ：user：userid：cart
          - Hset(key,skuid,value)
      - 存在数据库中【oracle，mysql】
  - 展示购物车。
    - 未登录状态展示·
      - 直接从cookie中取得数据展示即可。
    - 登录状态
- 用户一旦登录:必须显示数据库【redis 】 +cookie中的购物车的数据
  - cookie中有三条记录
  - redis中有五条纪律
  - 真正展示的时候应该有8条记录

## 消息队列在项目中的使用

背景:在分布式系统中是如何处理高并发的。↓
由于在高并发的环境下，来不及同步处理用户发送的请求，则会导致请求发生阻塞。比如说，大量的insert，update之类的请求同时到达数据库MYSQL，直接导致无数的行锁表锁，甚至会导致请求堆积很多。从而触发too many connections错误。

![image-20220313205902174](D:\JavaProject\面试题总结Images\image-20220313205902174.png)

![image-20220313210109265](D:\JavaProject\面试题总结Images\image-20220313210109265.png)



![image-20220313210127617](D:\JavaProject\面试题总结Images\image-20220313210127617.png)

### 消息队列在电商中的使用场景

![image-20220313210219639](D:\JavaProject\面试题总结Images\image-20220313210219639.png)



消息队列的弊端:

- 消息的不确定性:延迟队列，轮询技术来解决该问题即可!

## 请谈谈你对volatile的理解

volatile是Java虚拟机提供的轻量级的同步机制

- 保证可见性
- 不保证原子性
- 禁止指令重排

### Jvm模型讲解

JMM(Java内存模型Java Memcry Model，简称JMM)本身是一种抽象的概念并不真实存在，它描述的是一组规则或规范，通过这组规范定义了程序中各个变量（包括实例字段，静态字段和构成数组对象的元素）的访问方式。

JMM关于同步的规定:

- 1 线程解锁前，必须把共享变量的值刷新回主内存
- 2线程加锁前，必须读取主内存的最新值到自己的工作内存
- 3加锁解锁是同一把锁

由于JVM运行程序的实体是线程，而每个线程创建时JVM都会为其创建一个工作内存(有些地方称为栈空间)，工作内存是每个线程的私有数据区域，而Java内存模型中规定所有变量都存储在主内存，主内存是共享内存区域，所有线程都可以访问，但线程对变量的操作(读取赋值等)必须在工作内存中进行，首先要将变量从主内存拷贝的自己的工作内存空间，然后对变量进行操作，操作完成后再将变量写回主内存，不能直接操作主内存中的变量，各个线程中的工作内存中存储着主内存中的变量副本拷贝，因此不同的线程间无法访问对方的工作内存，线程间的通信(传值)必须通过主内存来完成，其简要访问过程如下图:

![image-20220313213808751](D:\JavaProject\面试题总结Images\image-20220313213808751.png)

### 线程安全三大特性

- 可见性
- 原子性
- VolatileDemo代码演示可见性和原子性
- 有序性

```java
package com.haust.volatileTest;

import java.util.concurrent.TimeUnit;

public class VOlatileTest {
    public static void main(String[] args) {
        MyData myData = new MyData();
        new Thread(()->{
            System.out.println(Thread.currentThread().getName() + "---come in");
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            myData.addTo60();
            System.out.println(Thread.currentThread().getName() + "\t update number value"+ myData.number);
        },"AAA").start();

        while(myData.number == 0){

        }
        System.out.println(Thread.currentThread().getName() + "\t mission is over , main get number" + myData.number);
    }
}

class MyData{
    int number = 0;
    public void addTo60(){
        this.number = 60;
    }
}

```

![image-20220313221154551](D:\JavaProject\面试题总结Images\image-20220313221154551.png)

修改代码

```java
package com.haust.volatileTest;

import java.util.concurrent.TimeUnit;

public class VOlatileTest {
    public static void main(String[] args) {
        MyData myData = new MyData();
        new Thread(()->{
            System.out.println(Thread.currentThread().getName() + "---come in");
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            myData.addTo60();
            System.out.println(Thread.currentThread().getName() + "\t update number value"+ myData.number);
        },"AAA").start();

        while(myData.number == 0){

        }
        System.out.println(Thread.currentThread().getName() + "\t mission is over , main get number" + myData.number);
    }
}

class MyData{
    volatile int number = 0;
    public void addTo60(){
        this.number = 60;
    }
}

```

![image-20220313221304459](D:\JavaProject\面试题总结Images\image-20220313221304459.png)

- 验证volatile的可见性
  - 1.1假如 int number = 0; ， number变量之前根本没有添加oLatile关键字修饰,没有可见性
  - 1.2添加volatile，可以解决可见性问题。
- 验证volatile不保证原子性
  - 原子性指的是什么意思?
  - 不可分割，完整性，也即某个线程正在做某个具体业务时，中间不可以被加塞或者被分割。需要整体完整要么同时成功，要么同时失败。

```java
public class VOlatileTest {
    public static void main(String[] args) {
        MyData data = new MyData();
        for (int i = 1 ; i <= 20; i++ ){
            new Thread(()->{
                for(int j = 1 ; j <= 1000; j++){
                    data.addPlusPlus();
                }
            },String.valueOf(i)).start();

        }
        //需要等待上面的20个线程都全部计算完成后，再用main线程最大的结果值
        while(Thread.activeCount() > 2){
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + "\t finally number value" + data.number);

    }
}
class MyData{
    volatile int number = 0;
    public void addTo60(){
        this.number = 60;
    }
    public void addPlusPlus(){
        this.number ++;
    }
}

```

![image-20220314160802401](D:\JavaProject\面试题总结Images\image-20220314160802401.png)

### 解析i++的工作原理

![image-20220314164725582](D:\JavaProject\面试题总结Images\image-20220314164725582.png)

首先加载一个int类型的数据0，之后开辟一份存储空间，拿到属性值，定义一个常量值1，iadd是一个操作数，操作栈顶数据和下一个数据加和，将栈顶元素放回到主空间中。结束运行。

### 如何解决原子性？

- 加sync
- 使用AtomicInteger

```java
package com.haust.volatileTest;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class VOlatileTest {
    public static void main(String[] args) {
//        VolatileToSee();

        MyData data = new MyData();
        for (int i = 1 ; i <= 20; i++ ){
            new Thread(()->{
                for(int j = 1 ; j <= 1000; j++){
                    data.addPlusPlus();
                    data.atomicPlusPlus();
                }
            },String.valueOf(i)).start();

        }
        //需要等待上面的20个线程都全部计算完成后，再用main线程最大的结果值
        while(Thread.activeCount() > 2){
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + "\t finally number value" + data.number);
        System.out.println(Thread.currentThread().getName() + "\t finally number value" + data.atomicInteger);

    }

    /**
     * 保证可见性
     */
    private static void VolatileToSee() {
        MyData myData = new MyData();
        new Thread(()->{
            System.out.println(Thread.currentThread().getName() + "---come in");
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            myData.addTo60();
            System.out.println(Thread.currentThread().getName() + "\t update number value"+ myData.number);
        },"AAA").start();

        while(myData.number == 0){

        }
        System.out.println(Thread.currentThread().getName() + "\t mission is over , main get number" + myData.number);
    }
}

class MyData{
    volatile int number = 0;
    volatile AtomicInteger atomicInteger = new AtomicInteger(0);
    public void addTo60(){
        this.number = 60;
    }
    public void addPlusPlus(){
        this.number ++;
    }
    public void atomicPlusPlus(){
        this.atomicInteger.getAndDecrement();
    }
}

```

为什么Atomic下的包装类可以解决原子性问题？

因为使用CAS，自旋锁

### 指令重排案例：

计算机在执行程序时，为了提高性能，编译器和处理器的常常会对指令做重排，一般分一下三种。

计算机在执行程序时，为了提高性能，编译器和处理器的常常会对指令做重排，一般分以下3种
源代码---->>编译器优化的重排---->>>指令并行的重排----->>>内存系统的重排-=-->>>终执行的指令
单线程环境里面确保程序最终执行结果和代码顺序执行的结果一致。
处理器在进行重排序时必须要考虑指令之间的**数据依赖性**
多线程环境中线程交替执行,由于编译器优化重排的存在，两个线程中使用的变量能否保证一致性是无法确定的,结果无法预测

```java
public class ReSortSeqDemo {
    int a = 0;
    boolean flag = false;

    public void method01() {
        a = 1; //语句
        flag = true;//语句2
    }

    //多线程环境中线程交替执行,由于编译器优化重排的存在，
//    两个线程中使用的变量能否保证一致性是无法确定的,结果无法预测public void method02()
    {
        if (flag) {
            a = a + 5; //语句3
            System.out.println("*****retValue: " + a);
        }
    }
}

```

![image-20220314183925182](D:\JavaProject\面试题总结Images\image-20220314183925182.png)

volatile实现禁止指令重排优化，从而避免多线程环境下程序出现乱序执行的现象
先了解一个概念，内存屏障(Memory Barrier）又称内存栅栏，是一个CPU指令，它的作用有两个:
一是保证特定操作的执行顺序，
二是保证某些变量的内存可见性（利用该特性实现volatile的内存可见性)。
由于编译器和处理器都能执行指令重排优化。如果在指令间插入一条Memory Barrier则会告诉编译器和CPU，不管什么指令都不能和这条Memory Barrier指令重排序，也就是说**通过插入内存屏障禁止在内存屏障前后的指令执行重排序优化**。内存屏障另外一个作用是强制刷出各种CPU的缓存数据，因此任何CPU上的线程都能读取到这些数据的最新版本。

![image-20220314180839094](D:\JavaProject\面试题总结Images\image-20220314180839094.png)



工作内存与主内存同步延迟现象导致的可见性问题
可以使用synchronized或volatile关键字解决，它们都可以使一个线程修改后的变量立即对其他线程可见。
对于指令重排导致的可见性问题和有序性问题
可以利用volatile关键字解决，因为volatile的另外一个作用就是禁止重排序优化。

### 在哪里用到了volatile关键字？

#### 单例模式DCL

双端检索模式

```java
// DCL (DooubLe check Lock双端检锁机制)
public static singletonDemo getInstance(){
    if(instance == null){
        synchronized (singletonDemo.class){
            if(instance == null){
                instance = new singletonDemo();
            }
        }
    }
    return instance;
}

```

DCL(双端检锁）机制不一定线程安全，原因是有指令重排序的存在，加入volatile可以禁止指令重排
原因在于某一个线程执行到第一次检测，读取到的instance不为null时，instance的引用对象可能没有完成初始化。instance = new SingletonDemo();可以分为以下3步完成(伪代码)
memory = allocate(); //1.分配对象内存空间
instance(memory);//2.初始化对象
instance = memory;//3.设置instance指向刚分配的内存地址，此时instance! =null
步骤2和步骤3不存在数据依赖关系，而且无论重排前还是重排后程序的执行结果在单线程中并没有改变，因此这种重排优化是允许的。
memory = allocate(); //1.分配对象内存空间
instance = memory;//3.设置instance指向刚分配的内存地址，此时instance! =null，但是对象还没有初始化完成!instance(memory);//2.初始化对象

但是指令重排只会保证串行语义的执行的一致性(单线程)，但并不会关心多线程间的语义一致性。
所以当一条线程访问instance不为null时，由于instance实例未必已初始化完成，也就造成了线程安全问题。



#### 单例模式volatile

```java
private static volatile singletonDemo = null; 
// DCL (DooubLe check Lock双端检锁机制)
public static singletonDemo getInstance(){
    if(instance == null){
        synchronized (singletonDemo.class){
            if(instance == null){
                instance = new singletonDemo();
            }
        }
    }
    return instance;
}
```



## CAS你知道吗?

CAS的执行需要unsafe类，而unsafe类的底层是使用C语言的指针直接操作内存。

Unsafe
是CAS的核心类，由于Java方法无法直接访问底层系统，需要通过本地（native)方法来访问，Unsafe相当于一个后门，基于该类可以直接操作特定内存的数据。Unsafe类存在于sun.misc包中，其内部方法操作可以像C的指针一样直接操作内存，因为Java中CAS操作的执行依赖于Unsafe类的方法。
注意Unsafe类中的所有方法都是native修饰的，也就是说Unsafe类中的方法都直接调川操作系统底层系统资源来执行相应任务

2变量valueOffset，表示该变量值在内存中的偏移地址，因为Unsafe就是根据内存偏移地址获取数据的。

```java
/**

* Atomically increments by one the current value.*
  *@return the previous value*/
  public final int getAndIncrement() {
  return unsafe.getAndAddInt( o: this，valueoffset，i: 1);
  }
```

3变量value用volatile修饰，保证了多线程之间的内存可见性。

### compareAndSet\compareAndSwap自旋锁

CAS的全称为Compare-And-Swap，它是一条CPU并发原语。
它的功能是判断内存某个位置的值是否为预期值，如果是则更改为新的值，这个过程是原子的。
CAS并发原语体现在JAVA语言中就是sun.misc.Unsafe类中的各个方法。调用UnSafe类中的CAS方法，JVM会帮我们实现出CAS汇编指令。这是一种完全依赖于硬件的功能，通过它实现了原子操作。再次强调，由于CAS是一种系统原语，原语属于操作系统用语范畴，是由若干条指令组成的，用于完成某个功能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说CAS是一条CPU的原子指令，不会造成所谓的数据不一致问题。

![image-20220315180152574](D:\JavaProject\面试题总结Images\image-20220315180152574.png)

![image-20220315215801455](D:\JavaProject\面试题总结Images\image-20220315215801455.png)

var1 AtomicInteger对象本身。var2该对象值得引用地址。var4需要变动的数量。
var5是用过var1 var2找出的主内存中真实的值。用该对象当前的值与var5比较:
如果相同，更新var5+var4并且返回true,
如果不同，继续取值然后再比较，直到更新完成。

假设线程A和线程B两个线程同时执行getAndAddInt操作（分别跑在不同CPU上) :
1 AtomicInteger里面的value原始值为3，即主内存中AtomicInteger的value为3，根据JMM模型，线程A和线程B各自持有份值为3的value的副本分别到各自的工作内存。
2线程A通过getIntVolatile(var1, var2)拿到value值3，这时线程A被挂起。
3线程B也通过getintVolatile(var1, var2)方法获取到value值3，此时刚好线程B没有被挂起并执行compareAndSwapInt方法比较内存值也为3，成功修改内存值为4，线程B打完收工，一切OK。
4这时线程A恢复，执行compareAndSwaplnt方法比较，发现自己手里的值数字3和主内存的值数字4不一致，说明该值己经被其它线程抢先一步修改过了，那A线程本次修改失败，只能重新读取重新来一遍了。
5线程A重新获取value值，因为变量value被volatle修饰，所以其它线程对它的修改，线程A总是能够看到，线程A继续执行compareAndSwaplInt进行比较替换，直到成功。

底层原语：

**Unsafe类中的compareAndSwapInt，是一个本地方法，该方法的实现位于unsafe.cpp中**
**UNSAFE_ENTRY(jboolean, Unsafe_CompareAndSwapInt(JNlEnv \*env, jobject unsafe, jobject obj, jlong offset, jint  e, int x))**

**UnsafeWrapper("Unsafe_CompareAndSwapInt");**
**oop p =JNlHandles::resolve(obj);**
**jint* addr = (jint *) index_oop_from_field_offset_long(p, offset);**

**return (jint)(Atomic:cmpxchg(x, addr, e))==e;**
**UNSAFE_END**
//先想办法拿到变量value在内存中的地址。
//通过Atomic::cmpxchg实现比较替换，其中参数x是即将更新的值，参数e是原内存的值。

CAS (CompareAndSwap)
比较当前工作内存中的值和主内存中的值，如果相同则执行规定操作，否则继续比较直到主内存和工作内存中的值一致为止.
CAS应用
CAS有3个操作数，内存值V，旧的预期值A，要修改的更新值B。
当且仅当预期值A和内存值V相同时，将内存值V修改为B，否则什么都不做。

## CAS的缺点？

1. 循环时间长，开销很大
2. 只能保证一个共享变量的原子操作
3. 引出来的ABA问题

当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，
但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁来保证原子性。

## 原子类Atomiclnteger的ABA问题谈谈?原子更新引用知道吗?

CAS会导致“ABA问题”。
CAS算法实现一个重要前提需要取出内存中某时刻的数据并在当下时刻比较并替换，那么在这个时间差类会导致数据的变化。比如说一个线程one从内存位置V中取出A，这时候另一个线程two也从内存中取出A，并且线程two进行了一些操作将值变成了B,然后线程two又将V位置的数据变成A，这时候线程one进行CAS操作发现内存中仍然是A，然后线程one操作成功。
**尽管线程one的CAS操作成功，但是不代表这个过程就是没有问题的。**

### 使用原子引用

**AtomicReference<User>** 使用原子引用来解决ABA问题

版本号原子引用**AtomicStampedReference**

## 我们知道ArrayList是线程不安全，请编码写一个不安全的案例并给出解决方案。

```java
public class ContainerNotSafeDemo{
    public static void main(String[] args){
        List<String> list = new ArrayList<>();
        for(int i = 0; i<30;i++){
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,8));
                System.out.println(list);
            },String.valueOf(i)).start();
        }
    }
}
//java.util.ConcurrentModificationException
```

1. 使用vector
2. 使用collections.synchronizeArrayList
3. 使用copyonwriteArrayList

写时复制
Copyonwrite容器即写时复制的容器。往一个容器添加元素的时候，不直接往当前容器object[]添加，而是先将当前容器object[]进行copy.复制出一个新的容器object[ ] newELements，然后新的容器object[ ] newElements里添加元素，添加完元素之后，
再将原容器的引用指向新的容器setArray(newELements);。这样做的好处是可以对Copyonwrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以Copyonwrite容器也是一种读写分离的思想，读和写不同的容器

**HashSet底层数据结构是HashMap，value值是present的object对象**

## 公平锁/非公平锁/可重入锁/递归锁/自旋锁谈谈你的理解?请手写一个自旋锁

公平锁和非公平锁？

公平锁 

- 是指多个线程按照申请锁的顺序来获取锁，类似排队打饭，先来后到。

非公平锁

- 是指多个线程获取锁的顺序并不是按照中请锁的顺序，有可能后中请的线程比先申请的线程优先获取锁
- 在高并发的情况下，有可能会造成优先级反转或者饥饿现象

### 公平锁/非公平锁

并发包中ReentrantLock的创建可以指定构造函数的boolean类型来得到公平锁或非公平锁，默认是非公平锁
关于两者区别:
公平锁:Threadsacquire a fair lock in the order in which they requested it
公平锁，就是很公平，在并发环境中，每个线程在获取锁时会先查看此锁维护的等待队列，如果为空，或者当前线程是等待队列的第一个，就占有锁，否则就会加入到等待队列中，以后会按照FIFO的规则从队列中取到自己
非公平锁: a nonfair lock permits barging: threads requesting a lock can jump ahead of the queue of waiting threads if the lockhappens to be available when it is requested.
非公平锁比较粗鲁，上来就直接尝试占有锁，如果尝试失败，就再采用类似公平锁那种方式。

Java ReentrantLock而言，
通过构造函数指定该锁是否是公平锁，默认是**非公平锁**。非公平锁的优点在于吞吐量比公平锁大。
对于Synchronized而言，也是一种非公平锁

### 可重入锁(也叫做递归锁)

指的是同一线程外层函数获得锁之后﹐内层递归函数仍然能获取该锁的代码，在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁
也即是说，**线程可以进入任何一个它已经拥有的锁所同步着的代码块**。

- ReentrantLock/Synchronized就是一个典型的可重入锁
- 可重入锁最大的作用是避兔死锁

```java
public synchronized void sendsMs ( ) throws Exception
{
    System.out.println( Thread.currentThread( ).getName()+"\t invoked sendSNS()");
    sendEmail();
}
public synchronized void sendEmail() throws Exception{
 	system.out.println(Thread.currentThread().getName()+"\t######## invoked sendEmail()");   
}

```

### 自旋锁

自旋锁(spinlock)
是指尝试获取锁的线程不会立即阻塞，而是**采用循环的方式去尝试获取锁**，这样的好处是减少线程上下文切换的消耗，缺点是循环会消耗CPU

题目:实现一个自旋锁
自旋锁好处:循环比较获取直到成功为止，没有类似wait的阻塞。
通过CAs操作完成自旋锁，A线程先进来调用yLock方法自己持有锁5秒钟，B随后进来后发现当前有线程持有锁，不是null，所以只能通过自旋等待，直到A释放锁郈B随后抢到。

```java
//原子引用线程
AtomicReference<Thread> atomicReference = new AtomicReference<>();
public void myLock(){
    Thread thread = Thread.currentThread();
    System.out.println( Thread.currentThread().getName()+"\t come in o(n_n)o");
    while(!atomicReference.compareAndset(null,thread)){

    }
}

public void myUnlock(){
    Thread thread = Thread.currentThread();
    atomicReference.compareAndset(thread,null);
    System.out.println(Thread.currentThread( ).getName()+"\t invoked myUnLock()");
}
```

### 独占锁/共享锁

独占锁:指该锁一次只能被一个线程所持有。对ReentrantLock和Synchronized而言都是独占锁
共享锁:指该锁可被多个线程所持有。
对**ReentrantReadWriteLock**其读锁是共享锁，其写锁是独占锁。
读锁的共享锁可保证并发读是非常高效的，读写，写读，写写的过程是互斥的。

未加锁：

```java
package com.haust.lock;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReentrantReadWriteLockDemo {

    public static void main(String[] args) {
        MyCache myCache = new MyCache();

        for(int i = 0; i < 5; i++) {
            final int tempInt = i;
            new Thread(()->{
                myCache.put(tempInt+"", tempInt+"");
            },String.valueOf(i)).start();
        }

        System.out.println();
        System.out.println();
        System.out.println();
        System.out.println();
        
        for(int i = 0; i < 5; i++) {
            final int tempInt = i;
            new Thread(()->{
                myCache.get(tempInt+ "");
            },String.valueOf(i)).start();
        }
    }
}

class MyCache{
    private volatile Map<String, Object> map= new HashMap<>();
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    public void put(String key, Object value){
        System.out.println(Thread.currentThread().getName() + "正在添加值" + key);
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "添加值完成" + key);
    }

    public Object get(String key){
        System.out.println(Thread.currentThread().getName() + "正在读取值" + key);
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName() + "读取值完成" + key);
        return o;
    }

}





3正在添加值3
4正在添加值4
1正在添加值1
2正在添加值2
0正在添加值0
0正在读取值0
1正在读取值1
3正在读取值3
4正在读取值4
2正在读取值2
2添加值完成2
0添加值完成0
1添加值完成1
3添加值完成3
4添加值完成4
0读取值完成0
1读取值完成1
2读取值完成2
4读取值完成4
3读取值完成3

Process finished with exit code 0
```

加锁之后：

```java
package com.haust.lock;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReentrantReadWriteLockDemo {

    public static void main(String[] args) {
        MyCache myCache = new MyCache();

        for(int i = 0; i < 5; i++) {
            final int tempInt = i;
            new Thread(()->{
                myCache.put(tempInt+"", tempInt+"");
            },String.valueOf(i)).start();
        }

        System.out.println();
        System.out.println();
        System.out.println();
        System.out.println();

        for(int i = 0; i < 5; i++) {
            final int tempInt = i;
            new Thread(()->{
                myCache.get(tempInt+ "");
            },String.valueOf(i)).start();
        }
    }
}

class MyCache{
    private volatile Map<String, Object> map= new HashMap<>();
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    public void put(String key, Object value){
        readWriteLock.writeLock().lock();
        System.out.println(Thread.currentThread().getName() + "正在添加值" + key);
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "添加值完成" + key);
        readWriteLock.writeLock().unlock();
    }

    public Object get(String key){
        readWriteLock.readLock().lock();
        System.out.println(Thread.currentThread().getName() + "正在读取值" + key);
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName() + "读取值完成" + key);
        readWriteLock.readLock().unlock();
        return o;
    }

}
0正在添加值0
0添加值完成0
1正在添加值1
1添加值完成1
4正在添加值4
4添加值完成4
3正在添加值3
3添加值完成3
2正在添加值2
2添加值完成2
0正在读取值0
1正在读取值1
2正在读取值2
3正在读取值3
4正在读取值4
4读取值完成4
1读取值完成1
3读取值完成3
2读取值完成2
0读取值完成0

Process finished with exit code 0

```

## CountDownLatch/CyclicBarrier/Semaphore使用过吗?

### CountDownLatch:

```java
package com.haust.lock;

import java.util.concurrent.CountDownLatch;

public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(6);

        for(int i = 0; i < 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName() +"\t 上完晚自习，离开了教室");
                countDownLatch.countDown();
            },String.valueOf(i)).start();
        }
        countDownLatch.await();
        System.out.println(Thread.currentThread().getName() + "\t 班长离开了教室并锁上了门" );
    }
}

0	 上完晚自习，离开了教室
2	 上完晚自习，离开了教室
3	 上完晚自习，离开了教室
4	 上完晚自习，离开了教室
1	 上完晚自习，离开了教室
5	 上完晚自习，离开了教室
main	 班长离开了教室并锁上了门
```

让一些线程阻塞直到另一些线程完成一系列操作后才被唤醒
CountDownLatch主要有两个方法，当一个或多个线程调用await方法时，调用线程会被阻塞。其它线程调用countDown方法会将计数器减1(调用countDown方法的线程不会阻塞)，
当计数器的值变为零时，因调用await方法被阻塞的线程会被唤醒，继续执行。

### CyclicBarrier:

CyclicBarrier的字面意思是可循环（Cyclic）使用的屏障（ Barrier）。它要做的事情是，
让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时
屏障才会开门，所有被屏障拦截的线程才会继续干活，线程进入屏障通过CyclicBarrier的await()方法。

```java
package com.haust.lock;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierDemo {

    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(7,()->{
            System.out.println("人员到齐，开始开会");
        });
        for(int i = 0; i < 7; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"\t 已经到了，准备开会");
                try {
                    barrier.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            },String.valueOf(i)).start();
        }
    }
}

0	 已经到了，准备开会
5	 已经到了，准备开会
1	 已经到了，准备开会
3	 已经到了，准备开会
4	 已经到了，准备开会
2	 已经到了，准备开会
6	 已经到了，准备开会
人员到齐，开始开会

```

### Semaphore:

信号量主要用于两个目的，一个是用于多个共享资源的互斥使用，另一个用于并发线程数的控制。

```java
public static void main(String[] args) {
        Semaphore semaphore = new Semaphore(3);

        for(int i = 0; i < 6; i++) {
            new Thread(()->{
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"\t 抢到了车位");
                    try {   TimeUnit.SECONDS.sleep(1);} catch (InterruptedException e) {    e.printStackTrace();}
                    System.out.println(Thread.currentThread().getName()+"\t 释放了车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
            },String.valueOf(i)).start();
        }

    }

0	 抢到了车位
2	 抢到了车位
1	 抢到了车位
0	 释放了车位
2	 释放了车位
1	 释放了车位
3	 抢到了车位
4	 抢到了车位
5	 抢到了车位
3	 释放了车位
5	 释放了车位
4	 释放了车位

```

## 阻塞队列知道吗?

- ArrayBlockingQueue:是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序
- LinkedBLockingueue:一个基于链表结构的阻塞队列，此队列按IFO（先进先出）排序元素，吞吐量通常要高于ArrayBLockingqueue 。
- synchronousQueue:一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高

阻塞队列，顾名思义，首先它是一个队列，而一个阻塞队列在数据结构中所起的作用大致如下图所

![image-20220317161533271](D:\JavaProject\面试题总结Images\image-20220317161533271.png)

- 当阻塞队列是空时，从队列中获取元素的操作将会被阻塞I当阻塞队列是满时，往队列里添加元素的操作将会被阻塞。
- 试图从空的阻塞队列中获取元素的线程将会被阻塞，直到其他的线程往空的队列插入新的元素。同样
- 试图往已满的阻塞队列中添加新元素的线程同样也会被阻塞，直到其他的线程从列中移除一个或者多个元素或者完全清空队列后使队列重新变得空闲起来并后续新增

在多线程领域:所谓阻塞，在某些情况下会挂起线程（即阻塞），一旦条件满足，被挂起的线程又会自动被唤醒

### 为什么需要BlockingQueue

好处是我们不需要关心什么时候需要阻塞线程，什么时候需要唤醒线程，因为这一切BlockingQueue都给你一手包办了
在concurrent包发布以前，在多线程环境下，**我们每个程序员都必须去自己控制这些细节，尤其还要兼顾效率和线程安全**，而这会给我们的程序带来不小的复杂度。

- **ArrayBlockingQueue :由数组结构组成的有界阻塞队列。**
- **LinkedBlockingQueue:由链表结构组成的有界(但大小默认值为Integer.MAX_VALUE)阻塞队列**
- PriorityBlockingQueue :支持优先级排序的无界阻塞队列。
- DelayQueue:使用优先级队列实现的延迟无界阻塞队列。
- **SynchronousQueue:不存储元素的阻塞队列，也即单个元素的队列**。
- LinkedTransferQueue:由链表结构组成的无界阻塞队列。
- LinkedBlockingDeque:由链表结构组成的双向阻塞队列

| 方法类型 | 抛出异常  | 特殊值   | 阻塞   | 超时               |
| -------- | --------- | -------- | ------ | ------------------ |
| 插入     | add(e)    | offer(e) | put(e) | offer(e,time,unit) |
| 移除     | remove(e) | poll()   | take() | poll(time,unit)    |
| 检查     | element() | peek()   | 不可用 | 不可用             |

| 抛出异常 | 当阻塞队列满时，再往队列里add插入元素会抛IllegalStateException: Queue full当阻塞队列空时，再往队列里remove移除元素会抛NoSuchElementException |
| -------- | ------------------------------------------------------------ |
| 特殊值   | 插入方法，成功ture失败false<br/>移除方法，成功返回出队列的元素，队列里面没有就返回null |
| 一直阻塞 | 当阻塞队列满时，生产者线程继续往队列里put元素，队列会一直阻塞生产线程直到put数据or响应中断退出。当阻塞队列空时，消费者线程试图从队列里take元素，队列会一直阻塞消费者线程直到队列可用。 |
| 超时退出 | 当阻塞队列满时，队列会阻塞生产者线程一定时间，超过后限时后生产者线程会退出 |

SynchronousQueue没有容量。
与其他BlockingQueue不同，SynchronousQueue是一个不存储元素的BlockingQuege。
每一个put操作必须要等待一个take操作，否则不能继续添加元素，反之亦然。

### 生产者消费者模式

#### 传统版

ProdConsumer_TraditionDemo

```java
package com.haust.queue;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BlockingQueueDemo {

    public static void main(String[] args) {
        ShareData data = new ShareData();
        for(int i = 0; i < 5; i++) {
            new Thread(()->{

                try {
                    data.increment();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            },String.valueOf(i)).start();
        }

        for(int i = 0; i < 5; i++) {
            new Thread(()->{

                try {
                    data.decrement();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            },String.valueOf(i)).start();
        }
    }

}

class ShareData{
    private int number = 0;
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void increment() throws Exception{
        lock.lock();
        try {
            //判断
            while(number != 0){
                condition.await();
            }
            number ++;
            System.out.println("生产者开始消费了" + number);
            //通知唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    public void decrement() throws Exception{
        lock.lock();
        try {
            //判断
            while(number == 0){
                condition.await();
            }
            number --;
            System.out.println("消费者开始生产了" + number);
            //通知唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}

```



```text
生产者开始消费了1
消费者开始生产了0
生产者开始消费了1
消费者开始生产了0
生产者开始消费了1
消费者开始生产了0
生产者开始消费了1
消费者开始生产了0
生产者开始消费了1
消费者开始生产了0
```

##### synchronize和lock有什么区别，用新的lock有什么好处？

1原始构成
synchronized是关键字属于JVM层面,
monitonenter(底层是通过monitor对象来完成，其实wait/notify等方法也依赖于monitor对象只有在同步块或方法中才能海wait/notify等方法monitorexit
Lock是具体类(java.util.concurrent.locks.Lock)是api层面的锁
2使用方法
synchronized 不需耍用户去手动释放锁，当synchronized代码执行完后系统会自动让线程释放对锁的占用ReentrantLock则需要用户去手动释放锁若没有主动释放锁，就有可能导致出现死锁现象。
需要Lock( )利lunLock()方法配合try/finally语句块来完成。
3等待是否可中断
synchronized不可中断，除非抛出异常或者正常运行完成
ReentrantLock可中断，1.设置超时方法 tryLock(Long timeout，Timeunit unit)
2.LockInterruptibly()放代码块中，调用interrupt()方法可中断
4加锁是否公平
synchronized非公平锁
ReentrantLock两者都可以，默认公平锁，构造方法可以传入boolean值，true为公平锁，false为非公平锁
5锁绑定多个条件condition
synchronized没有
ReentrantLock用来实现分组唤醒需要唤醒的线程们，可以精确唤醒，而不是像synchronized要么随机唤醒一个线程要么唤醒全部线程。



#### 阻塞队列版

ProdConsumer_BlockQueueDemo

```java
package com.haust.queue;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class ProdConsumer_BlockQueueDemo {
    public static void main(String[] args) {
        MyReource reource = new MyReource(new ArrayBlockingQueue<String>(5));
        new Thread(()->{
            try {
                reource.myProd();
            } catch (Exception e) {
                e.printStackTrace();
            }
        },"prod").start();
        new Thread(()->{
            try {
                reource.myConsumer();
            } catch (Exception e) {
                e.printStackTrace();
            }
        },"customer").start();

        try {   TimeUnit.SECONDS.sleep(5);} catch (InterruptedException e) {    e.printStackTrace();}
        reource.updateFlag();
    }
}

class MyReource{
    private volatile boolean FLAG = true;
    private AtomicInteger atomicInteger = new AtomicInteger();
    private BlockingQueue<String > blockingQueue = null;

    public MyReource(BlockingQueue blockingQueue){
        this.blockingQueue = blockingQueue;
        System.out.println(blockingQueue.getClass().getName());
    }

    public void myProd() throws Exception{
        String data = null;
        boolean retValue;
        while(FLAG){
            data = atomicInteger.incrementAndGet() + "";
            retValue = blockingQueue.offer(data, 2, TimeUnit.SECONDS);
            if(retValue){
                System.out.println(Thread.currentThread().getName() + "生产成功"+ data);
            }else{
                System.out.println(Thread.currentThread().getName() +"生产失败");
            }
            try {   TimeUnit.SECONDS.sleep(1);} catch (InterruptedException e) {    e.printStackTrace();}
        }
        System.out.println("大老板叫停了，生产者停止生产");
    }

    public void myConsumer() throws Exception{
        String result = null;
        while (FLAG){
            result = blockingQueue.poll(2, TimeUnit.SECONDS);
            if(result == null || result.equals("")){
                FLAG = false;
                System.out.println(Thread.currentThread().getName() +"没有东西可以消费了");
                return;
            }else{
                System.out.println(Thread.currentThread().getName() +"消费的东西是"+ result);
            }
        }
    }

    public void updateFlag(){
        this.FLAG = false;
    }

}


prod生产成功1
customer消费的东西是1
prod生产成功2
customer消费的东西是2
prod生产成功3
customer消费的东西是3
customer消费的东西是4
prod生产成功4
prod生产成功5
customer消费的东西是5
大老板叫停了，生产者停止生产
customer没有东西可以消费了


```

## 线程池用过吗? ThreadPoolExecutor谈谈你的理解?

### 线程池

#### callable<V>的使用：

```java
package com.haust.threa;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class CallableDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<Integer> futureTask = new FutureTask(new MyCallable());
        new Thread(futureTask,"aa").start();

        int resultMain = 10;
        int futureTaskResult = futureTask.get();
        System.out.println("最终的结果是"+(resultMain +futureTaskResult));
    }
}

class MyCallable implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        return 1024;
    }
}

```

FutureTask相当于一个适配器，可以使得callable的线程结果转化到runnable中

为什么出现了Runnable之后还要有Callable呢？

因为有时候使用多条线程同时执行的时候，需要获得他们的结果。而且需要同时的到他们的结果是不是都成功的返回了正确的值。

#### 线程池的使用和优势

**线程池做的工作主要是控制运行的线程的数量，处理过程中将任务放入队列，然后在线程创建后启动这些任务，如过线程数量超出了最大数量超出数量的线程排队等候，等其它线程执行完毕，再从队列中取出任务来执行。**
他的主要特点为:**线程复用;控制最大并发数;管理线程。**
第一:降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。第二:提高响应速度。当任务到达时，任务可以不需要的等到线程创建就能立即执行。第三:提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源
还会降低系统稳定性，使用线程池可以进行统一的分配，调优和监控

Java中的线程池是通过Executor框架实现的，该框架中用到了Executor，Executors,ExecutorService，ThreadPoolExecutor这几个类。

![image-20220317221032086](D:\JavaProject\面试题总结Images\image-20220317221032086.png)

```java
Executors.newSingleThreadExecutor() //创建一个单线程的线程池
Executors.newFixedThreadPool()//创建一个固定线程的线程池
Executors.newCachedThreadPool()//创建一个可以扩容的线程池
```

主要特点如下:
1创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
2 newFixedThreadPool创建的线程池corePoolSize和maximumPoolSize值是相等的，它使用的**LinkedBlockingQueue**;

主要特点如下:
1创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序执行。2 newSingleThreadExecutor将corePoolSize和maximumPoolSize都设置为1，它使用的**LinkedBlockingQueue**

主要特点如下:
1创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。2 newCachedThreadPool将corePoolSize设置为O，将maximumPoolSize设置为Integer.MAX_VALUE，使用的**SynchronousQueue**，也就是说来了任务就创建线程运行，当线程空闲超过60秒，就销毁线程

- 执行长期的任务，性能好
- 很多一个任务一个任务执行的场景
- 适用:执行很多短期异步的小程序或者负载较轻的服务器

#### 线程池的几个重要参数介绍?

- 1.corePoolSize:线程池中的常驻核心线程数
  - 在创建了线程池后，当有请求任务来之后，就会安排池中的线程去执行请求任务，近似理解为今日当值线程，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中;
- 2.maximumPoolSize:线程池能够容纳同时执行的最大线程数，此值必须大于等于1
- 3.keepAliveTime:多余的空闲线程的存活时间。当前线程池数量超过corePoolSize时，当空闲时间达到keepAliveTime值时多余空闲线程会被销毁直到只剩下corePoolSize个线程为止
- 4.unit: keepAliveTime的单位。
- 5.workQueue:任务队列，被提交但尚未被执行的任务。
- 6.threadFactory:表示生成线程池中工作线程的线程工厂，用于创建线程一般用默认的即可。
- 7.handler:拒绝策略，表示当队列满了并且工作线程大于等于线程池的最大线程数（ maximumPoolSize）采用什么样的拒绝任务的策略

#### 说说线程池的底层工作原理?

![image-20220317230757261](D:\JavaProject\面试题总结Images\image-20220317230757261.png)

- 1．在创建了线程池后，等待提交过来的任务请求。
- 2.当调用execute()方法添加一个请求任务时，线程池会做如下判断:
  - 2.1如果正在运行的线程数量小于corePoolSize，那么马上创建线程运行这个任务;
  - 2.2如果正在运行的线程数量大于或等于corePoolSize，那么将这个任务放入队列;
  - 2.3 如果这时候队列满了且正在运行的线程数量还小于maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务;
  - 2.4如果队列满了且正在运行的线程数量大于或等于maximumPoolSize，那么线程池会启动饱和拒绝策略来执行。
- 3.当一个线程完成任务时，它会从队列中取下一个任务来执行。
- 4．当一个线程无事可做超过一定的时间（keepAliveTime）时，线程池会判断:
  如果当前运行的线程数大于corePoolSize，那么这个线程就被停掉。

![image-20220317231304256](D:\JavaProject\面试题总结Images\image-20220317231304256.png)

#### 拒绝策略是什么？

等待队列也已经排满了，再也塞不下新任务了同时，
线程池中的max线程也达到了，无法继续为新任务服务。
这时候我们就需要拒绝策略机制合理的处理这个问题。

- AbortPolicy(默认):直接抛出RejectedExecutionException异常阻止系统正常运行。
- CallerRunsPolicy:"调用者运行"一种调节机制，该策略既不会抛弃任务，也不会抛出异常，而是将某些任务回退到调用者，从而降低新任务的流量。
- DiscardOldestPolicy:抛弃队列中等待最久的任务，然后把当前任务加入队列中尝试再次提交当前任务。
- DiscardPolicy:直接丢弃任务，不予任何处理也不抛出异常。如果允许任务丢失，这是最好的一种方案。

以上内置拒绝策略均实现了**RejectedExecutionHandler**接口

在工作中固定线程\单线程\可扩容的线程线程池，常用哪一个？

答案是一个都不用，我们生产上只能使用自定义的Executors 中JDK己经给你提供了，为什么不用?√

- 1）FixedThreadPool 和 SingleThreadPool:   允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。
- 2）CachedThreadPool 和 ScheduledThreadPool:   允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM

你在工作中是如何使用线程池的，是否自定义过线程池

```java
package com.haust.executor;

import java.util.concurrent.*;

public class CustomerExecutor {

    public static void main(String[] args) {
        ExecutorService threadPool = new ThreadPoolExecutor(
          2,
          5,
          1L,
          TimeUnit.SECONDS,
          new LinkedBlockingQueue<Runnable>(3),
            Executors.defaultThreadFactory(),
            new ThreadPoolExecutor.AbortPolicy()
        );
        try {
            for(int i = 0; i < 10; i++) {
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+"\t 正在办理业务");
                    try {   TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {    e.printStackTrace();}
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }

    }
}


pool-1-thread-1	 正在办理业务
pool-1-thread-3	 正在办理业务
pool-1-thread-5	 正在办理业务
pool-1-thread-4	 正在办理业务
pool-1-thread-2	 正在办理业务
java.util.concurrent.RejectedExecutionException: Task com.haust.executor.CustomerExecutor$$Lambda$1/1023892928@4dd8dc3 rejected from java.util.concurrent.ThreadPoolExecutor@6d03e736[Running, pool size = 5, active threads = 5, queued tasks = 3, completed tasks = 0]
	at java.util.concurrent.ThreadPoolExecutor$AbortPolicy.rejectedExecution(ThreadPoolExecutor.java:2063)
	at java.util.concurrent.ThreadPoolExecutor.reject(ThreadPoolExecutor.java:830)
	at java.util.concurrent.ThreadPoolExecutor.execute(ThreadPoolExecutor.java:1379)
	at com.haust.executor.CustomerExecutor.main(CustomerExecutor.java:19)
pool-1-thread-5	 正在办理业务
pool-1-thread-2	 正在办理业务
pool-1-thread-3	 正在办理业务

Process finished with exit code 0

```

### 线程池用过吗?生产上你如何设置合理参数

#### 如何合理的安排线程池的线程数？

##### CPU密集型

CPU密集的意思是该任务需要大量的运算，而没有阻塞，CPU一直全速运行。CPU密集任务只有在真正的多核CPU上才可能得到加速(通过多线程)，
而在单核CPU上(悲剧吧?（;’工 ') ')，无论你开几个模拟的多线程该任务都不可能得到加速，因为CPU总的运算能力就那些。
CPU密集型任务配置尽可能少的线程数量:一般公式:CPU核数+1个线程的线程池

##### IO密集型

- 由于IO密集型任务线程并不是T直在执行任务，则应配置尽可能多的线程，如CPU核数*2
- IO密集型，即该任务需要大量的IO，即大量的阻塞。
  在单线程上运行IO密集型的任务会导致浪费大量的CPU运算能力浪费在等待。
  所以在lO密集型任务中使用多线程可以大大的加速程序运行，即使在单核CPU上，这种加速主要就是利用了被浪费掉的阻寨时间。
  IO密集型时，大部分线程都阻塞，故需要多配置线程数:
  **参考公式:CPU核数/1-阻塞系数**
  阻塞系数在0.8~0.9之间
  比如8核CPU:8/1-0.9=80个线程数

## 死锁编码及定位分析

死锁是指两个或两个以上的进程在执行过程中,因争夺资源而造成的一种互相等待的现象,投无外力干涉那它们都将无法推进下去，如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则就会因争夺有限的资源而陷入死锁。

![image-20220318223900925](D:\JavaProject\面试题总结Images\image-20220318223900925.png)

- 系统资源不足
- 进程运行推进的顺序不合适
- 资源分配不当

```java
package com.haust.threa;

import java.util.concurrent.TimeUnit;

class Sharding implements Runnable{

    private String lockA;
    private String lockB;

    public Sharding(String lockA, String lockB){
        this.lockA = lockA;
        this.lockB = lockB;
    }
    @Override
    public void run() {
        synchronized (lockA){
            System.out.println(Thread.currentThread().getName() + "拿到了lockA" + lockA + "尝试获取lockB");
            synchronized (lockB){
                System.out.println(Thread.currentThread().getName() + "拿到了lockB" + lockA + "尝试获取lockA");
            }
        }

        synchronized (lockB){
            System.out.println(Thread.currentThread().getName() + "拿到了lockA" + lockA + "尝试获取lockB");
            synchronized (lockA){
                System.out.println(Thread.currentThread().getName() + "拿到了lockB" + lockA + "尝试获取lockA");
            }
        }
    }
}
public class DeadSyncDemo {
    public static void main(String[] args) {
        new Thread(new Sharding("lockA", "lockB"),"ThreadAAA").start();
        new Thread(new Sharding("lockA", "lockB"),"ThreadBBB").start();
        new Thread(new Sharding("lockA", "lockB"),"ThreadCCC").start();
        new Thread(new Sharding("lockA", "lockB"),"ThreadDDD").start();
    }
}

```

## JVM+GC解析

### JVM内存结构

![image-20220318231144799](D:\JavaProject\面试题总结Images\image-20220318231144799.png)

![image-20220318231242424](D:\JavaProject\面试题总结Images\image-20220318231242424.png)

### 垃圾回收的四大算法

- 标记清理
  - 垃圾收集算法——标记清除法(Mark-Sweep)
    算法分成标记和清除两个阶段，先标记出要回收的对象，然后统一回收这些对象。形如:
  - ![image-20220318231933637](D:\JavaProject\面试题总结Images\image-20220318231933637.png)
- 标记压缩
  - ![image-20220318231949678](D:\JavaProject\面试题总结Images\image-20220318231949678.png)
- 引用清理
  - ![image-20220318231708656](D:\JavaProject\面试题总结Images\image-20220318231708656.png)
- 复制清理 
  - ![image-20220318231747002](D:\JavaProject\面试题总结Images\image-20220318231747002.png)
  - 1:eden、Survivor From复制到SurvivorTo，年龄+1
    首先，当Eden区满的时候会触发第一次GC,把还活着的对象拷贝到SurvivorFrom区，当Eden区再次触发GC的时候会扫描Eden区和From区域,对这两个区域进行垃圾回收，经过这次回收后还存活的对象,则直接复制到To区域（如果有对象的年龄已经达到了老年的标准，则赋值到老年代区），同时把这些对象的年龄+1
  - 2:清空eden、SurvivorFrom
    然后，清空Eden和SurvivorFrom中的对象，也即复制之后有交换，谁空谁是to
  - 3: SurvivorTo和SurvivorFrom互换
    最后，Survivor To和SurvivorFrom互换，原SurvivorTo成为下一次GC时的SurvivorFrom区。部分对象会在From和To区域中复制来复制去,如此交换15次(由ⅣM参数MaxTenuringThreshold决定,这个参数默认是15),最终如果还是存活,就存入到老年代

## JVM垃圾回收的时候如何确定垃圾?是否知道什么是Gc Roots

### 什么是垃圾？

简单的说就是内存中已经不再被使用到的空间就是垃圾要进行垃圾回收

### 如何判断一个对象是否可以被回收?

为了解决引用计数法的循环引用问题，java使用了可达性分析的方法。

所谓的GC root或者说是tracing GC的根集合就是一组必须活跃的引用

基本思路就是通过一系列名为GC root的对象作为起始点，从这个被称为GC root的对象开始向下搜索，如果一个对象到GC roto没有任何引用链相连的时候，这说明此对象不可用。也即给定一个集合的引用作为根出发，通过引用关系遍历对象图，能被遍历到的对象就被判定为活；没有被遍历到的就自然被认定为为死亡。

![image-20220318232508941](D:\JavaProject\面试题总结Images\image-20220318232508941.png)

![image-20220318233336964](D:\JavaProject\面试题总结Images\image-20220318233336964.png)

### 那些可以作为GC root对象呢？

- 虚拟机栈（栈帧中的局部变量区，也叫做局部变量表）中引用的对象。
- 方法区中的类静态属性引用的对象。
- 方法区中常量引用的对象。
- 本地方法栈中JNI(Native方法)引用的对象。

## 你说你做过JVM调优和参数配置，请问如何盘点查看JVM系统默认值你平时工作用过的JVM常用基本配置参数有哪些?

### JVM参数类型

参数查找路径

![image-20220319000935203](D:\JavaProject\面试题总结Images\image-20220319000935203.png)

![image-20220319000957644](D:\JavaProject\面试题总结Images\image-20220319000957644.png)

https://docs.oracle.com/en/java/javase/17/docs/specs/man/java.html

- **标准参数**
  - -m or --module module[/mainclass]
  - -agentlib:libname[=options]
  - -agentlib:jdwp=transport=dt_socket,server=y,address=8000
  - -agentpath:pathname[=options]
  - --class-path classpath, -classpath classpath, or -cp classpath
  - --disable-@files
  - --enable-preview
  - --module-path modulepath... or -p modulepath
  - --upgrade-module-path modulepath...
  - --add-modules module[,module...]
  - --list-modules
  - -d module_name or --describe-module module_name
  - --dry-run
  - --validate-modules
  - -Dproperty=value
  - -disableassertions[:[packagename]...|:classname] or -da[[packagename]...|:classname]
  - -disablesystemassertions or -dsa
  - -enableassertions[:[packagename]...|:classname] or -ea[:[packagename]...|:classname]
  - -enablesystemassertions or -esa
  - -help, -h, or -?
  - --help
  - -javaagent:jarpath[=options]
  - --show-version
  - -showversion
  - --show-module-resolution
  - -splash:imagepath
  - -splash:images/splash.gif
  - -verbose:class
  - -verbose:gc
  - -verbose:jni
  - -verbose:module
  - --version
  - -version
  - --help-extra
  - --add-reads module=target-module(,target-module)*
  - --add-exports module/package=target-module(,target-module)*
  - --add-opens module/package=target-module(,target-module)*
  - --limit-modules module[,module...]
  - --patch-module module=file(;file)*
  - --source version
  - --illegal-access=parameter
- **-x参数**
  - -Xbatch
  - -Xbootclasspath/a:directories|zip|JAR-files
  - -Xcheck:jni
  - -Xdebug
  - -Xdiag
  - -Xint
  - -Xinternalversion
  - -Xlog:option
  - -Xmixed
  - -Xmn size
  - -Xmn256m
  - -Xmn262144k
  - -Xmn268435456
  - -Xms size
  - -Xms6291456
  - -Xms6144k
  - -Xms6m
  - -Xmx size
  - -Xmx83886080
  - -Xmx81920k
  - -Xmx80m
  - -Xnoclassgc
  - -Xrs
  - -Xshare:mode
  - -XshowSettings
  - -XshowSettings:category
  - -Xss size
  - -Xss1m
  - -Xss1024k
  - -Xss1048576
  - -XstartOnFirstThread
  - -Xdock:name=application_name
  - -Xdock:icon=path_to_icon_file
  - -Xfuture
  - -Xloggc:filename
  - -Xlog:gc:garbage-collection.log
  - -Xlog:codecache=Trace
  - -Xlog:codecache=Debug
  - -Xlog[:[what][:[output][:[decorators][:output-options[,...]]]]]
  - -Xlog:directive
  - -Xlog
  - -Xlog:help
  - -Xlog:disable
  - -Xlog[:option]
  - -Xlog:all=warning:stdout:uptime,level,tags
  - -Xlog Tags and Levels
  - -Xlog Output
  - -Xlog Output Mode
  - -Xlog:async
  - -Xlog Usage Examples
  - -Xlog
  - -Xlog:all=info:stdout:uptime,levels,tags
  - -Xlog:gc
  - -Xlog:gc,safepoint
  - -Xlog:gc+ref=debug
  - -Xlog:gc=debug:file=gc.txt:none
  - -Xlog:gc=trace:file=gctrace.txt:uptimemillis,pids:filecount=5,filesize=1024
  - -Xlog:gc::uptime,tid
  - -Xlog:gc*=info,safepoint*=off
  - -Xlog:disable -Xlog:safepoint=trace:safepointtrace.txt
  - -Xlog:gc+class=debug
  - *-Xlog:gc+meta*=trace,class*=off:file=gcmetatrace.txt
  - -Xlog:gc+meta=trace
  - *-Xlog:gc+class+heap*=debug,meta*=warning,threads*=off
- **-xx参数**
  - -XX:+UnlockDiagnosticVMOptions
  - -XX:+UnlockExperimentalVMOptions
  - -XX:ActiveProcessorCount=x
  - -XX:AllocateHeapAt=path
  - -XX:-CompactStrings
  - -XX:ErrorFile=filename
  - -XX:ErrorFile=./hs_err_pid%p.log
  - -XX:ErrorFile=/var/log/java/java_error.log
  - -XX:ErrorFile=C:/log/java/java_error.log
  - -XX:+ExtensiveErrorReports
  - -XX:FlightRecorderOptions=parameter=value (or)
  - -XX:FlightRecorderOptions:parameter=value
  - -XX:LargePageSizeInBytes=size
  - -XX:LargePageSizeInBytes=1g
  - -XX:MaxDirectMemorySize=size
  - -XX:MaxDirectMemorySize=1m
  - -XX:MaxDirectMemorySize=1024k
  - -XX:MaxDirectMemorySize=1048576
  - -XX:-MaxFDLimit
  - -XX:NativeMemoryTracking=mode
  - -XX:ObjectAlignmentInBytes=alignment
  - -XX:OnError=string
  - -XX:OnError="gcore %p;gdb -p %p"
  - -XX:OnError="userdump.exe %p"
  - -XX:OnOutOfMemoryError=string
  - -XX:+PrintCommandLineFlags //很重要的一个参数，可以展示项目使用的jvm参数，而且可以显示使用的是哪一个gc垃圾回收器
  - -XX:+PreserveFramePointer
  - -XX:+PrintNMTStatistics
  - -XX:SharedArchiveFile=path
  - -XX:SharedArchiveConfigFile=shared_config_file
  - -XX:SharedClassListFile=file_name
  - -XX:+ShowCodeDetailsInExceptionMessages
  - -XX:+ShowMessageBoxOnError
  - -XX:StartFlightRecording=parameter=value
  - -XX:ThreadStackSize=size
  - -XX:ThreadStackSize=1k
  - -XX:ThreadStackSize=1024
  - -XX:-UseCompressedOops
  - -XX:-UseContainerSupport
  - -XX:+UseHugeTLBFS
  - -XX:+UseLargePages
  - -XX:+UseTransparentHugePages
  - -XX:+AllowUserSignalHandlers
  - -XX:VMOptionsFile=filename
  - -XX:AllocateInstancePrefetchLines=lines
  - -XX:AllocateInstancePrefetchLines=1
  - -XX:AllocatePrefetchDistance=size
  - -XX:AllocatePrefetchDistance=1024
  - -XX:AllocatePrefetchInstr=instruction
  - -XX:AllocatePrefetchInstr=0
  - -XX:AllocatePrefetchLines=lines
  - -XX:AllocatePrefetchLines=5
  - -XX:AllocatePrefetchStepSize=size
  - -XX:AllocatePrefetchStepSize=16
  - -XX:AllocatePrefetchStyle=style
  - -XX:+BackgroundCompilation
  - -XX:CICompilerCount=threads
  - -XX:CICompilerCount=2
  - -XX:+UseDynamicNumberOfCompilerThreads
  - -XX:CompileCommand=command,method[,option]
  - -XX:CompileCommand=exclude,java/lang/String.indexOf
  - -XX:CompileCommand=exclude,java.lang.String::indexOf
  - -XX:CompileCommand="exclude,java/lang/String.indexOf,(Ljava/lang/String;)I"
  - -XX:CompileCommand=exclude,*.indexOf
  - -XX:CompileCommand="exclude java/lang/String indexOf"
  - -XX:CompileCommand=option,java/lang/StringBuffer.append,BlockLayoutByFrequency
  - -XX:CompileCommandFile=filename
  - -XX:CompilerDirectivesFile=file
  - -XX:+CompilerDirectivesPrint
  - -XX:CompileOnly=methods
  - -XX:CompileOnly=java/lang/String.length,java/util/List.size
  - -XX:CompileOnly=java.lang.String::length,java.util.List::size
  - -XX:CompileOnly=java/lang/String
  - -XX:CompileOnly=java/lang
  - -XX:CompileOnly=.length
  - -XX:CompileThresholdScaling=scale
  - -XX:+DoEscapeAnalysis
  - -XX:InitialCodeCacheSize=size
  - -XX:InitialCodeCacheSize=32k
  - -XX:+Inline
  - -XX:InlineSmallCode=size
  - -XX:InlineSmallCode=1000
  - -XX:+LogCompilation
  - -XX:FreqInlineSize=size
  - -XX:FreqInlineSize=325
  - -XX:MaxInlineSize=size
  - -XX:MaxInlineSize=35
  - -XX:C1MaxInlineSize=size
  - -XX:MaxInlineSize=35
  - -XX:MaxTrivialSize=size
  - -XX:MaxTrivialSize=6
  - -XX:C1MaxTrivialSize=size
  - -XX:MaxTrivialSize=6
  - -XX:MaxNodeLimit=nodes
  - -XX:MaxNodeLimit=100000
  - -XX:NonNMethodCodeHeapSize=size
  - -XX:NonProfiledCodeHeapSize=size
  - -XX:+OptimizeStringConcat
  - -XX:+PrintAssembly
  - -XX:ProfiledCodeHeapSize=size
  - -XX:+PrintCompilation
  - -XX:+PrintInlining
  - -XX:ReservedCodeCacheSize=size
  - -XX:RTMAbortRatio=abort_ratio
  - -XX:RTMRetryCount=number_of_retries
  - -XX:+SegmentedCodeCache
  - -XX:StartAggressiveSweepingAt=percent
  - -XX:-TieredCompilation
  - -XX:UseSSE=version
  - -XX:UseAVX=version
  - -XX:+UseAES
  - -XX:+UseAESIntrinsics
  - -XX:+UseAES -XX:+UseAESIntrinsics
  - -XX:+UseAESCTRIntrinsics
  - -XX:+UseGHASHIntrinsics
  - -XX:+UseBASE64Intrinsics
  - -XX:+UseAdler32Intrinsics
  - -XX:+UseCRC32Intrinsics
  - -XX:+UseCRC32CIntrinsics
  - -XX:+UseSHA
  - -XX:+UseSHA1Intrinsics
  - -XX:+UseSHA256Intrinsics
  - -XX:+UseSHA512Intrinsics
  - -XX:+UseMathExactIntrinsics
  - -XX:+UseMultiplyToLenIntrinsic
  - -XX:+UseSquareToLenIntrinsic
  - -XX:+UseMulAddIntrinsic
  - -XX:+UseMontgomeryMultiplyIntrinsic
  - -XX:+UseMontgomerySquareIntrinsic
  - -XX:+UseCMoveUnconditionally
  - -XX:+UseCodeCacheFlushing
  - -XX:+UseCondCardMark
  - -XX:+UseCountedLoopSafepoints
  - -XX:LoopStripMiningIter=number_of_iterations
  - -XX:LoopStripMiningIterShortLoop=number_of_iterations
  - -XX:+UseFMA
  - -XX:+UseRTMDeopt
  - -XX:+UseRTMLocking
  - -XX:+UseSuperWord
  - -XX:+DisableAttachMechanism
  - -XX:+ExtendedDTraceProbes
  - -XX:+HeapDumpOnOutOfMemoryError
  - -XX:HeapDumpPath=path
  - -XX:HeapDumpPath=./java_pid%p.hprof
  - -XX:HeapDumpPath=/var/log/java/java_heapdump.hprof
  - -XX:HeapDumpPath=C:/log/java/java_heapdump.log
  - -XX:LogFile=path
  - -XX:LogFile=/var/log/java/hotspot.log
  - -XX:LogFile=C:/log/java/hotspot.log
  - -XX:+PrintClassHistogram
  - -XX:+PrintConcurrentLocks
  - -XX:+PrintFlagsInitial //打印初始化的jvm参数
  - -XX:+PrintFlagsFinal //打印最后的jvm参数
  - -XX:+PrintFlagsRanges
  - -XX:+PerfDataSaveToFile
  - -XX:+UsePerfData
  - -XX:+AggressiveHeap
  - -XX:+AlwaysPreTouch
  - -XX:ConcGCThreads=threads
  - -XX:ConcGCThreads=2
  - -XX:+DisableExplicitGC
  - -XX:+ExplicitGCInvokesConcurrent
  - -XX:G1AdaptiveIHOPNumInitialSamples=number
  - -XX:G1HeapRegionSize=size
  - -XX:G1HeapRegionSize=16m
  - -XX:G1HeapWastePercent=percent
  - -XX:G1MaxNewSizePercent=percent
  - -XX:G1MixedGCCountTarget=number
  - -XX:G1MixedGCLiveThresholdPercent=percent
  - -XX:G1NewSizePercent=percent
  - -XX:G1OldCSetRegionThresholdPercent=percent
  - -XX:G1ReservePercent=percent
  - -XX:G1ReservePercent=20
  - -XX:+G1UseAdaptiveIHOP
  - -XX:InitialHeapSize=size
  - -XX:InitialHeapSize=6291456
  - -XX:InitialHeapSize=6144k
  - -XX:InitialHeapSize=6m
  - -XX:InitialRAMPercentage=percent
  - -XX:InitialRAMPercentage=5
  - -XX:InitialSurvivorRatio=ratio
  - -XX:InitialSurvivorRatio=4
  - -XX:InitiatingHeapOccupancyPercent=percent
  - -XX:InitiatingHeapOccupancyPercent=75
  - -XX:MaxGCPauseMillis=time
  - -XX:MaxGCPauseMillis=500
  - -XX:MaxHeapSize=size
  - -XX:MaxHeapSize=83886080
  - -XX:MaxHeapSize=81920k
  - -XX:MaxHeapSize=80m
  - -XX:MaxHeapFreeRatio=percent
  - -XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5
  - -XX:MaxMetaspaceSize=size
  - -XX:MaxMetaspaceSize=256m
  - -XX:MaxNewSize=size
  - -XX:MaxRAM=size
  - -XX:MaxRAM=2G
  - -XX:MaxRAMPercentage=percent
  - -XX:MaxRAMPercentage=75
  - -XX:MinRAMPercentage=percent
  - -XX:MinRAMPercentage=75
  - -XX:MaxTenuringThreshold=threshold
  - -XX:MaxTenuringThreshold=10 //幸存者交换多少次之后进入老年代
  - -XX:MetaspaceSize=size
  - -XX:MinHeapFreeRatio=percent
  - -XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5
  - -XX:MinHeapSize=size
  - -XX:MinHeapSize=6291456
  - -XX:MinHeapSize=6144k
  - -XX:MinHeapSize=6m
  - -XX:NewRatio=ratio
  - -XX:NewRatio=1
  - -XX:NewSize=size
  - -XX:NewSize=256m
  - -XX:NewSize=262144k
  - -XX:NewSize=268435456
  - -XX:ParallelGCThreads=threads
  - -XX:ParallelGCThreads=2
  - -XX:+ParallelRefProcEnabled
  - -XX:+PrintAdaptiveSizePolicy
  - -XX:+ScavengeBeforeFullGC
  - -XX:SoftRefLRUPolicyMSPerMB=time
  - -XX:SoftRefLRUPolicyMSPerMB=2500
  - -XX:-ShrinkHeapInSteps
  - -XX:StringDeduplicationAgeThreshold=threshold
  - -XX:SurvivorRatio=ratio
  - -XX:SurvivorRatio=4
  - -XX:TargetSurvivorRatio=percent
  - -XX:TargetSurvivorRatio=30
  - -XX:TLABSize=size
  - -XX:TLABSize=512k
  - -XX:+UseAdaptiveSizePolicy
  - -XX:+UseG1GC
  - -XX:+UseGCOverheadLimit
  - -XX:+UseNUMA
  - -XX:+UseParallelGC
  - -XX:+UseSerialGC
  - -XX:+UseSHM
  - -XX:+UseStringDeduplication
  - -XX:+UseTLAB
  - -XX:+UseZGC
  - -XX:ZAllocationSpikeTolerance=factor
  - -XX:ZCollectionInterval=seconds
  - -XX:ZFragmentationLimit=percent
  - -XX:+ZProactive
  - -XX:+ZUncommit
  - -XX:ZUncommitDelay=seconds
  - -XX:+FlightRecorder
  - -XX:InitialRAMFraction=ratio
  - -XX:MaxRAMFraction=ratio
  - -XX:MinRAMFraction=ratio
  - -XX:+UseBiasedLocking
  - -XX:+UseMembar
  - -XX:MaxPermSize=size
  - -XX:PermSize=size
  - -XX:+TraceClassLoading
  - -XX:+TraceClassLoadingPreorder
  - -XX:+TraceClassResolution
  - -XX:+TraceLoaderConstraints
  - -XX:SharedArchiveConfigFile=shared_config_file
  - -XX:MaxHeapFreeRatio=10 -XX:MinHeapFreeRatio=5

```sh
jps -l # 查看进程
jinfo -flag [option] java进程号 # 查看当前项目使用的java功能
jinfo -flags
```

**-Xms -XX:MaxHeapSize=size和-Xxs -XX:MinHeapSize=size是-XX命令**

=符号表示jvm默认初始化的:=表示是用户手动添加的

```
[Global flags]
     intx ActiveProcessorCount                      = -1                                  {product}
    uintx AdaptiveSizeDecrementScaleFactor          = 4                                   {product}
    uintx AdaptiveSizeMajorGCDecayTimeScale         = 10                                  {product}
    uintx AdaptiveSizePausePolicy                   = 0                                   {product}
    uintx AdaptiveSizePolicyCollectionCostMargin    = 50                                  {product}
    uintx AdaptiveSizePolicyInitializingSteps       = 20                                  {product}
    uintx AdaptiveSizePolicyOutputInterval          = 0                                   {product}
    uintx AdaptiveSizePolicyWeight                  = 10                                  {product}
    uintx AdaptiveSizeThroughPutPolicy              = 0                                   {product}
    uintx AdaptiveTimeWeight                        = 25                                  {product}
     bool AdjustConcurrency                         = false                               {product}
     bool AggressiveHeap                            = false                               {product}
     bool AggressiveOpts                            = false                               {product}
     intx AliasLevel                                = 3                                   {C2 product}
     bool AlignVector                               = true                                {C2 product}
     intx AllocateInstancePrefetchLines             = 1                                   {product}
     intx AllocatePrefetchDistance                  = -1                                  {product}
     intx AllocatePrefetchInstr                     = 0                                   {product}
     intx AllocatePrefetchLines                     = 3                                   {product}
     intx AllocatePrefetchStepSize                  = 16                                  {product}
     intx AllocatePrefetchStyle                     = 1                                   {product}
     bool AllowJNIEnvProxy                          = false                               {product}
     bool AllowNonVirtualCalls                      = false                               {product}
     bool AllowParallelDefineClass                  = false                               {product}
     bool AllowUserSignalHandlers                   = false                               {product}
     bool AlwaysActAsServerClassMachine             = false                               {product}
     bool AlwaysCompileLoopMethods                  = false                               {product}
     bool AlwaysLockClassLoader                     = false                               {product}
     bool AlwaysPreTouch                            = false                               {product}
     bool AlwaysRestoreFPU                          = false                               {product}
     bool AlwaysTenure                              = false                               {product}
     bool AssertOnSuspendWaitFailure                = false                               {product}
     bool AssumeMP                                  = false                               {product}
     intx AutoBoxCacheMax                           = 128                                 {C2 product}
    uintx AutoGCSelectPauseMillis                   = 5000                                {product}
     intx BCEATraceLevel                            = 0                                   {product}
     intx BackEdgeThreshold                         = 100000                              {pd product}
     bool BackgroundCompilation                     = true                                {pd product}
    uintx BaseFootPrintEstimate                     = 268435456                           {product}
     intx BiasedLockingBulkRebiasThreshold          = 20                                  {product}
     intx BiasedLockingBulkRevokeThreshold          = 40                                  {product}
     intx BiasedLockingDecayTime                    = 25000                               {product}
     intx BiasedLockingStartupDelay                 = 4000                                {product}
     bool BindGCTaskThreadsToCPUs                   = false                               {product}
     bool BlockLayoutByFrequency                    = true                                {C2 product}
     intx BlockLayoutMinDiamondPercentage           = 20                                  {C2 product}
     bool BlockLayoutRotateLoops                    = true                                {C2 product}
     bool BranchOnRegister                          = false                               {C2 product}
     bool BytecodeVerificationLocal                 = false                               {product}
     bool BytecodeVerificationRemote                = true                                {product}
     bool C1OptimizeVirtualCallProfiling            = true                                {C1 product}
     bool C1ProfileBranches                         = true                                {C1 product}
     bool C1ProfileCalls                            = true                                {C1 product}
     bool C1ProfileCheckcasts                       = true                                {C1 product}
     bool C1ProfileInlinedCalls                     = true                                {C1 product}
     bool C1ProfileVirtualCalls                     = true                                {C1 product}
     bool C1UpdateMethodData                        = true                                {C1 product}
     intx CICompilerCount                           = 2                                   {product}
     bool CICompilerCountPerCPU                     = false                               {product}
     bool CITime                                    = false                               {product}
     bool CMSAbortSemantics                         = false                               {product}
    uintx CMSAbortablePrecleanMinWorkPerIteration   = 100                                 {product}
     intx CMSAbortablePrecleanWaitMillis            = 100                                 {manageable}
    uintx CMSBitMapYieldQuantum                     = 10485760                            {product}
    uintx CMSBootstrapOccupancy                     = 50                                  {product}
     bool CMSClassUnloadingEnabled                  = true                                {product}
    uintx CMSClassUnloadingMaxInterval              = 0                                   {product}
     bool CMSCleanOnEnter                           = true                                {product}
     bool CMSCompactWhenClearAllSoftRefs            = true                                {product}
    uintx CMSConcMarkMultiple                       = 32                                  {product}
     bool CMSConcurrentMTEnabled                    = true                                {product}
    uintx CMSCoordinatorYieldSleepCount             = 10                                  {product}
     bool CMSDumpAtPromotionFailure                 = false                               {product}
     bool CMSEdenChunksRecordAlways                 = true                                {product}
    uintx CMSExpAvgFactor                           = 50                                  {product}
     bool CMSExtrapolateSweep                       = false                               {product}
    uintx CMSFullGCsBeforeCompaction                = 0                                   {product}
    uintx CMSIncrementalDutyCycle                   = 10                                  {product}
    uintx CMSIncrementalDutyCycleMin                = 0                                   {product}
     bool CMSIncrementalMode                        = false                               {product}
    uintx CMSIncrementalOffset                      = 0                                   {product}
     bool CMSIncrementalPacing                      = true                                {product}
    uintx CMSIncrementalSafetyFactor                = 10                                  {product}
    uintx CMSIndexedFreeListReplenish               = 4                                   {product}
     intx CMSInitiatingOccupancyFraction            = -1                                  {product}
    uintx CMSIsTooFullPercentage                    = 98                                  {product}
   double CMSLargeCoalSurplusPercent                = 0.950000                            {product}
   double CMSLargeSplitSurplusPercent               = 1.000000                            {product}
     bool CMSLoopWarn                               = false                               {product}
    uintx CMSMaxAbortablePrecleanLoops              = 0                                   {product}
     intx CMSMaxAbortablePrecleanTime               = 5000                                {product}
    uintx CMSOldPLABMax                             = 1024                                {product}
    uintx CMSOldPLABMin                             = 16                                  {product}
    uintx CMSOldPLABNumRefills                      = 4                                   {product}
    uintx CMSOldPLABReactivityFactor                = 2                                   {product}
     bool CMSOldPLABResizeQuicker                   = false                               {product}
    uintx CMSOldPLABToleranceFactor                 = 4                                   {product}
     bool CMSPLABRecordAlways                       = true                                {product}
    uintx CMSParPromoteBlocksToClaim                = 16                                  {product}
     bool CMSParallelInitialMarkEnabled             = true                                {product}
     bool CMSParallelRemarkEnabled                  = true                                {product}
     bool CMSParallelSurvivorRemarkEnabled          = true                                {product}
    uintx CMSPrecleanDenominator                    = 3                                   {product}
    uintx CMSPrecleanIter                           = 3                                   {product}
    uintx CMSPrecleanNumerator                      = 2                                   {product}
     bool CMSPrecleanRefLists1                      = true                                {product}
     bool CMSPrecleanRefLists2                      = false                               {product}
     bool CMSPrecleanSurvivors1                     = false                               {product}
     bool CMSPrecleanSurvivors2                     = true                                {product}
    uintx CMSPrecleanThreshold                      = 1000                                {product}
     bool CMSPrecleaningEnabled                     = true                                {product}
     bool CMSPrintChunksInDump                      = false                               {product}
     bool CMSPrintEdenSurvivorChunks                = false                               {product}
     bool CMSPrintObjectsInDump                     = false                               {product}
    uintx CMSRemarkVerifyVariant                    = 1                                   {product}
     bool CMSReplenishIntermediate                  = true                                {product}
    uintx CMSRescanMultiple                         = 32                                  {product}
    uintx CMSSamplingGrain                          = 16384                               {product}
     bool CMSScavengeBeforeRemark                   = false                               {product}
    uintx CMSScheduleRemarkEdenPenetration          = 50                                  {product}
    uintx CMSScheduleRemarkEdenSizeThreshold        = 2097152                             {product}
    uintx CMSScheduleRemarkSamplingRatio            = 5                                   {product}
   double CMSSmallCoalSurplusPercent                = 1.050000                            {product}
   double CMSSmallSplitSurplusPercent               = 1.100000                            {product}
     bool CMSSplitIndexedFreeListBlocks             = true                                {product}
     intx CMSTriggerInterval                        = -1                                  {manageable}
    uintx CMSTriggerRatio                           = 80                                  {product}
     intx CMSWaitDuration                           = 2000                                {manageable}
    uintx CMSWorkQueueDrainThreshold                = 10                                  {product}
     bool CMSYield                                  = true                                {product}
    uintx CMSYieldSleepCount                        = 0                                   {product}
    uintx CMSYoungGenPerWorker                      = 67108864                            {pd product}
    uintx CMS_FLSPadding                            = 1                                   {product}
    uintx CMS_FLSWeight                             = 75                                  {product}
    uintx CMS_SweepPadding                          = 1                                   {product}
    uintx CMS_SweepTimerThresholdMillis             = 10                                  {product}
    uintx CMS_SweepWeight                           = 75                                  {product}
     bool CheckEndorsedAndExtDirs                   = false                               {product}
     bool CheckJNICalls                             = false                               {product}
     bool ClassUnloading                            = true                                {product}
     bool ClassUnloadingWithConcurrentMark          = true                                {product}
     intx ClearFPUAtPark                            = 0                                   {product}
     bool ClipInlining                              = true                                {product}
    uintx CodeCacheExpansionSize                    = 65536                               {pd product}
    uintx CodeCacheMinimumFreeSpace                 = 512000                              {product}
     bool CollectGen0First                          = false                               {product}
     bool CompactFields                             = true                                {product}
     intx CompilationPolicyChoice                   = 0                                   {product}
ccstrlist CompileCommand                            =                                     {product}
    ccstr CompileCommandFile                        =                                     {product}
ccstrlist CompileOnly                               =                                     {product}
     intx CompileThreshold                          = 10000                               {pd product}
     bool CompilerThreadHintNoPreempt               = true                                {product}
     intx CompilerThreadPriority                    = -1                                  {product}
     intx CompilerThreadStackSize                   = 0                                   {pd product}
    uintx CompressedClassSpaceSize                  = 1073741824                          {product}
    uintx ConcGCThreads                             = 0                                   {product}
     intx ConditionalMoveLimit                      = 3                                   {C2 pd product}
     intx ContendedPaddingWidth                     = 128                                 {product}
     bool ConvertSleepToYield                       = true                                {pd product}
     bool ConvertYieldToSleep                       = false                               {product}
     bool CrashOnOutOfMemoryError                   = false                               {product}
     bool CreateMinidumpOnCrash                     = false                               {product}
     bool CriticalJNINatives                        = true                                {product}
     bool DTraceAllocProbes                         = false                               {product}
     bool DTraceMethodProbes                        = false                               {product}
     bool DTraceMonitorProbes                       = false                               {product}
     bool Debugging                                 = false                               {product}
    uintx DefaultMaxRAMFraction                     = 4                                   {product}
     intx DefaultThreadPriority                     = -1                                  {product}
     intx DeferPollingPageLoopCount                 = -1                                  {product}
     intx DeferThrSuspendLoopCount                  = 4000                                {product}
     bool DeoptimizeRandom                          = false                               {product}
     bool DisableAttachMechanism                    = false                               {product}
     bool DisableExplicitGC                         = false                               {product}
     bool DisplayVMOutputToStderr                   = false                               {product}
     bool DisplayVMOutputToStdout                   = false                               {product}
     bool DoEscapeAnalysis                          = true                                {C2 product}
     bool DontCompileHugeMethods                    = true                                {product}
     bool DontYieldALot                             = false                               {pd product}
    ccstr DumpLoadedClassList                       =                                     {product}
     bool DumpReplayDataOnError                     = true                                {product}
     bool DumpSharedSpaces                          = false                               {product}
     bool EagerXrunInit                             = false                               {product}
     intx EliminateAllocationArraySizeLimit         = 64                                  {C2 product}
     bool EliminateAllocations                      = true                                {C2 product}
     bool EliminateAutoBox                          = true                                {C2 product}
     bool EliminateLocks                            = true                                {C2 product}
     bool EliminateNestedLocks                      = true                                {C2 product}
     intx EmitSync                                  = 0                                   {product}
     bool EnableContended                           = true                                {product}
     bool EnableResourceManagementTLABCache         = true                                {product}
     bool EnableSharedLookupCache                   = true                                {product}
     bool EnableTracing                             = false                               {product}
    uintx ErgoHeapSizeLimit                         = 0                                   {product}
    ccstr ErrorFile                                 =                                     {product}
    ccstr ErrorReportServer                         =                                     {product}
   double EscapeAnalysisTimeout                     = 20.000000                           {C2 product}
     bool EstimateArgEscape                         = true                                {product}
     bool ExitOnOutOfMemoryError                    = false                               {product}
     bool ExplicitGCInvokesConcurrent               = false                               {product}
     bool ExplicitGCInvokesConcurrentAndUnloadsClasses  = false                               {product}
     bool ExtendedDTraceProbes                      = false                               {product}
    ccstr ExtraSharedClassListFile                  =                                     {product}
     bool FLSAlwaysCoalesceLarge                    = false                               {product}
    uintx FLSCoalescePolicy                         = 2                                   {product}
   double FLSLargestBlockCoalesceProximity          = 0.990000                            {product}
     bool FailOverToOldVerifier                     = true                                {product}
     bool FastTLABRefill                            = true                                {product}
     intx FenceInstruction                          = 0                                   {ARCH product}
     intx FieldsAllocationStyle                     = 1                                   {product}
     bool FilterSpuriousWakeups                     = true                                {product}
    ccstr FlightRecorderOptions                     =                                     {product}
     bool ForceNUMA                                 = false                               {product}
     bool ForceTimeHighResolution                   = false                               {product}
     intx FreqInlineSize                            = 325                                 {pd product}
   double G1ConcMarkStepDurationMillis              = 10.000000                           {product}
    uintx G1ConcRSHotCardLimit                      = 4                                   {product}
    uintx G1ConcRSLogCacheSize                      = 10                                  {product}
     intx G1ConcRefinementGreenZone                 = 0                                   {product}
     intx G1ConcRefinementRedZone                   = 0                                   {product}
     intx G1ConcRefinementServiceIntervalMillis     = 300                                 {product}
    uintx G1ConcRefinementThreads                   = 0                                   {product}
     intx G1ConcRefinementThresholdStep             = 0                                   {product}
     intx G1ConcRefinementYellowZone                = 0                                   {product}
    uintx G1ConfidencePercent                       = 50                                  {product}
    uintx G1HeapRegionSize                          = 0                                   {product}
    uintx G1HeapWastePercent                        = 5                                   {product}
    uintx G1MixedGCCountTarget                      = 8                                   {product}
     intx G1RSetRegionEntries                       = 0                                   {product}
    uintx G1RSetScanBlockSize                       = 64                                  {product}
     intx G1RSetSparseRegionEntries                 = 0                                   {product}
     intx G1RSetUpdatingPauseTimePercent            = 10                                  {product}
     intx G1RefProcDrainInterval                    = 10                                  {product}
    uintx G1ReservePercent                          = 10                                  {product}
    uintx G1SATBBufferEnqueueingThresholdPercent    = 60                                  {product}
     intx G1SATBBufferSize                          = 1024                                {product}
     intx G1UpdateBufferSize                        = 256                                 {product}
     bool G1UseAdaptiveConcRefinement               = true                                {product}
    uintx GCDrainStackTargetSize                    = 64                                  {product}
    uintx GCHeapFreeLimit                           = 2                                   {product}
    uintx GCLockerEdenExpansionPercent              = 5                                   {product}
     bool GCLockerInvokesConcurrent                 = false                               {product}
    uintx GCLogFileSize                             = 8192                                {product}
    uintx GCPauseIntervalMillis                     = 0                                   {product}
    uintx GCTaskTimeStampEntries                    = 200                                 {product}
    uintx GCTimeLimit                               = 98                                  {product}
    uintx GCTimeRatio                               = 99                                  {product}
    uintx HeapBaseMinAddress                        = 2147483648                          {pd product}
     bool HeapDumpAfterFullGC                       = false                               {manageable}
     bool HeapDumpBeforeFullGC                      = false                               {manageable}
     bool HeapDumpOnOutOfMemoryError                = false                               {manageable}
    ccstr HeapDumpPath                              =                                     {manageable}
    uintx HeapFirstMaximumCompactionCount           = 3                                   {product}
    uintx HeapMaximumCompactionInterval             = 20                                  {product}
    uintx HeapSizePerGCThread                       = 87241520                            {product}
     bool IgnoreEmptyClassPaths                     = false                               {product}
     bool IgnoreUnrecognizedVMOptions               = false                               {product}
    uintx IncreaseFirstTierCompileThresholdAt       = 50                                  {product}
     bool IncrementalInline                         = true                                {C2 product}
    uintx InitialBootClassLoaderMetaspaceSize       = 4194304                             {product}
    uintx InitialCodeCacheSize                      = 2555904                             {pd product}
    uintx InitialHeapSize                           = 0                                   {product}
    uintx InitialRAMFraction                        = 64                                  {product}
   double InitialRAMPercentage                      = 1.562500                            {product}
    uintx InitialSurvivorRatio                      = 8                                   {product}
    uintx InitialTenuringThreshold                  = 7                                   {product}
    uintx InitiatingHeapOccupancyPercent            = 45                                  {product}
     bool Inline                                    = true                                {product}
    ccstr InlineDataFile                            =                                     {product}
     intx InlineSmallCode                           = 1000                                {pd product}
     bool InlineSynchronizedMethods                 = true                                {C1 product}
     bool InsertMemBarAfterArraycopy                = true                                {C2 product}
     intx InteriorEntryAlignment                    = 16                                  {C2 pd product}
     intx InterpreterProfilePercentage              = 33                                  {product}
     bool JNIDetachReleasesMonitors                 = true                                {product}
     bool JavaMonitorsInStackTrace                  = true                                {product}
     intx JavaPriority10_To_OSPriority              = -1                                  {product}
     intx JavaPriority1_To_OSPriority               = -1                                  {product}
     intx JavaPriority2_To_OSPriority               = -1                                  {product}
     intx JavaPriority3_To_OSPriority               = -1                                  {product}
     intx JavaPriority4_To_OSPriority               = -1                                  {product}
     intx JavaPriority5_To_OSPriority               = -1                                  {product}
     intx JavaPriority6_To_OSPriority               = -1                                  {product}
     intx JavaPriority7_To_OSPriority               = -1                                  {product}
     intx JavaPriority8_To_OSPriority               = -1                                  {product}
     intx JavaPriority9_To_OSPriority               = -1                                  {product}
     bool LIRFillDelaySlots                         = false                               {C1 pd product}
    uintx LargePageHeapSizeThreshold                = 134217728                           {product}
    uintx LargePageSizeInBytes                      = 0                                   {product}
     bool LazyBootClassLoader                       = true                                {product}
     intx LiveNodeCountInliningCutoff               = 40000                               {C2 product}
     bool LogCommercialFeatures                     = false                               {product}
     intx LoopMaxUnroll                             = 16                                  {C2 product}
     intx LoopOptsCount                             = 43                                  {C2 product}
     intx LoopUnrollLimit                           = 60                                  {C2 pd product}
     intx LoopUnrollMin                             = 4                                   {C2 product}
     bool LoopUnswitching                           = true                                {C2 product}
     bool ManagementServer                          = false                               {product}
    uintx MarkStackSize                             = 4194304                             {product}
    uintx MarkStackSizeMax                          = 536870912                           {product}
    uintx MarkSweepAlwaysCompactCount               = 4                                   {product}
    uintx MarkSweepDeadRatio                        = 5                                   {product}
     intx MaxBCEAEstimateLevel                      = 5                                   {product}
     intx MaxBCEAEstimateSize                       = 150                                 {product}
    uintx MaxDirectMemorySize                       = 0                                   {product}
     bool MaxFDLimit                                = true                                {product}
    uintx MaxGCMinorPauseMillis                     = 4294967295                          {product}
    uintx MaxGCPauseMillis                          = 4294967295                          {product}
    uintx MaxHeapFreeRatio                          = 70                                  {manageable}
    uintx MaxHeapSize                               = 130862280                           {product}
     intx MaxInlineLevel                            = 9                                   {product}
     intx MaxInlineSize                             = 35                                  {product}
     intx MaxJNILocalCapacity                       = 65536                               {product}
     intx MaxJavaStackTraceDepth                    = 1024                                {product}
     intx MaxJumpTableSize                          = 65000                               {C2 product}
     intx MaxJumpTableSparseness                    = 5                                   {C2 product}
     intx MaxLabelRootDepth                         = 1100                                {C2 product}
     intx MaxLoopPad                                = 15                                  {C2 product}
    uintx MaxMetaspaceExpansion                     = 5452592                             {product}
    uintx MaxMetaspaceFreeRatio                     = 70                                  {product}
    uintx MaxMetaspaceSize                          = 4294967295                          {product}
    uintx MaxNewSize                                = 4294967295                          {product}
     intx MaxNodeLimit                              = 80000                               {C2 product}
 uint64_t MaxRAM                                    = 0                                   {pd product}
    uintx MaxRAMFraction                            = 4                                   {product}
   double MaxRAMPercentage                          = 25.000000                           {product}
     intx MaxRecursiveInlineLevel                   = 1                                   {product}
    uintx MaxTenuringThreshold                      = 15                                  {product}
     intx MaxTrivialSize                            = 6                                   {product}
     intx MaxVectorSize                             = 32                                  {C2 product}
    uintx MetaspaceSize                             = 21810376                            {pd product}
     bool MethodFlushing                            = true                                {product}
    uintx MinHeapDeltaBytes                         = 170392                              {product}
    uintx MinHeapFreeRatio                          = 40                                  {manageable}
     intx MinInliningThreshold                      = 250                                 {product}
     intx MinJumpTableSize                          = 10                                  {C2 pd product}
    uintx MinMetaspaceExpansion                     = 340784                              {product}
    uintx MinMetaspaceFreeRatio                     = 40                                  {product}
    uintx MinRAMFraction                            = 2                                   {product}
   double MinRAMPercentage                          = 50.000000                           {product}
    uintx MinSurvivorRatio                          = 3                                   {product}
    uintx MinTLABSize                               = 2048                                {product}
     intx MonitorBound                              = 0                                   {product}
     bool MonitorInUseLists                         = false                               {product}
     intx MultiArrayExpandLimit                     = 6                                   {C2 product}
     bool MustCallLoadClassInternal                 = false                               {product}
    uintx NUMAChunkResizeWeight                     = 20                                  {product}
    uintx NUMAInterleaveGranularity                 = 2097152                             {product}
    uintx NUMAPageScanRate                          = 256                                 {product}
    uintx NUMASpaceResizeRate                       = 1073741824                          {product}
     bool NUMAStats                                 = false                               {product}
    ccstr NativeMemoryTracking                      = off                                 {product}
     bool NeedsDeoptSuspend                         = false                               {pd product}
     bool NeverActAsServerClassMachine              = false                               {pd product}
     bool NeverTenure                               = false                               {product}
    uintx NewRatio                                  = 2                                   {product}
    uintx NewSize                                   = 1363144                             {product}
    uintx NewSizeThreadIncrease                     = 5320                                {pd product}
     intx NmethodSweepActivity                      = 10                                  {product}
     intx NmethodSweepCheckInterval                 = 5                                   {product}
     intx NmethodSweepFraction                      = 16                                  {product}
     intx NodeLimitFudgeFactor                      = 2000                                {C2 product}
    uintx NumberOfGCLogFiles                        = 0                                   {product}
     intx NumberOfLoopInstrToAlign                  = 4                                   {C2 product}
     intx ObjectAlignmentInBytes                    = 8                                   {lp64_product}
    uintx OldPLABSize                               = 1024                                {product}
    uintx OldPLABWeight                             = 50                                  {product}
    uintx OldSize                                   = 5452592                             {product}
     bool OmitStackTraceInFastThrow                 = true                                {product}
ccstrlist OnError                                   =                                     {product}
ccstrlist OnOutOfMemoryError                        =                                     {product}
     intx OnStackReplacePercentage                  = 140                                 {pd product}
     bool OptimizeFill                              = true                                {C2 product}
     bool OptimizePtrCompare                        = true                                {C2 product}
     bool OptimizeStringConcat                      = true                                {C2 product}
     bool OptoBundling                              = false                               {C2 pd product}
     intx OptoLoopAlignment                         = 16                                  {pd product}
     bool OptoScheduling                            = false                               {C2 pd product}
    uintx PLABWeight                                = 75                                  {product}
     bool PSChunkLargeArrays                        = true                                {product}
     intx ParGCArrayScanChunk                       = 50                                  {product}
    uintx ParGCDesiredObjsFromOverflowList          = 20                                  {product}
     bool ParGCTrimOverflow                         = true                                {product}
     bool ParGCUseLocalOverflow                     = false                               {product}
    uintx ParallelGCBufferWastePct                  = 10                                  {product}
    uintx ParallelGCThreads                         = 0                                   {product}
     bool ParallelGCVerbose                         = false                               {product}
    uintx ParallelOldDeadWoodLimiterMean            = 50                                  {product}
    uintx ParallelOldDeadWoodLimiterStdDev          = 80                                  {product}
     bool ParallelRefProcBalancingEnabled           = true                                {product}
     bool ParallelRefProcEnabled                    = false                               {product}
     bool PartialPeelAtUnsignedTests                = true                                {C2 product}
     bool PartialPeelLoop                           = true                                {C2 product}
     intx PartialPeelNewPhiDelta                    = 0                                   {C2 product}
    uintx PausePadding                              = 1                                   {product}
     intx PerBytecodeRecompilationCutoff            = 200                                 {product}
     intx PerBytecodeTrapLimit                      = 4                                   {product}
     intx PerMethodRecompilationCutoff              = 400                                 {product}
     intx PerMethodTrapLimit                        = 100                                 {product}
     bool PerfAllowAtExitRegistration               = false                               {product}
     bool PerfBypassFileSystemCheck                 = false                               {product}
     intx PerfDataMemorySize                        = 32768                               {product}
     intx PerfDataSamplingInterval                  = 50                                  {product}
    ccstr PerfDataSaveFile                          =                                     {product}
     bool PerfDataSaveToFile                        = false                               {product}
     bool PerfDisableSharedMem                      = false                               {product}
     intx PerfMaxStringConstLength                  = 1024                                {product}
     intx PreInflateSpin                            = 10                                  {pd product}
     bool PreferInterpreterNativeStubs              = false                               {pd product}
     intx PrefetchCopyIntervalInBytes               = -1                                  {product}
     intx PrefetchFieldsAhead                       = -1                                  {product}
     intx PrefetchScanIntervalInBytes               = -1                                  {product}
     bool PreserveAllAnnotations                    = false                               {product}
     bool PreserveFramePointer                      = false                               {pd product}
    uintx PretenureSizeThreshold                    = 0                                   {product}
     bool PrintAdaptiveSizePolicy                   = false                               {product}
     bool PrintCMSInitiationStatistics              = false                               {product}
     intx PrintCMSStatistics                        = 0                                   {product}
     bool PrintClassHistogram                       = false                               {manageable}
     bool PrintClassHistogramAfterFullGC            = false                               {manageable}
     bool PrintClassHistogramBeforeFullGC           = false                               {manageable}
     bool PrintCodeCache                            = false                               {product}
     bool PrintCodeCacheOnCompilation               = false                               {product}
     bool PrintCommandLineFlags                     = false                               {product}
     bool PrintCompilation                          = false                               {product}
     bool PrintConcurrentLocks                      = false                               {manageable}
     intx PrintFLSCensus                            = 0                                   {product}
     intx PrintFLSStatistics                        = 0                                   {product}
     bool PrintFlagsFinal                           = false                               {product}
     bool PrintFlagsInitial                         = false                               {product}
     bool PrintGC                                   = false                               {manageable}
     bool PrintGCApplicationConcurrentTime          = false                               {product}
     bool PrintGCApplicationStoppedTime             = false                               {product}
     bool PrintGCCause                              = true                                {product}
     bool PrintGCDateStamps                         = false                               {manageable}
     bool PrintGCDetails                            = false                               {manageable}
     bool PrintGCID                                 = false                               {manageable}
     bool PrintGCTaskTimeStamps                     = false                               {product}
     bool PrintGCTimeStamps                         = false                               {manageable}
     bool PrintHeapAtGC                             = false                               {product rw}
     bool PrintHeapAtGCExtended                     = false                               {product rw}
     bool PrintHeapAtSIGBREAK                       = true                                {product}
     bool PrintJNIGCStalls                          = false                               {product}
     bool PrintJNIResolving                         = false                               {product}
     bool PrintOldPLAB                              = false                               {product}
     bool PrintOopAddress                           = false                               {product}
     bool PrintPLAB                                 = false                               {product}
     bool PrintParallelOldGCPhaseTimes              = false                               {product}
     bool PrintPromotionFailure                     = false                               {product}
     bool PrintReferenceGC                          = false                               {product}
     bool PrintSafepointStatistics                  = false                               {product}
     intx PrintSafepointStatisticsCount             = 300                                 {product}
     intx PrintSafepointStatisticsTimeout           = -1                                  {product}
     bool PrintSharedArchiveAndExit                 = false                               {product}
     bool PrintSharedDictionary                     = false                               {product}
     bool PrintSharedSpaces                         = false                               {product}
     bool PrintStringDeduplicationStatistics        = false                               {product}
     bool PrintStringTableStatistics                = false                               {product}
     bool PrintTLAB                                 = false                               {product}
     bool PrintTenuringDistribution                 = false                               {product}
     bool PrintTieredEvents                         = false                               {product}
     bool PrintVMOptions                            = false                               {product}
     bool PrintVMQWaitTime                          = false                               {product}
     bool PrintWarnings                             = true                                {product}
    uintx ProcessDistributionStride                 = 4                                   {product}
     bool ProfileInterpreter                        = true                                {pd product}
     bool ProfileIntervals                          = false                               {product}
     intx ProfileIntervalsTicks                     = 100                                 {product}
     intx ProfileMaturityPercentage                 = 20                                  {product}
     bool ProfileVM                                 = false                               {product}
     bool ProfilerPrintByteCodeStatistics           = false                               {product}
     bool ProfilerRecordPC                          = false                               {product}
    uintx PromotedPadding                           = 3                                   {product}
    uintx QueuedAllocationWarningCount              = 0                                   {product}
    uintx RTMRetryCount                             = 5                                   {ARCH product}
     bool RangeCheckElimination                     = true                                {product}
     intx ReadPrefetchInstr                         = 0                                   {ARCH product}
     bool ReassociateInvariants                     = true                                {C2 product}
     bool ReduceBulkZeroing                         = true                                {C2 product}
     bool ReduceFieldZeroing                        = true                                {C2 product}
     bool ReduceInitialCardMarks                    = true                                {C2 product}
     bool ReduceSignalUsage                         = false                               {product}
     intx RefDiscoveryPolicy                        = 0                                   {product}
     bool ReflectionWrapResolutionErrors            = true                                {product}
     bool RegisterFinalizersAtInit                  = true                                {product}
     bool RelaxAccessControlCheck                   = false                               {product}
    ccstr ReplayDataFile                            =                                     {product}
     bool RequireSharedSpaces                       = false                               {product}
    uintx ReservedCodeCacheSize                     = 50331648                            {pd product}
     bool ResizeOldPLAB                             = true                                {product}
     bool ResizePLAB                                = true                                {product}
     bool ResizeTLAB                                = true                                {pd product}
     bool RestoreMXCSROnJNICalls                    = false                               {product}
     bool RestrictContended                         = true                                {product}
     bool RewriteBytecodes                          = true                                {pd product}
     bool RewriteFrequentPairs                      = true                                {pd product}
     intx SafepointPollOffset                       = 256                                 {C1 pd product}
     intx SafepointSpinBeforeYield                  = 2000                                {product}
     bool SafepointTimeout                          = false                               {product}
     intx SafepointTimeoutDelay                     = 10000                               {product}
     bool ScavengeBeforeFullGC                      = true                                {product}
     intx SelfDestructTimer                         = 0                                   {product}
    uintx SharedBaseAddress                         = 0                                   {product}
    ccstr SharedClassListFile                       =                                     {product}
    uintx SharedMiscCodeSize                        = 122880                              {product}
    uintx SharedMiscDataSize                        = 4194304                             {product}
    uintx SharedReadOnlySize                        = 16777216                            {product}
    uintx SharedReadWriteSize                       = 16777216                            {product}
     bool ShowMessageBoxOnError                     = false                               {product}
     intx SoftRefLRUPolicyMSPerMB                   = 1000                                {product}
     bool SpecialEncodeISOArray                     = true                                {C2 product}
     bool SplitIfBlocks                             = true                                {C2 product}
     intx StackRedPages                             = 1                                   {pd product}
     intx StackShadowPages                          = 6                                   {pd product}
     bool StackTraceInThrowable                     = true                                {product}
     intx StackYellowPages                          = 3                                   {pd product}
     bool StartAttachListener                       = false                               {product}
     intx StarvationMonitorInterval                 = 200                                 {product}
     bool StressLdcRewrite                          = false                               {product}
    uintx StringDeduplicationAgeThreshold           = 3                                   {product}
    uintx StringTableSize                           = 60013                               {product}
     bool SuppressFatalErrorMessage                 = false                               {product}
    uintx SurvivorPadding                           = 3                                   {product}
    uintx SurvivorRatio                             = 8                                   {product}
     intx SuspendRetryCount                         = 50                                  {product}
     intx SuspendRetryDelay                         = 5                                   {product}
     intx SyncFlags                                 = 0                                   {product}
    ccstr SyncKnobs                                 =                                     {product}
     intx SyncVerbose                               = 0                                   {product}
    uintx TLABAllocationWeight                      = 35                                  {product}
    uintx TLABRefillWasteFraction                   = 64                                  {product}
    uintx TLABSize                                  = 0                                   {product}
     bool TLABStats                                 = true                                {product}
    uintx TLABWasteIncrement                        = 4                                   {product}
    uintx TLABWasteTargetPercent                    = 1                                   {product}
    uintx TargetPLABWastePct                        = 10                                  {product}
    uintx TargetSurvivorRatio                       = 50                                  {product}
    uintx TenuredGenerationSizeIncrement            = 20                                  {product}
    uintx TenuredGenerationSizeSupplement           = 80                                  {product}
    uintx TenuredGenerationSizeSupplementDecay      = 2                                   {product}
     intx ThreadPriorityPolicy                      = 0                                   {product}
     bool ThreadPriorityVerbose                     = false                               {product}
    uintx ThreadSafetyMargin                        = 52428800                            {product}
     intx ThreadStackSize                           = 0                                   {pd product}
    uintx ThresholdTolerance                        = 10                                  {product}
     intx Tier0BackedgeNotifyFreqLog                = 10                                  {product}
     intx Tier0InvokeNotifyFreqLog                  = 7                                   {product}
     intx Tier0ProfilingStartPercentage             = 200                                 {product}
     intx Tier23InlineeNotifyFreqLog                = 20                                  {product}
     intx Tier2BackEdgeThreshold                    = 0                                   {product}
     intx Tier2BackedgeNotifyFreqLog                = 14                                  {product}
     intx Tier2CompileThreshold                     = 0                                   {product}
     intx Tier2InvokeNotifyFreqLog                  = 11                                  {product}
     intx Tier3BackEdgeThreshold                    = 60000                               {product}
     intx Tier3BackedgeNotifyFreqLog                = 13                                  {product}
     intx Tier3CompileThreshold                     = 2000                                {product}
     intx Tier3DelayOff                             = 2                                   {product}
     intx Tier3DelayOn                              = 5                                   {product}
     intx Tier3InvocationThreshold                  = 200                                 {product}
     intx Tier3InvokeNotifyFreqLog                  = 10                                  {product}
     intx Tier3LoadFeedback                         = 5                                   {product}
     intx Tier3MinInvocationThreshold               = 100                                 {product}
     intx Tier4BackEdgeThreshold                    = 40000                               {product}
     intx Tier4CompileThreshold                     = 15000                               {product}
     intx Tier4InvocationThreshold                  = 5000                                {product}
     intx Tier4LoadFeedback                         = 3                                   {product}
     intx Tier4MinInvocationThreshold               = 600                                 {product}
     bool TieredCompilation                         = true                                {pd product}
     intx TieredCompileTaskTimeout                  = 50                                  {product}
     intx TieredRateUpdateMaxTime                   = 25                                  {product}
     intx TieredRateUpdateMinTime                   = 1                                   {product}
     intx TieredStopAtLevel                         = 4                                   {product}
     bool TimeLinearScan                            = false                               {C1 product}
     bool TraceBiasedLocking                        = false                               {product}
     bool TraceClassLoading                         = false                               {product rw}
     bool TraceClassLoadingPreorder                 = false                               {product}
     bool TraceClassPaths                           = false                               {product}
     bool TraceClassResolution                      = false                               {product}
     bool TraceClassUnloading                       = false                               {product rw}
     bool TraceDynamicGCThreads                     = false                               {product}
     bool TraceExceptions                           = false                               {product}
     bool TraceGen0Time                             = false                               {product}
     bool TraceGen1Time                             = false                               {product}
    ccstr TraceJVMTI                                =                                     {product}
     bool TraceLoaderConstraints                    = false                               {product rw}
     bool TraceMetadataHumongousAllocation          = false                               {product}
     bool TraceMonitorInflation                     = false                               {product}
     bool TraceParallelOldGCTasks                   = false                               {product}
     intx TraceRedefineClasses                      = 0                                   {product}
     bool TraceSafepointCleanupTime                 = false                               {product}
     bool TraceSharedLookupCache                    = false                               {product}
     bool TraceSuspendWaitFailures                  = false                               {product}
     intx TrackedInitializationLimit                = 50                                  {C2 product}
     bool TransmitErrorReport                       = false                               {product}
     bool TrapBasedNullChecks                       = false                               {pd product}
     bool TrapBasedRangeChecks                      = false                               {C2 pd product}
     intx TypeProfileArgsLimit                      = 2                                   {product}
    uintx TypeProfileLevel                          = 111                                 {pd product}
     intx TypeProfileMajorReceiverPercent           = 90                                  {C2 product}
     intx TypeProfileParmsLimit                     = 2                                   {product}
     intx TypeProfileWidth                          = 2                                   {product}
     intx UnguardOnExecutionViolation               = 0                                   {product}
     bool UnlinkSymbolsALot                         = false                               {product}
     bool Use486InstrsOnly                          = false                               {ARCH product}
     bool UseAES                                    = false                               {product}
     bool UseAESCTRIntrinsics                       = false                               {product}
     bool UseAESIntrinsics                          = false                               {product}
     intx UseAVX                                    = 99                                  {ARCH product}
     bool UseAdaptiveGCBoundary                     = false                               {product}
     bool UseAdaptiveGenerationSizePolicyAtMajorCollection  = true                                {product}
     bool UseAdaptiveGenerationSizePolicyAtMinorCollection  = true                                {product}
     bool UseAdaptiveNUMAChunkSizing                = true                                {product}
     bool UseAdaptiveSizeDecayMajorGCCost           = true                                {product}
     bool UseAdaptiveSizePolicy                     = true                                {product}
     bool UseAdaptiveSizePolicyFootprintGoal        = true                                {product}
     bool UseAdaptiveSizePolicyWithSystemGC         = false                               {product}
     bool UseAddressNop                             = false                               {ARCH product}
     bool UseAltSigs                                = false                               {product}
     bool UseAutoGCSelectPolicy                     = false                               {product}
     bool UseBMI1Instructions                       = false                               {ARCH product}
     bool UseBMI2Instructions                       = false                               {ARCH product}
     bool UseBiasedLocking                          = true                                {product}
     bool UseBimorphicInlining                      = true                                {C2 product}
     bool UseBoundThreads                           = true                                {product}
     bool UseCLMUL                                  = false                               {ARCH product}
     bool UseCMSBestFit                             = true                                {product}
     bool UseCMSCollectionPassing                   = true                                {product}
     bool UseCMSCompactAtFullCollection             = true                                {product}
     bool UseCMSInitiatingOccupancyOnly             = false                               {product}
     bool UseCRC32Intrinsics                        = false                               {product}
     bool UseCodeCacheFlushing                      = true                                {product}
     bool UseCompiler                               = true                                {product}
     bool UseCompilerSafepoints                     = true                                {product}
     bool UseCompressedClassPointers                = false                               {lp64_product}
     bool UseCompressedOops                         = false                               {lp64_product}
     bool UseConcMarkSweepGC                        = false                               {product}
     bool UseCondCardMark                           = false                               {C2 product}
     bool UseCountLeadingZerosInstruction           = false                               {ARCH product}
     bool UseCountTrailingZerosInstruction          = false                               {ARCH product}
     bool UseCountedLoopSafepoints                  = false                               {C2 product}
     bool UseCounterDecay                           = true                                {product}
     bool UseDivMod                                 = true                                {C2 product}
     bool UseDynamicNumberOfGCThreads               = false                               {product}
     bool UseFPUForSpilling                         = false                               {C2 product}
     bool UseFastAccessorMethods                    = true                                {product}
     bool UseFastEmptyMethods                       = true                                {product}
     bool UseFastJNIAccessors                       = true                                {product}
     bool UseFastStosb                              = false                               {ARCH product}
     bool UseG1GC                                   = false                               {product}
     bool UseGCLogFileRotation                      = false                               {product}
     bool UseGCOverheadLimit                        = true                                {product}
     bool UseGCTaskAffinity                         = false                               {product}
     bool UseGHASHIntrinsics                        = false                               {product}
     bool UseHeavyMonitors                          = false                               {product}
     bool UseInlineCaches                           = true                                {product}
     bool UseInterpreter                            = true                                {product}
     bool UseJumpTables                             = true                                {C2 product}
     bool UseLWPSynchronization                     = true                                {product}
     bool UseLargePages                             = false                               {pd product}
     bool UseLargePagesInMetaspace                  = false                               {product}
     bool UseLargePagesIndividualAllocation        := false                               {pd product}
     bool UseLegacyJNINameEscaping                  = false                               {product}
     bool UseLockedTracing                          = false                               {product}
     bool UseLoopCounter                            = true                                {product}
     bool UseLoopInvariantCodeMotion                = true                                {C1 product}
     bool UseLoopPredicate                          = true                                {C2 product}
     bool UseMathExactIntrinsics                    = true                                {C2 product}
     bool UseMaximumCompactionOnSystemGC            = true                                {product}
     bool UseMembar                                 = false                               {pd product}
     bool UseMontgomeryMultiplyIntrinsic            = false                               {C2 product}
     bool UseMontgomerySquareIntrinsic              = false                               {C2 product}
     bool UseMulAddIntrinsic                        = false                               {C2 product}
     bool UseMultiplyToLenIntrinsic                 = false                               {C2 product}
     bool UseNUMA                                   = false                               {product}
     bool UseNUMAInterleaving                       = false                               {product}
     bool UseNewLongLShift                          = false                               {ARCH product}
     bool UseOSErrorReporting                       = false                               {pd product}
     bool UseOldInlining                            = true                                {C2 product}
     bool UseOnStackReplacement                     = true                                {pd product}
     bool UseOnlyInlinedBimorphic                   = true                                {C2 product}
     bool UseOptoBiasInlining                       = true                                {C2 product}
     bool UsePSAdaptiveSurvivorSizePolicy           = true                                {product}
     bool UseParNewGC                               = false                               {product}
     bool UseParallelGC                             = false                               {product}
     bool UseParallelOldGC                          = false                               {product}
     bool UsePerfData                               = true                                {product}
     bool UsePopCountInstruction                    = false                               {product}
     bool UseRDPCForConstantTableBase               = false                               {C2 product}
     bool UseRTMDeopt                               = false                               {ARCH product}
     bool UseRTMLocking                             = false                               {ARCH product}
     bool UseSHA                                    = false                               {product}
     bool UseSHA1Intrinsics                         = false                               {product}
     bool UseSHA256Intrinsics                       = false                               {product}
     bool UseSHA512Intrinsics                       = false                               {product}
     intx UseSSE                                    = 99                                  {product}
     bool UseSSE42Intrinsics                        = false                               {product}
     bool UseSerialGC                               = false                               {product}
     bool UseSharedSpaces                           = true                                {product}
     bool UseSignalChaining                         = true                                {product}
     bool UseSquareToLenIntrinsic                   = false                               {C2 product}
     bool UseStoreImmI16                            = true                                {ARCH product}
     bool UseStringDeduplication                    = false                               {product}
     bool UseSuperWord                              = true                                {C2 product}
     bool UseTLAB                                   = true                                {pd product}
     bool UseThreadPriorities                       = true                                {pd product}
     bool UseTypeProfile                            = true                                {product}
     bool UseTypeSpeculation                        = true                                {C2 product}
     bool UseUTCFileTimestamp                       = true                                {product}
     bool UseUnalignedLoadStores                    = false                               {ARCH product}
     bool UseVMInterruptibleIO                      = false                               {product}
     bool UseXMMForArrayCopy                        = false                               {product}
     bool UseXmmI2D                                 = false                               {ARCH product}
     bool UseXmmI2F                                 = false                               {ARCH product}
     bool UseXmmLoadAndClearUpper                   = true                                {ARCH product}
     bool UseXmmRegToRegMoveAll                     = false                               {ARCH product}
     bool VMThreadHintNoPreempt                     = false                               {product}
     intx VMThreadPriority                          = -1                                  {product}
     intx VMThreadStackSize                         = 0                                   {pd product}
     intx ValueMapInitialSize                       = 11                                  {C1 product}
     intx ValueMapMaxLoopSize                       = 8                                   {C1 product}
     intx ValueSearchLimit                          = 1000                                {C2 product}
     bool VerifyMergedCPBytecodes                   = true                                {product}
     bool VerifySharedSpaces                        = false                               {product}
     intx WorkAroundNPTLTimedWaitHang               = 1                                   {product}
    uintx YoungGenerationSizeIncrement              = 20                                  {product}
    uintx YoungGenerationSizeSupplement             = 80                                  {product}
    uintx YoungGenerationSizeSupplementDecay        = 8                                   {product}
    uintx YoungPLABSize                             = 4096                                {product}
     bool ZeroTLAB                                  = false                               {product}
     intx hashCode                                  = 5                                   {product}

```

## 平时用过那些JVM参数

-Xms、-Xmx、-Xss、-Xmn、-XX:MetaspaceSize、-XX:+PrintGCDetails、-XX:SurvivorRatio、-XX:NewRatio、-XX:MaxTenuringThreshold

1. -Xss:设置线程堆栈大小（以字节为单位）。加字母`k`或`K`表示KB，加`m`或`M`表示MB，加`g`或`G`表示GB。默认值取决于平台。

   ·     Linux/x64（64位）：1024 KB

   ·     macOS（64位）：1024KB

   ·     Windows。默认值取决于虚拟内存

2. -Xms:设置堆的最小和初始大小（以字节为单位）。这个值必须是1024的倍数并且大于1MB。附加字母`k`或`K`表示千字节，`m`或`M`表示兆字节，或`g`或`G`表示千兆字节。下面的例子显示了如何使用各种单位将分配的内存大小设置为6MB。

3. -Xmx:指定堆的最大尺寸（以字节为单位）。这个值必须是1024的倍数并且大于2MB。加字母`k`或`K`表示千字节，加`m`或`M`表示兆字节，加`g`或`G`表示千兆字节。默认值是在运行时根据系统配置来选择的。对于服务器的部署，`-Xms`和`-Xmx`通常被设置为同一个值。下面的例子显示了如何使用各种单位将分配内存的最大允许大小设置为80MB。

4. -Xmn:设置世代收集器中年轻一代（苗圃）的堆的初始和最大尺寸（字节）。附加字母`k`或`K`表示千字节，`m`或`M`表示兆字节，或`g`或`G`表示千兆字节。堆的年轻一代区域用于新对象。与其他区域相比，在这个区域执行GC的频率更高。如果年轻一代的大小太小，那么就会执行很多小的垃圾收集。如果大小太大，那么只执行完全的垃圾收集，这可能需要很长的时间来完成。建议你不要为G1收集器设置年轻一代的大小，并保持年轻一代的大小大于25%，小于其他收集器的整体堆大小的50%。下面的例子显示了如何使用各种单位将年轻一代的初始和最大尺寸设置为256MB。

5. -XX:MetaspaceSize:设置分配的类元数据空间的大小，第一次超过这个大小就会触发垃圾收集。这个垃圾收集的阈值会根据元数据的使用量而增加或减少。默认的大小取决于平台。

6. -XX:+PrintGCDetails:打印GC的基本详细信息

7. -XX:SurvivorRatio：设置伊甸园空间大小和生存者空间大小之间的比例。默认情况下，这个选项被设置为8。下面的例子显示了如何将伊甸园/幸存者空间的比例设置为4。

8. -XX:NewRatio：设置年轻一代和老一代的比例。默认情况下，该选项被设置为2。下面的例子显示了如何将年轻与年老的比例设置为1。

9. -XX:MaxTenuringThreshold：设置用于自适应GC大小的最大租用阈值。**最大的值是15**。并行（吞吐量）收集器的默认值是

![image-20220321215917174](D:\JavaProject\面试题总结Images\image-20220321215917174.png)

![image-20220321220727168](D:\JavaProject\面试题总结Images\image-20220321220727168.png)

## 强引用、软引用、弱引用、虚引用分别是什么?

 ![image-20220321222227590](D:\JavaProject\面试题总结Images\image-20220321222227590.png)

1. 强引用：当内存不足，JVM开始垃圾回收，对于强引用的对象，就算是出现了OOM也不会对该对象进行回收，死都不收。
   强引用是我们最常见的普通对象引用，只要还有强引用指向一个对象，就能表明对象还“活着”，垃圾收集器不会碰这种对象。在Jav中最常见的就是强引用，把一个对象赋给一个引用变量，这个引用变量就是一个强引用。当一个对象被强引用变量引用时，它处于可达状态，它是不可能被垃圾回收机制回收的，即使该对象以后永远都不会被用到JVM也不会回收。因此强引用是选成Java内存泄漏的主要原因之一。
   对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显式地将相应（强）引用赋值为 mul一般认为就是可以被垃圾收集的了(当然具体回收时机还是要看垃圾收集策略)。

2. 软引用是一种相对强引用弱化了一些的引用，需要用java.lang.ref.SoftReference类来实现，可以让对象豁免一些垃圾收集。
   对于只有软引用的对象来说，
   当系统内存充足时它不会被回收，
   当系统内存不足时它会被回收。
   软引用通常用在对内存敏感的程序中，比如高速缓存就有用到软引用，内存够用的时候就保留，不够用就回收!
3. 弱引用需要用java.lang.ref.WeakReference类来实现，它比软引用的生存期更短，
   对于只有弱引用的对象来说，只要垃圾回收机制一运行，不管JVM的内存空间是否足够，都会回收该对象占用的内存。

4. 虚引用需要java.lang.ref.PhantomReferenc类来实现。
   顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。
   如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收.
   它不能单独使用也不能通过它访问
   对象，虚引用必须和引用队列(ReferenceQueue)联合使用。
   虚引用的主要作用是跟踪对象被垃圾回收的状态。仅仅是提供了一种确保对象被finalize以后，做某些事情的机制。
   PhantomReference的get方法总是返回nul，因此无法访问对应的引用对象。其意义在于说明一个对象已经进入finalization阶段，可以被gc回收，用来实现比finalization机制更灵活的回收操作。
   换句话说，设置虚引用关联的唯一目的，就是在这个对象被收集器回收的时候收到一个系统通知或者后续添加进一步的处理。Java技术允许使用finalize()方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。

假如有一个应用需要读取大量的本地图片:
*如果每次读取图片都从硬盘读取则会严重影响性能，*如果一次性全部加载到内存中又可能造成内存溢出。
此时使用软引用可以解决这个问题。
设计思路是:用一个HashMap来保存图片的路径和相应图片对象关联的软引用之间的映射关系，在内存不足时，JVM会自动回收这些缓存图片对象所占用的空间，从而有效地避免了OOM的问题。
Map<String, SoftReference<Bitmap>>imageCache = new HashMap<String, SoftReference<Bitmap>>();

![image-20220322170830770](D:\JavaProject\面试题总结Images\image-20220322170830770.png)

## 请谈谈你对OOM的认识

- java.lang.StackOverflowError
- java.lang.OutOfMemoryError: Java heap space
- java.lang.OutOfMemoryError: Gc overhead limit exceeded
- java.lang.OutOfMemoryError: Direct buffer memory
- java.lang.OutOfMemoryError: unable to create new native thread
- java.lang.OutOfMemoryError: Metaspace

### Gc overhead limit exceeded：

GC回收时间过长时会抛出outOfNemroyError。过长的定义是，超过98%的时间用来做GC并且回收了不到2%的堆内存连续多次GC都只回收了不到%的极端情况下才会抛出。假如不抛出 GC overhead limit错误会发生什么情况呢?那就是GC清理的这么点内存很快会再次填满，迫使Gc再次执行．这样就形成恶性循环，
CPU使用率一直是100%，而GC却没有任何成果

### Direct buffer memory：

元空间的本质和永久代类似，都是对vM规范中方法区的实现。不过元空间与永久代之间最大的区别在于:
元空间并不在虚拟机中，而是使用本地内存。
因此，默认情况下，**元空间的大小仅受本地内存限制**

写NIO程序经常使用ByteBuffer来读取或者写入数据，这是一种基于通道(Channel)与缓冲区(Buffer)的I/0方式，
它可以使用Native函数库直接分配堆外内存，然后通过一个存储在Java堆里面的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在Java堆和Native堆中来回复制数据。
ByteBuffer.allocate(capability)第一种方式是分配JVM堆内存，属于GC管辖范围，由于需要拷贝所以速度相对较慢
ByteBuffer.allocteDirect(capability)第一种方式是分配OS本地内存，不属于Gc管辖范围，由于不需要内存拷贝所以速度相对较快。但如果不断分配本地内存，堆内存很少使用，那么JVM就不需要执行GC，DirectByteBuffer对象们就不会被回收，
这时候堆内存充足，但本地内存可能已经使用光了，再次尝试分配本地内存就会出现OutOfMlemoryError，那程序就直接崩溃了。

### unable to create new native thread：

高并发请求服务器时,经常出现如下异常: java.Lang.OutOfMemoryError: unable to create new native thread准确的讲该native thread异常与对应的平台有关
导致原因:
你的应用创建了太多线程了，一个应用进程创建多个线程,超过系统承载极限
你的服务器并不允许你的应用程序创建这么多线程, Linux系统默认允许单个进程可以创建的线程数是1024个,你的应用创建线程超过这个数量,就会报java.Lang.OutOfNemoryError: unable to create new native thread
解次办法:
1.想办法降低你应用程序创建线程的数量,分析应用是否真的需要创建这么多线程,如果不是,改代码将线程数降到最低
2.对于有的应用,确实需要创建很多线程,远超过Linux系统的**默认1024个线程**的限制,可以通过修改Linux服务器配置,扩大Linux默认限制

![image-20220322181355968](D:\JavaProject\面试题总结Images\image-20220322181355968.png)

![image-20220322181630642](D:\JavaProject\面试题总结Images\image-20220322181630642.png)

![image-20220322181651036](D:\JavaProject\面试题总结Images\image-20220322181651036.png)

### Metaspace：

JVM参数
-XX : Metaspacesize=8m -XX:MaxMetaspacesize=8m
java 8及之后的版本使用Metaspace来替代永久代。
Metaspace是方法区在Hotspot中的实现，它与持久代最大的区别在于:Netaspace并不在虚拟机内存中而是使用本地内存l即在java8中,classe metadata(the virtual machines internal presentation of Java class),被存储在叫做Metaspace l的native memory
认久代(java8后被原空间Metaspace取代了）存放了以下信息:
虚拟机加载的类信息
常量池
静态变量
即时编译后的代码
模拟Netaspace空间溢出，我们不断生成类往元空间灌，类占据的空间总是会超过Netaspace指定的空间大小的

## Gc垃圾回收算法和垃圾收集器的关系?分别是什么请你谈谈

![image-20220322194018011](D:\JavaProject\面试题总结Images\image-20220322194018011.png)

UseSerialGC，UseParallelGC，UseConcMarkSweepGC，UseParNewGC，UseParallelOldGC，UseG1GC，UserZGC

![image-20220322211954315](D:\JavaProject\面试题总结Images\image-20220322211954315.png)

![image-20220322212532048](D:\JavaProject\面试题总结Images\image-20220322212532048.png)

### 怎么查看服务器默认的垃圾收集器是那个?

```
java -XX:+PrintCommandLineFlags  //可以查看垃圾回收器
```

### 生产上如何配置垃圾收集器的?

- 单CPU或小内存，单机程序
  - -XX:+UseSerialGC

- 多CPU，需要最大吞吐量，如后台计算型应用
  - -XX:+UseParallelGC或者
  - -XX:+UseParallelOldGC

- 多CPU，追求低停顿时间，需快速响应如互联网应用
  - -XX:+UseConcMarkSweepGC
  - -XX:+ParNewGC

### 谈谈你对垃圾收集器的理解?

| 参数                                    | 新生代垃级收集器          | 新生代算法                             | 老年代垃圾收集器             | 老年代算法                              |
| --------------------------------------- | ------------------------- | -------------------------------------- | ---------------------------- | --------------------------------------- |
| -X×:+UseSerialGc                        | SerialGc                  | 复制                                   | SerialOldGc                  | 标整                                    |
| -XX:+UseParNewGc                        | ParNew                    | 复制                                   | SerialOldGc                  | 标整                                    |
| -X×:+UseParallelGC/-XX:+UsePrallelOldGc | Parallel[Scavenge]        | 复制                                   | Parallel Old                 | 标整                                    |
| -XX:+UseConcMarkSweepcc                 | ParNew                    | 复制                                   | CMS + Serial Old的收集器组合 | 标清(Serial Old作为CMS出错的后备收集器) |
| -XX:+UseG1GC                            | G1整体上采用标记-整理算法 | 局部是通过复制算法，不会产生内存碎片。 |                              |                                         |

## G1垃圾收集器

![image-20220323101156582](D:\JavaProject\面试题总结Images\image-20220323101156582.png)

-XX:UseG1GC

### 以前收集器的原则

- 年轻代和老年代是各自独立且连续的内存块;
- 年轻代收集使用单edenSo+s进行复制算法;
- 老年代收集必须扫描整个老年代区域;
- 都是以尽可能少而快速地执行Gc为设计原则。

### G1 (Garbage-First）收集器，是一款面向服务端应用的收集器

从官网的描述中，我们知道G1是一种服务器端的垃圾收集器，应用在多处理器和大容量内存环境中，在实现高吞吐量的同时，尽可能的满足垃圾收集暂停时间的要求。另外，它还具有以下特性:

- 像CMS收集器一样，能与应用程序线程并发执行。
- 整理空闲空间更快。
- 需要更多的时间来预测GC停顿时间。
- 不希望牺牲大量的吞吐性能。
- 不需要更大的Java Heap。

G1收集器的设计目标是取代CMS收集器，它同CMS相比，在以下方面表现的更出色:G1是一个有整理内存过程的垃圾收集器，不会产生很多内存碎片。
G1的Stop The World(STW)更可控，G1在停顿时间上添加了预测机制，用户可以指定期望停顿时间。

CMS垃圾收集器虽然减少了暂停应用程序的运行时间，但是它还是存在着内存碎片问题。于是，为了去除内存碎片问题，同时又保留CMS垃圾收集器低暂停时间的优点，JAVA7发布了一个新的垃圾收集器-G1垃圾收集器。
G1是在2012年才在jdk1.7u4中可用。oracle官方计划在jdk9中将G1变成默认的垃圾收集器以替代CMS。它是一款面向服务端应用的收集器，主要应用在多CPU和大内存服务器环境下，极大的减少垃圾收集的停顿时间，全面提升服务器的性能，逐步替换java8以前的CMS收集器。
主要改变是Eden，**Survivor和Tenured等内存区域不再是连续的了，而是变成了一个个大小一样的region ,每个region从1M到32M不等**。一个region有可能属于Eden，Survivor或者Tenured内存区域。

- 1:G1能充分利用多CPU、多核环境硬件优势，尽量缩短STw。
- 2:G1整体上采用标记-整理算法，局部是通过复制算法，不会产生内存碎片。
- 3:宏观上看G1之中不再区分年轻代和老年代。把内存划分成多个独立的子区域(Region)，可以近似理解为一个围棋的棋盘。
- 4:G1收集器里面讲整个的内存区都混合在一起了，但其本身依然在小范围内要进行年轻代和老年代的区分，保留了新生代和老年代，但它们不再是物理隔离的，而是一部分Region的集合且不需要Region是连续的，也就是说依然会采用不同的GC方式来处理不同的区域。
- 5:G1虽然也是分代收集器，但整个内存分区不存在物理上的年轻代与老年代的区别，也不需要完全独立的survivor(to space)堆做复制准备。G1只有逻辑上的分代概念，或者说每个分区都可能随G1的运行在不同代之间前后切换;

![image-20220323102710504](D:\JavaProject\面试题总结Images\image-20220323102710504.png)

- 算法将堆划分为若干个区域(Region） ，它仍然属于分代收集器
  这些Region的一部分包含新生代，新生代的垃圾收集依然采用暂停所有应用线程的方式，将存活对象拷贝到老年代或者Survivor空间。
  这些Region的一部分包含老年代，G1收集器通过将对象从一个区域复制到另外一个区域，完成了清理工作。这就意味着，在正常的处理过程中，G1完成了堆的压缩（至少是部分堆的压缩），这样也就不会有
  cMS内存碎片问题的存在了。
- 在G1中，还有一种特殊的区域，Humongous(巨大的)区域
  如果一个对象占用的空间超过了分区容量50%以上，G1收集器就认为这是一个巨型对象。这些巨型对象默认直接会被分配在年老代，但是如果它是一个短期存在的巨型对象，就会对垃圾收集器造成负面影响。为了解决这个问题，G1划分了一个Humongous区，它用来专门存放巨型对象。如果一个H区装不下一个巨型对象，那么G1会寻找连续的H分区来存储。为了能找到连续的H区，有时候不得不启动Full GC。

![image-20220323103057471](D:\JavaProject\面试题总结Images\image-20220323103057471.png)

开发人员仅仅需要声明以下参数即可:
**三步归纳:开始G1+设置最大内存↓设置最大停顿时间**

- -XX:+UseG1Gc-Xmx32g
- -XX:MaxGCPauseMillis=100
- -XX:MaxGCPauseMillis=n:最大GC停顿时间单位毫秒，这是个软目标，JVM将尽可能（但不保证停顿小于这个时间



## ZGC垃圾收集器

### 1. ZGC简介和性能

G1的目标是在可控的停顿时间内完成[垃圾回收](https://so.csdn.net/so/search?q=垃圾回收&spm=1001.2101.3001.7020)，所以进行了分区设计，在回收时采用部分内存回收（在YGC时会回收所有新生代分区，在混合回收时会回收所有的新生代分区和部分老生代分区），支持的内存也可以达到几十个GB或者上百个GB。为了进行部分回收，G1实现了RSet管理对象的引用关系。基于G1设计上的特点，导致存在以下问题：

1. 停顿时间过长，通常G1的停顿时间要达到几十到几百毫秒；这个数字其实已经非常小了，但是我们知道垃圾回收发生导致应用程序在这几十或者几百毫秒中不能提供服务，在某些场景中，特别是对用户体验有较高要求的情况下不能满足实际需求。
2. 内存利用率不高，通常引用关系的处理需要额外消耗内存，一般占整个内存的1%~20%左右。
3. 支持的内存空间有限，不适用于超大内存的系统，特别是在内存容量高于100GB的系统中，会因内存过大而导致停顿时间增长。

ZGC作为新一代的垃圾回收器，在设计之初就定义了三大目标：

1. 支持TB级内存

2. 停顿时间控制在10ms之内

3. 对程序吞吐量影响小于15%。

   

实际上目前ZGC已经满足设计之初定义的目标，最大支持4TB堆空间，依据实际测试的情况来看，停顿时间通常都在10ms以下，并且垃圾回收所引起的暂停时间并不会随着[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)的增大而延长。

ZGC如何设计以达成目标？
简单地说，就是ZGC把一切能并发处理的工作都并发执行。
ZGC是在G1的基础上发展起来的，我们知道G1中实现了并发标记，所以标记已经不会再影响停顿时间了。

G1中的停顿时间主要来自垃圾回收（YGC和混合回收）阶段中的复制算法，在复制算法中，需要把对象转移到新的空间中，并且更新其他对象到这个对象的引用。实际中对象的转移涉及内存的分配和对象成员变量的复制，而对象成员变量的复制是非常耗时的。

在G1中对象的转移都是在STW中并行执行的，而ZGC就是把对象的转移也并发执行，从而满足停顿时间在10ms以下。

我们看到G1只有在MARKING的时候，是并发的。而ZGC 在对象的复制和压缩，复制集的选择，等很多方面都改成了并发（和应用线程同时进行）。这就是它STW时间如此之短的秘诀。

![img](https://img-blog.csdnimg.cn/img_convert/eed7db027a2ccea2eec49b997f4a2041.png)

在最新的JDK版本中，后面2项没有打钩的，ZGC也把他们优化进并发步骤中，根据他们最新的测试，STW最大停顿时间可以缩小到1MS。

上图左边也展示了一些ZGC里的经典特性。比如说用到了读屏障，颜色指针。是个单代的垃圾收集器（不区分年轻代和老年代, 在JVMLS 2018上，ZGC的领队Per大大明确表示目前的ZGC没有分代只是为了实现简单，目前正在考虑给ZGC添加分代支持或者是添加一个Thread-Local GC来起到类似Young GC的作用，还在探索中），是个部分压缩的算法（和G1类似，有解决内存碎片的标记整理步骤）。立即的内存重用，和NUMA友好的特性。

我们会在下文中或多或少的展开介绍这些特点。

在JDK11中的ZGC只支持 Linux/x86_64

![img](https://img-blog.csdnimg.cn/img_convert/f7000bd1a4b49ab868403f5e92de6026.png)

现在ZGC的目标是1MS，原来ZGC的停顿时间和堆SIZE无关，但是和ROOT-SET的SIZE有关。他们希望实现并发的线程栈扫描，这样可以使得ZGC的停顿时间和ROOT-SET SIZE也无关。可以把性能提升到1MS的效果。大概和下图这么牛逼

![img](https://img-blog.csdnimg.cn/img_convert/addaf90f8012247d2fb6074167969016.png)

当然我们再来看看ZGC和G1比有多牛逼。

![img](https://img-blog.csdnimg.cn/img_convert/da81261a96a6b6c44bf18ffae40a9b48.png)

![img](https://img-blog.csdnimg.cn/img_convert/d73e761175da244fb684d8257013adb8.png)

在缩短延迟的同时，吞吐率也不受影响。

![img](https://img-blog.csdnimg.cn/img_convert/2a31779c25c4566f8ccc5c51870ea5d6.png)

![img](https://img-blog.csdnimg.cn/img_convert/f441c0d2d535b7f027d2026b7aa870ca.png)

### 2. ZGC流程介绍

并发垃圾回收算法实际上是以复制算法为基础，增加了并发处理。我们先回顾一下复制算法，它可以概括为3个阶段，分别为标记（mark）、转移（relocate）和重定位（remap）。这3个阶段分别完成的功能是：

- 标记：从根集合出发，标记活跃对象；此时内存中存在活跃对象和已死亡对象。
- 转移：把活跃对象转移（复制）到新的内存上，原来的内存空间可以回收。
- 重定位：因为对象的内存地址发生了变化，所以所有指向对象老地址的指针都要调整到对象新的地址上。

从细节角度可以分为如下步骤

![img](https://img-blog.csdnimg.cn/img_convert/7f4612de7ea5b7325fc5d0346fade0c5.png)

1. 初始标记，从根集合出发，找出根集合直接引用的活跃对象，并入栈；该步需要STW。
2. 并发标记，根据初始标记找到的根对象，使用深度优先遍历对象的成员变量进行标记；并发标记需要解决标记过程中引用关系变化导致的漏标记问题
3. 再标记和非强根并行标记，在并发标记结束后尝试终结标记动作，理论上并发标记结束后所有待标记的对象会全部完成，但是因为GC工作线程和应用程序线程是并发运行，所以可能存在GC工作线程执行结束标记时，应用程序线程又有新的引用关系变化导致漏标记，所以这一步先判断是否真的结束了对象的标记，如果没有结束就还会启动并行标记，所以这一步需要STW。另外，在该步中，还会对非强根（软应用，虚引用等）进行并行标记。
4. 并发处理非强引用和非强根并发标记
5. 重置转移集合中的页面，**实际上第一次垃圾回收时无须处理这一步。**
6. 回收无效的页面，**实际上在内存充足的情况下不会触发这一步。**
7. 并发选择对象的转移集合，转移集合中就是待回收的页面。
8. 并发初始化转移集合中的每个页面，在后续重定位（也称为Remap）时需要的对象转移表（Forward Table）就是在这一步初始化的。
9. 转移根对象引用的对象，该步需要STW。
10. 并发转移，把对象移动到新的页面中，这样对象所在的老的页面中所有活跃对象都被转移了，页面就可以被回收重用。

![img](https://img-blog.csdnimg.cn/img_convert/a9c90f91a377475410503c14101fb41a.png)
为了画图方便，把步骤5）~步骤8）放在一个并发步骤中，实际中这是4步，并且这4步是串行执行，每一步都是并发执行的。

上述步骤，你可能会看晕。我们来看简单的版本。

![img](https://img-blog.csdnimg.cn/img_convert/00dbad29f3b7cb4af475a8df815537e7.png)

上述3个蓝线代表需要STW， 灰线代表可以并发运作。

1. 根集合标记（STW）
2. 并发标记
3. 并发标记的同步点（STW，同G1）还会处理一些非强根
4. 并发-转移前的准备（上面的4-8步，最核心的是引用处理，非强根清理，转移集(relocation set)的选择）
5. 转移 在转移集中的根对象（STW）
6. 并发转移其他在转移集中的对象

并发算法中明确地提到重定位阶段，但上面的步骤中并没有体现。在ZGC中并没有明确这一步，重定位实际上被合并到标记阶段中，即在标记的时候如果发现对象引用到老的地址，这时会先完成重定位更新对象的引用关系，然后再标记对象。所以实质上ZGC的并发垃圾回收中还是包含了重定位这一阶段，只不过重定位和标记阶段复用了。

![img](https://img-blog.csdnimg.cn/img_convert/a57b11eb0c9985f34fc590b079cf04d1.png)

 

思考题. 有3个步骤需要STW的原因是什么？他们和应用线程并发有什么问题？

第二个STW，如果你看过我的G1的文章，就很好理解。如果不STW，那么应用程序一直在改引用，会找不到一个时间点，把待标记对象在QUEUE中给全部排干净。那么会一直处于并发标记阶段。
第三个STW，在初始转移中所做的工作主要针对根集合引用的对象，如果这些对象所在的页面在转移集中，则转移这些对象；如果对象所在的页面不在转移集中，则直接调整对象的页面映射视图。第10步中的并发标记是对所有在转移集的页面中所有活跃对象做转移，在并发转移之后的下一次垃圾回收的标记阶段完成重定位。那么我们能不能把第9步的工作分散在第10步和下一次垃圾回收的标记阶段进行？
要回答这个问题，需要理解读屏障，和对ZGC的全貌有一个认识，我会放在文章最后解答。
关于第一个STW，也要对ZGC的工作原理有一定的理解。我会在讲完（颜色指针怎么工作后解答这个问题）

### 3. ZGC堆的内存布局

ZGC 支持的最大堆内存为4T。 但是我们确需要保留更大的虚拟地址空间。如下图

![img](https://img-blog.csdnimg.cn/img_convert/d26322dbbc6d8cd265e33f4665c927f7.png)

因为ZGC里需要用到3个地址视图。
分别是Marked0、Marked1和Remapped，而且有趣的是这3个视图会映射到操作系统的同一物理地址。这就是ZGC中Color Pointers的概念，通过Color Pointers来区别不同的虚拟视图。
在ZGC中常见的几个虚拟空间有[0 ~ 4TB)、[4TB ~ 8TB)、[8TB ~ 12TB)和[16TB ~ 20TB)。其中[0 ~ 4TB)对应的是Java的堆空间；[4TB ~ 8TB)、[8TB ~ 12TB)和[16TB ~ 20TB)分别对应Marked0、Marked1和Remapped这3个视图。这几个区域有什么关系？我们先看图
mutator 就是应用线程的意思

![img](https://img-blog.csdnimg.cn/img_convert/c4a8010365235a053eaeb5f25aa7c6a6.png)
0~4TB的虚拟地址是ZGC提供给应用程序使用的虚拟空间，它并不会映射到真正的物理地址。

- 操作系统管理的虚拟内存为Marked0、Marked1和Remapped这3个空间，且它们对应同一物理空间。
- 在ZGC中这3个空间在同一时间点有且仅有一个空间有效。为什么这么设计？这是利用虚拟空间换时间；这3个空间的切换是由垃圾回收的不同阶段触发的。（下文介绍颜色指针会介绍）

应用程序可见并使用的虚拟地址为0~4TB，经ZGC转化，真正使用的虚拟地址为[4TB ~ 8TB)、[8TB ~ 12TB)和[16TB ~ 20TB)，操作系统管理的虚拟地址也是[4TB ~ 8TB)、[8TB ~ 12TB)和[16TB ~ 20TB)。应用程序可见的虚拟地址[0 ~ 4TB)和物理内存直接的关联由ZGC来管理。

![img](https://img-blog.csdnimg.cn/img_convert/f71299a03de52affdc4992cff463ecd8.png)

为了细粒度地控制内存的分配，和G1一样，ZGC将内存划分成小的分区，在ZGC中称为页面（page）。ZGC支持3种页面，分别为小页面、中页面和大页面。其中小页面指的是2MB的页面空间，中页面指32MB的页面空间，大页面指受操作系统控制的大页。我们先回顾一下操作系统所支持的大页。

标准大页（huge page）是Linux Kernel 2.6引入的，目的是通过使用大页内存来取代传统的4KB内存页面，以适应越来越大的系统内存，让操作系统可以支持现代硬件架构的大页面容量功能。它有两种大小：2MB和1GB。2MB页块大小适合用于吉字节级的内存，1GB页块大小适合用于太字节级别的内存；2MB是默认的大页尺寸。

一个ZGC的页面可能由几个不连续的操作系统页面组成。

![img](https://img-blog.csdnimg.cn/img_convert/7bd72815d984b9962b23a2fd0f7eeb3c.png)

ZGC对于不同页面回收的策略也不同。简单地说，小页面优先回收；中页面和大页面则尽量不回收。

![img](https://img-blog.csdnimg.cn/img_convert/4539a52e012d4b33b09019f673dd366a.png)

### 4. ZGC对NUMA支持

在过去，对于X86架构的计算机，内存控制器还没有整合进CPU，所有对内存的访问都需要通过北桥芯片来完成。X86系统中的所有内存都可以通过CPU进行同等访问。任何CPU访问任何内存的速度是一致的，不必考虑不同内存地址之间的差异，这称为“统一内存访问”（Uniform Memory Access，UMA）。UMA系统的架构示意图如图所示。

![img](https://img-blog.csdnimg.cn/img_convert/dbc99d2221750ccc221bb044287b11d1.png)

在UMA中，各处理器与内存单元通过互联总线进行连接，各个CPU之间没有主从关系。之后的X86平台经历了一场从“拼频率”到“拼核心数”的转变，越来越多的核心被尽可能地塞进了同一块芯片上，各个核心对于内存带宽的争抢访问成为瓶颈，所以人们希望能够把CPU和内存集成在一个单元上（称为Socket），这就是非统一内存访问（Non-Uniform Memory Access，NUMA）。很明显，在NUMA下，CPU访问本地存储器的速度比访问非本地存储器快一些。下图所示是初期处理器架构示意图。

![img](https://img-blog.csdnimg.cn/img_convert/7231cc14b4f13219a80adc95d555eb60.png)

ZGC是支持NUMA的，在进行小页面分配时会优先从本地内存分配，当不能分配时才会从远端的内存分配。对于中页面和大页面的分配，ZGC并没有要求从本地内存分配，而是直接交给操作系统，由操作系统找到一块能满足ZGC页面的空间。
ZGC这样设计的目的在于，对于小页面，存放的都是小对象，从本地内存分配速度很快，且不会造成内存使用的不平衡，而中页面和大页面因为需要的空间大，如果也优先从本地内存分配，极易造成内存使用不均衡，反而影响性能。

### 5. 颜色指针在ZGC中的运用

![img](https://img-blog.csdnimg.cn/img_convert/c2d02b4e7b823d208f4b2237f795e42d.png)

颜色指针可以说是ZGC的核心概念。因为他在指针中借了几个位出来做事情，所以它必须要求在64位的机器上才可以工作。并且因为要求64位的指针，也就不能支持压缩指针。（关于JVM的压缩指针，可以自行百度其他文章）

ZGC支持64位系统，我们看一下ZGC是如何使用64位地址的。ZGC中低42位（第0 ~ 41位）用于描述真正的虚拟地址（这就是上面提到的应用程序可以使用的堆空间），接着的4位（第42 ~ 45位）用于描述元数据，其实就是大家所说的Color Pointers，还有1位（第46位）目前暂时没有使用，最高17位（第47~63位）固定为0

![img](https://img-blog.csdnimg.cn/img_convert/93d6dac286183a5bbee867a1302b8332.png)

![img](https://img-blog.csdnimg.cn/img_convert/53e3626fd455d1eb549da682d8f18988.png)

由于42位地址最大的寻址空间就是4TB，这就是ZGC一直宣称自己最大支持4TB内存的原因。这里还有视图的概念，Marked0、Marked1和Remapped就是3个视图，分别将第42、43、44位设置为1，就表示采用对应的视图。在ZGC中，这4位标记位的目的并不是用于地址寻址的，而是为了区分Marked0、Marked1和Remapped这3个视图。当然对于操作系统来说，这4位标记位代表了不同的虚拟地址空间，而这些不同标记位指示的不同虚拟空间通过mmap映射在同一物理地址；颜色指针能够快速实现并发标记、转移和重定位。

> **为什么最高位16个不能用？**
>
> 由于X86_64处理器硬件的限制，目前X86_64处理器地址线只有48条，也就是说64位系统支持的地址空间为256TB。为什么处理器的指令集是64位的，但是硬件仅支持48位的地址？最主要的原因是成本问题，即便到目前为止由48位地址访问的256TB的内存空间也是非常巨大的，也没有多少系统有这么大的内存，所以在设计CPU时仅仅支持48位地址，可以少用很多硬件

#### 5.1 ZGC基于颜色指针的并发处理算法

ZGC初始化之后，整个内存空间的地址视图被设置为Remapped，当进入标记阶段时的视图转变为Marked0（也称为M0）或者Marked1（也称为M1），从标记阶段结束进入转移阶段时的视图再次设置为Remapped。ZGC通过视图的切换加上SATB算法实现并发处理。具体算法如下。
1．初始化阶段
在ZGC初始化之后，此时地址视图为Remapped，程序正常运行，在内存中分配对象，满足一定条件后垃圾回收启动
2．标记阶段
第一次进入标记阶段时视图为M0，在标记阶段，应用程序和标记线程并发执行，那么对象的访问可能来自标记线程和应用程序线程。

- 标记线程：它从根集合开始标记对象，在标记前先判断对象的地址视图，如果发现对象的地址视图是M0，说明对象是在进入标记阶段之后新分配的对象或者对象已经完成了标记（对象活跃），无须处理。如果发现对象的地址视图是Remapped，说明对象是前一阶段分配的，而且通过根集合可达，所以把对象的地址视图从Remapped调整为M0。**（M0表示活跃）**
- 应用程序线程如果创建新的对象，则对象的地址视图为M0。
  如果应用程序线程访问对象并且对象的地址视图是Remapped，说明对象是前一阶段分配的，按照SATB的算法，只要把该对象的视图调整为M0就能防止对象漏标。**只标记应用线程访问到的对象还不够，实际上还需要把对象的成员变量所引用的对象都进行递归标记**。如果应用线程访问对象地址视图是M0，说明对象是在进入标记阶段之后新分配的对象或者对象已经完成了标记，无须额外处理，直接访问。

所以，在标记阶段结束之后，对象的地址视图要么是M0（活跃），要么是Remapped（垃圾）。**这里的虚拟地址虽然不一样，但是指向的是物理内存的同一个区域**
所有标记为M0的对象放入活跃信息表

3．并发转移阶段
标记结束后就进入转移阶段，此时地址视图再次被设置为Remapped。

转移阶段会把活跃对象转移到新的内存中，并回收对象转移前的内存空间。在转移阶段，应用程序和标记线程并发执行，那么对象的访问可能来自转移线程和应用程序线程。

- 转移线程：转移线程仅仅根据活跃对象进行转移。当转移线程访问对象时：
  如果对象在对象活跃信息表中并且视图为M0，则转移对象，并且视图从M0调整为Remapped。
  如果对象在对象活跃信息表中并且视图Remapped，说明对象已经被转移，无须处理。
- 应用程序线程如果创建新的对象，则对象的地址视图为Remapped。
  如果应用线程访问对象且不在活跃信息表中，则说明是新创建的或者对象无须转移，无须处理。
  如果应用线程访问对象且在活跃信息表中且视图为Remapped，说明对象已经被转移，无须处理。
  如果应用程序线程访问在对象活跃信息表中，且视图为M0，说明对象是标记阶段标记的活跃对象，所以需要转移对象
  在对象转移以后，对象的地址视图从M0调整为Remapped；
  **注意，只把应用线程读到的对象进行转移还不够，实际上还需要把对象的成员变量所引用的对象都进行转移，ZGC对这一实现做了优化，由转移线程完成对象成员变量的转移。**
  至此，ZGC的一个垃圾回收周期中，并发标记和并发转移就结束了。

我们提到在标记阶段存在两个地址视图M0和M1，上面的算法过程显示只用到了一个地址视图，为什么设计成两个？简单地说是为了区别前一次标记和当前标记。

第一次垃圾回收时地址视图为M0，假设标记了两个对象ObjA和ObjB，说明ObjA和ObjB都是活跃的，它们的地址视图都是M0。在转移阶段，ZGC是按照页面进行部分内存垃圾回收的，也就是说当对象所在的页面需要回收时，页面里面的对象需要被转移，如果页面不需要转移，页面里面的对象也就不需要转移。

假设ObjA所在的页面被回收，ObjB所在的页面在这一次垃圾回收中不会被回收。ObjA被转移后，它的地址视图从M0调整为Remapped，ObjB不会被转移，ObjB的地址视图仍然为M0。

那么下一次垃圾回收标记阶段开始的时候，存在两种地址视图的对象

1. 地址视图为Remapped的对象，说明该对象在并发转移阶段被转移或者被访问过；
2. 地址视图为M0的对象，说明该对象在前一次垃圾回收的标记阶段已经被标记。

**如果本次垃圾回收标记阶段仍然使用M0这个地址视图，那么就不能区分出对象是活跃的，还是上一次垃圾回收标记过的。**

所以新标记阶段使用了另外一个地址视图M1，则标记结束后所有活跃对象的地址视图都为M1。
此时这3个地址视图代表的含义是：

- M1：本次垃圾回收中识别的活跃对象。
- M0：前一次垃圾回收的标记阶段被标记过的活跃对象，对象在转移阶段未被转移，但是在本次垃圾回收中被识别为不活跃对象。
- Remapped：前一次垃圾回收的转移阶段发生转移的对象或者是被应用程序线程访问的对象，但是在本次垃圾回收中被识别为不活跃对象。

#### 5.2 上述过程算法演示

![img](https://img-blog.csdnimg.cn/img_convert/5ba234a4a845bf41f6a0edddeef52ec8.png)

 

![img](https://img-blog.csdnimg.cn/img_convert/f25121f11075f286e67e77575267d512.png)

 

 

![img](https://img-blog.csdnimg.cn/img_convert/0d592591038abe95ef813b29c849a112.png)

 

![img](https://img-blog.csdnimg.cn/img_convert/d9c09469feea167ab4e96f4661d69925.png)

### 6. 读屏障在ZGC中的运用

![img](https://img-blog.csdnimg.cn/img_convert/56e54b8da1895f96a32073edc9bd1462.png)

 

![img](https://img-blog.csdnimg.cn/img_convert/54d6b4bddc250c6c2b348860598fae0b.png)

 

![img](https://img-blog.csdnimg.cn/img_convert/b2a86f6eb13e93a75153566923bc0b0a.png)

 

![img](https://img-blog.csdnimg.cn/img_convert/daec56fecb2751e61ec868cff0e96647.png)

对应用程序线程进行标记发生在读对象时，为了触发标记动作可以设计一个读屏障，在字节码层面或者编译代码层面给读操作增加一个额外的处理即可。

读屏障由读命令触发，JVM有3种运行状态：解释执行、C1优化执行和C2优化执行。不同的运行状态，读屏障的触发代码略有不同，但它们使用的读屏障是完全一样的。

我们从最简单的解释执行看一下读屏障的实现。读屏障在解释执行时通过load相关的字节码指令加载数据，我们直接从堆空间中加载对象的地方了解一下读屏障，其代码如下：

```cpp
template <DecoratorSet decorators, typename BarrierSetT>



template <typename T>



inline oop ZBarrierSet::AccessBarrier<decorators,BarrierSetT>::oop_load_in_heap(T* addr) {



  verify_decorators_absent<ON_UNKNOWN_OOP_REF>();



  const oop o = Raw::oop_load_in_heap(addr);



  return load_barrier_on_oop_field_preloaded(addr, o);



}
```

```cpp
//
// Load barrier
//
uintptr_t ZBarrier::load_barrier_on_oop_slow_path(uintptr_t addr) {
  return relocate_or_mark(addr);
}
uintptr_t ZBarrier::relocate_or_mark(uintptr_t addr) {
  return during_relocate() ? relocate(addr) : mark<Follow, Strong, Publish>(addr);
    //判断是否在重定位期间，如果在重定位期间，则重定位当前地址，将新地址传递过去，如果不在重定位期间，则通过标记表查找标记
}
void ZBarrier::load_barrier_on_oop_fields(oop o) {
  assert(ZAddress::is_good(ZOop::to_address(o)), "Should be good");
  //当前地址必须是强引用？
  ZLoadBarrierOopClosure cl;
  o->oop_iterate(&cl);
}
```



这里调用的load_barrier_on_oop_field_preloaded就是读屏障，在对象加载完成后做额外的处理。

![img](https://img-blog.csdnimg.cn/img_convert/e1292ca6a4e7d2ca5a2a772359412d81.png)

> 读屏障增加了额外的代码，所以会引起性能下降。据Per Linda的介绍，SPECjbb测试表明使用读屏障之后，性能大概降低4%左右。

![img](https://img-blog.csdnimg.cn/img_convert/3dac3dba997dba94970e3a4e97bf37eb.png)
上面提及了标记的一些基本概念，STW的时候是并行的（多个标记线程一起标记ROOT直接引用的对象），之后是并发的。读屏障可以让应用线程再使用对象的时候，知道它是不是一个GOOD COLOR的对象，如果不是。就先要帮他恢复成GOOD COLOR。
**如果在标记的时刻，地址视图为M0，发现他不是M0，那么应用线程会帮助把它（如果需要转移还会帮助转移）标记为M0，因为我可以访问到它所以它是活的。**
上述的做法通过任意一次读操作，都把颜色置为灰色（活跃），从而打破并发标记中漏标的充要条件（充要条件参考我的[G1 详解](https://www.jianshu.com/p/cc6b98b1640e)）

**回答思考题3**

为什么初始标记需要STW？
假如我们不STW，那么应用程序和标记线程并发。CPU可以在任意位置切换。假设现在是初始状态，地址视图为REMAPPED。应用线程从栈中的引用读到一个堆中的对象OBJ_A。并且要把它的引用给断开。

```python
local1.next = obj_a
```

按照STAB的设计，如果此时已经开始并发标记了，那么OBJ_A会标记为M0。但是此时还没开始。所以应用程序把OBj_A读出来。准备给LOCAL1.NEXT赋值前，切换到了标记线程。
标记线程启动，把地址视图改为M0。然后从LOCAL1这个跟开始扫描。发现LOCAL1.NEXT=NULL。那么把LOCAL1标记为M0就结束了。

切回到应用线程，此时继续完成赋值任务。就造成了一个黑色对象指向了一个白色对象的漏标问题。白色对象会被GC掉之后，程序会出错。

为了解决这个问题，程序会利用STW的安全点，来防止这种应用线程做到一半的操作被切走的情况。

### 7. strip(条带）在ZGC中的运用

ZGC中引入了标记条带（mark strip）。为了让线程之间标记的时候可以互不干扰，减少竞争锁的开销。

![img](https://img-blog.csdnimg.cn/img_convert/805caabc59cc2a615545efc371e40f38.png)

标记栈在ZGC中的底层实现使用的是数组，因为标记栈只有一个，所有的并发标记线程会访问这一个标记栈，所以自然会想到将这个标记栈进行划分，划分后形成多个标记条带，然后让多个并发标记线程并行地访问其中的标记条带，互不干扰。例如，Thread0标记Strip0中的对象，而Strip0可能包含多个内存区域块。示意图如下图所示。

![img](https://img-blog.csdnimg.cn/img_convert/527c2ae9e74c7cbd7101110fc48aaefb.png)

划分成多个标记条带和为并发工作线程设置相应的标记条带需要提前完成，这样才能在标记时把待标记对象直接放入相应的标记条带中，这是第一步开始标记做的工作。

ZGC是根据对象的地址计算对象属于哪个标记条带，把地址通过哈希函数计算得到的值作为标记条带号。

这里其实有一个问题——在对象放入标记条带中存在并发问题。设想这样一种情况，有两个线程T1和T2，有两个标记条带Strip1和Strip2，里面分别存放了Obj1和Obj2。T1标记Strip1里面的对象，T2标记Strip2里面的对象。当T1标记Obj1的时候，需要把Obj1所有的成员变量进行标记，假设发现Obj1的成员变量按照哈希函数计算后需要放入Strip2中，那么T1会访问Strip2，把该对象的成员变量指向的待标记对象入栈。
同理，线程T2也可能访问Strip1。

这和我们前面提到的设计标记条带的目的完全不同。设计标记条带的目的是希望线程T1只访问Strip1，线程T2只访问Strip2，从而完全解决并发性问题。

为了尽可能地让标记线程之间进行并发标记，ZGC对每个标记线程使ZMarkThreadLocalStacks保存需要遍历的对象，当线程的本地标记栈满时，再把标记栈转入ZMarkStripSet中。

![img](https://img-blog.csdnimg.cn/img_convert/e4420cc9cdef9b819af49a2a966d2b0a.png)

ZMarkStripSet中标记条带是链表的形式，所以放入的时候相当简单，直接插入新的节点到链表中，而无须分配额外的空间。

对于并发工作线程来说，所有的标记条带都没有对象，说明并发工作线程可以结束工作。因为并发工作线程是多线程执行，所以判断的条件是所有工作线程访问的条带都没有对象。对于多线程来说，可能存在有些线程待标记对象少，执行得快，而有些线程待标记对象多，执行得慢的情况，所以并发执行中需要有一种机制来进行负载均衡，让所有并发工作线程尽可能同时结束。并发标记工作线程的负载均衡是通过窃取其他线程的任务完成的，即当本线程没有可以执行的任务时，并不会立即停止线程，而是先从其他的线程窃取任务，然后执行任务。

![img](https://img-blog.csdnimg.cn/img_convert/64dff19e8a1e88a16f1084676bf8f72b.png)

因为应用程序线程也可能执行标记，而且应用程序线程标记后，待标记对象存放在应用程序线程的本地标记条带中，所以当并发工作线程结束标记任务后，应该将应用程序线程中的待标记对象转移到并发工作线程的标记条带中，让并发标记线程继续高速工作。因此在并发标记结束过程中设计了主动刷新机制，并发标记0号线程把应用程序线程中的待标记对象刷新到并发线程的标记条带中。继续工作。这就要求应用程序需要能够进入到自己的安全点再去响应主动刷新，对于单个线程走进安全点暂停做别的事情，需要支持HandShake机制。

> JDK 10中引入JEP312 Thread-Local HandShake，该项目实现单个线程的暂停，而不是暂停所有线程。HandShake机制也是通过VMThread机制完成的，只不过HandShake中指定了一个暂停的目标线程。

**并发转移**

![img](https://img-blog.csdnimg.cn/img_convert/1e181e600158b95d9c3cf6bdd3010146.png)

因为是并发转移，所以必然会涉及应用程序线程在转移时访问待转移的对象。此时应用程序线程会先完成转移的任务，然后再访问对象，这就涉及我们之前介绍的读屏障。读屏障的流程图中，会根据页面的状态判断进行转移还是标记或者重定位操作。

**思考题2的解答**

为什么需要初始转移呢？反正有读屏障不能直接并发转移吗？

应用程序线程正在访问对象，在第10步并发转移中，应用程序线程的访问是需要读屏障的，此时在读屏障中会把对象转移到新的地址。
听起来似乎可行，但是这里有一个问题，我们提到如果应用程序线程正在访问对象，通过读屏障完成转移。这个读屏障只能在第10步中发生，所以它针对的都是在第10步中从根集合中新产生的引用对象，这些新的对象可以通过读屏障得到正确的处理（即新产生的对象被正确地转移），但是可能存在这样一种情况，在进入第10步之前已经通过根集合访问了对象，这时进入第10步后，第10步中的读屏障对于这些已经访问的对象就不起作用了，单从转移角度来说，对象仍然可以被并发转移线程正确地转移。但是从访问角度就会出现问题，此时如果有新的应用程序线程也访问这个对象，如果对象已经被转移，那么这个新应用程序线程通过读屏障访问到新对象，如果对象还未转移，那么这个新应用程序线程则会通过读屏障先转移对象再访问，结论就是两个应用程序线程一个访问老对象，一个访问新对象，如果两个应用程序线程都对对象进行修改，就会发生数据不一致，导致错误。**这就是初始转移STW解决的问题。**

### 8. ZGC全流程动画演示

在JVM启动后和垃圾回收发生之前，相关的地址视图会被设置为Remapped。假定应用程序运行一段时间后，整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/e26050d7b802d0b6cb2002ebc7ebf595.png)

图中对象1 ~ 5位于小页面中，对象6 ~ 11位于中页面中。小页面位于虚拟地址的头部，中页面和大页面位于虚拟地址的尾部，本例中假设不存在大页面对象。小页面占用的空间为2MB，中页面占用的空间为32MB

进入初始标记后，地址视图切换为M0，然后从根集合出发，开始遍历直接引用的对象。
为了更好地突出图中的变化，统一定义虚线表示正在变化的引用关系，标记过程中的活跃对象使用深色背景。初始标记结束后，整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/f1fac09d63d61b6c8ea34ed99232963a.png)

对象1、2和4都被标记为活跃的。这里活跃的意思是对象1、2和4的地址视图都变成M0。
同时在初始标记结束后，在标记条带中存在指向对象1、2和4的指针，标记条带将被用于并发标记。假设并发工作线程为两个，对象1、2和4将被放在不同的标记条带中，分别由线程1和线程2并发地标记。

![img](https://img-blog.csdnimg.cn/img_convert/bf0e2141ca193b6734cb81fd5ed54fe1.png)

并发标记时，从标记条带获取对象，开始标记。
注意，在图中初始标记中仅仅把对象1、2和4的地址从Remapped变成了M0，但是并没有记录它们所在页面的活跃对象的信息。
并发标记从对象1、2和4遍历对象的成员变量，同时统计对象1、2和4的信息，这些统计信息放在对象所属的页面中。
这里把这些统计信息称为标记信息，主要包括页面中存活对象的个数，对象经过内存对齐之后占用的内存大小和对象的标记位图信息。

这里没有演示应用程序并发执行的情况，如果应用程序新分配对象，一定是从一个新的页面中分配，因为对象的缓冲页面在初始标记中被清空。

如果是新页面，则不会在本轮垃圾回收中回收，所以在图中没有体现。另外，如果应用程序线程访问待标记的对象，则通过读屏障完成标记，其处理的方法和并发标记中的方法完全一样，图中也没有体现。
**在并发标记结束时，所有页面中的活跃对象都被标记，同时标记条带一定为空。进入再标记阶段**
再标记主要是为了处理因应用程序线程标记对象，导致仍然有待标记的对象。实际上在并发标记结束前会尝试多次主动刷新（和被动刷新，文章中没有介绍）以避免这种状况，但是这种状况不能完全避免，如果需要完全避免，必须在STW中进行。另外，再标记还会处理部分非强根的标记。为了简化，本例中假定所有的对象在并发标记中都标记完成，也没有非强根引用。

非强引用处理之后，将重置转移集。主要是针对前一次垃圾回收过程中产生的对象地址信息表重置。在本例中因为是第一次垃圾回收所以不会涉及重置。

重置转移集之后，将回收无效的页面。如果在页面分配时内存不足，将回收预分配或者缓存页面中的页面对应的物理地址。本例中假定内存充足，不会执行本步。

在并发标记之后，一共有4个页面被标记。在这一步中，会根据这4个页面的统计信息选择——哪些页面可以回收。ZGC只会选择页面中垃圾空间超过页面空间的25%的页面，然后把所有选择到的页面根据页面中垃圾空间大小排序，根据排序结果计算是否存在一些页面在转移后导致新的页面无剩余空间，如果存在，则把这些页面也丢弃，不进行转移。

在本例中，假定有一个小页面和中页面将被转移，所以它们将进入转移集

![img](https://img-blog.csdnimg.cn/img_convert/5c90e86442fc1bce87bd93c62c4a2d10.png)

选择转移集结束后，将对转移集中的页面初始化，初始化最终的动作是初始化对象地址转移信息表。转移信息表将存储转移完成后对象转移前后的地址。初始化转移集结束后，整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/d3fd8fc7ceb3bf31f67e794d8c78fa53.png)

接着将进入初始转移阶段。进入转移阶段后，地址视图再次从M0切换到Remapped。初始转移从根集合出发，遍历对象，对象进行转移或者调整对象的视图。初始转移结束后，整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/0ab08e6a3d5564934b155bc7649c97d1.png)

可以看到对象4所在的页面在转移集中，所以它会被转移到新的页面中。对象4转移之后，会在所在的页面中记录对象转移信息。对象1和对象2不在转移集中，所以它们不会被转移，但是它们的地址视图会从M0调整为Remapped。

初始转移只会针对根集合进行，结束后将进入并发转移，并发转移是根据转移集的页面进行遍历，即只会遍历转移集选择的页面。在遍历时，两个并发工作线程会根据标记信息逐个转移对象。转移过程涉及内存的复制，比较耗时，所以在转移时会把一个页面分成64个段并发的转移。因为对象4已经完成转移，所以并发转移会继续转移剩下的对象5和对象8，转移后分别称为对象5’和对象8’。转移后也会更新对象转移信息表。对象5属于小页面对象，转移后也会在小页面中，对象8属于中页面对象，所以转移中会新分配一个中页面来存储对象8。

![img](https://img-blog.csdnimg.cn/img_convert/255e20df6543fe3f42acd9359913442f.png)

需要注意的是，虽然对象4’和对象5’都在一个新的页面中，但是对象4’在并发转移完成后，指向的还是对象5的地址而不会调整到对象5’。

并发转移之后，转移集中的页面会被立即加入页面缓存供新的页面分配使用。
假设转移之后，应用程序线程在执行过程中从对象4’访问了对象5’，从对象8’访问了对象9，此时会利用读屏障对对象进行重定位，此时整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/a40de5ec0b8b5797f15942e3cf6bcc44.png)

对象4’将根据页面中对象地址转移信息表得到对象5的地址为对象5’，对象5’的地址视图是Remapped，所以直接调整引用关系。对象8’访问对象9，对象9并未被转移，它的地址视图仍然为M0，所以此时会先调整对象9的地址视图从M0到Remapped，然后调整引用关系（指向另一个视图为remapped 的9）。

经过一段时间的运行，因为某种原因再次触发垃圾回收。垃圾回收触发,进入初始标记后，地址视图切换为M1，然后从根集合出发，开始遍历直接引用的对象。初始标记结束后，整个内存的对象关系如图

![img](https://img-blog.csdnimg.cn/img_convert/2b55b0b6f62bb00e288cb05fe3c118c0.png)

在新一轮的并发标记中，从标记条带开始标记。在标记时会对页面的标记信息进行复位。这里还要注意，在标记前，对象10、对象11和其他对象稍有区别，其他对象的地址视图为Remapped，对象10和对象11的地址视图为M0，说明它们在上一轮垃圾回收的标记阶段被识别为活跃对象，但是它们所在的页面没有在转移阶段被转移或者被访问。在标记结束后，所有活跃对象的地址视图都会调整到M1

![img](https://img-blog.csdnimg.cn/img_convert/5ec2562751a6a83c400b2a2c1020ff26.png)

接下来的步骤和我们介绍过的垃圾回收步骤完全相同，也不再介绍。



## JVMGC+ SpringBoot微服务的生产部署和调参优化

实际工作中，如何结合springboot进行调优??
如何落地??
JVMGC—->>调优—->> springboot微服务的生产部署和调参优化

- 1 IDEA开发完微服务工程
- 2 maven进行clean package
- 3要求微服务启动的时候，同时配置我们的JVM/GC的调优参数
  - 3.1 内
  - 3.2外===>重点
- 4公式
  - java -server jvm的各种参数 -jar 第一步上面的jar/war包名字

## 生产环境服务器变慢，诊断思路和性能评估谈谈?

- 整机: top
- CPU: vmstat
- 内存:free
- 硬盘: df
- 磁盘lo: iostat
- 网络lo: ifstat

### top

![image-20220323105431978](D:\JavaProject\面试题总结Images\image-20220323105431978.png)

load average系统平均一分钟，五分钟，十五分钟的系统负载值

如果三个值相加/3 * 100% > 60%,则代表超出负载了

按住S可以查看所有的cpu运行情况。

uptime可以查看精简版的top

### vmstat

**vmstat -n 2 3**
一般vmstat工具的使用是通过两个数字参数来完成的，第一个参数是采样的时间间隔数单位是秒，
第二个参数是采样的次数

- procs
  - r:运行和等待CPU时间片的进程数，原则上1核的CPU的运行队列不要超过2，整个系统的运行队列不能超过总核数的2倍,
    否则代表系统压力过大
  - b:等待资源的进程数，比如正在等待磁盘I/0、网络I/0等。
- cpu
  - us:用户进程消耗CPU时间百分比，us值高，用户进程消耗CPU时间多，如果长期大于50%优化程序;
  - sy:内核进程消耗的CPU时间百分比;
  - id:处于空闲的CPU百分比.
  - wa:系统等待IO的CPU时间百分比.
  - st:来自于一个虚拟机偷取的CPU时间的百分比

### mpstat

查看cpu所有的使用情况

mpstat -P ALL 2

![image-20220324145557414](D:\JavaProject\面试题总结Images\image-20220324145557414.png)

### pidstat：

展示那个用户的哪一个进程的信息

![image-20220323224459075](D:\JavaProject\面试题总结Images\image-20220323224459075.png)

### free:

free -m查看剩余的内存

pidstat -p 进程号 -r 采样间隔

### df:

查看磁盘剩余空间

df -h

### iostat:

磁盘io

![image-20220324150117183](D:\JavaProject\面试题总结Images\image-20220324150117183.png)

rkB/s每秒读取数据量kB;wkB/s每秒写入数据量kB;
svctm l/O请求的平均服务时间，单位毫秒;
await lI/O请求的平均等待时间，单位毫秒;值越小，性能越好;
util一秒中有百分几的时间用于IO操作。接近100%时，表示磁盘带宽跑满，需要优化程序或者增加磁盘;
rkB/s、wkB/s根据系统应用不同会有不同的值，但有规律遵循:长期、超大数据读写，肯定不正常，需要优化程序读取。svctm的值与await的值很接近，表示几乎没有IO等待，磁盘性能好，
如果await的值远高于svctm的值，则表示I/O队列等待太长，需要优化程序或更换更快磁盘。

### ifstat:

网络io

```
wget http://gael.roualland.free.frlifstat/ifstat-1.1.tar.gz

tar xzvf ifstat-1.1.tar.gz

cd ifstat-1.1

./configuremake

make install
```

ifstat l

## 假如生产环境出现cPu占用过高，请谈谈你的分析思路和定位

1. 先用top命令查询一下整机中那个应用cpu占用率最高
2. **ps -ef或者jps**进一步定位，得知是一个怎么样的一个后台程序给我们惹事
   - ps -mp [进程号] -o THREAD ,tid, time
3. 定位到具体线程或者代码
4. 将需要的**线程ID**转换为16进制
5. jstack 进程ID | grep tid （16进制线程ID小写因为） -A60

## 使用GitHub

### in功能

[关键词] in: name,readme,description

### 通配符

xxx关键词 stars 通配符

:> :>=大于或者大于等于

区间范围： 数字1..数字2

案例：

```
sprigboot stars :> 5000
sprigboot forks :> 5000
springboot forks:200..500 stats:80..100
```

###  awesome [关键词]

```
awesome redis
// 论文，官网，优秀的框架
```

### 高亮显示某一行代码

地址+#L[lineNumber]-L[lineNumber]

### 项目内搜索

英文T

可以搜别人的源码，从源码级开始搜索

### 搜索用户

```
location:beijing language:java
```

## 字符串常量池，java内部加载

```java
String str1 = new StringBuilder("58").append("tongcheng");
System.out.Println(str1);
System.out.Println(str1.intern());
System.out.Println(str1 == str1.intern());

String str2 = new StringBuilder("ja").append("va");
System.out.Println(str2);
System.out.Println(str2.intern());
System.out.Println(str2 == str2.intern());

/*
*
58tongcheng
58tongcheng
true
java
java
false
/
```

查看jdk源码

可以得到，在jdk classLoader的时候，会将一个java的字符串常量加载进元空间中，所以自己再重新写的java永远不可能和元空间的那个java相同

## 闲聊AQS：

### 可重入锁(也叫做递归锁)

指的是同一线程外层函数获得锁之后﹐内层递归函数仍然能获取该锁的代码，在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁
也即是说，**线程可以进入任何一个它已经拥有的锁所同步着的代码块**。

- ReentrantLock/Synchronized就是一个典型的可重入锁
- 可重入锁最大的作用是避兔死锁

```java
public synchronized void sendsMs ( ) throws Exception
{
    System.out.println( Thread.currentThread( ).getName()+"\t invoked sendSNS()");
    sendEmail();
}
public synchronized void sendEmail() throws Exception{
 	system.out.println(Thread.currentThread().getName()+"\t######## invoked sendEmail()");   
}


public static void m1(){
    new Thread(() -> {
        synchronized (objectLockA){
            system.out.println(Thread.currentThread( ).getName()+"\t"+"-----外层调用");
            synchronized (objectLockA){
                system.out.println( Thread.currentThread( ).getName()+"\t"+"-----中层调用");
                synchronized (objectLockA){
                    system.out.println(Thread.currentThread( ).getName()+"\t"+"-----内层调用");
}, name: "t1").start();

```

每个锁对象拥有一个锁计数器和一个指向持有该锁的线程的指针。
当执行monitorenterit，如果目标锁对象的计数器为零，那么说明它没有被其他线程所持有，Java虚拟机会将该锁对象的持有线程设置为当前线程，并且将其计数器加1。
在目标锁对象的计数器不为零的情况下，如果锁对象的持有线程是当前线程，那么Java虚拟机可以将其计数器加1，否则需要等待，直至持有线程释放该锁。
当执行monitorexit时，Java虚拟机则需将锁对象的计数器减1。计数器为零代表锁已被释放。

### LockSupport

#### lockSupport是什么

用于创建锁和其他同步类的基本线程阻塞原语。

![image-20220325155502615](D:\JavaProject\面试题总结Images\image-20220325155502615.png)



该类与使用它的每个线程关联一个许可证（在[`Semaphore`](https://www.apiref.com/java11-zh/java.base/java/util/concurrent/Semaphore.html)类的意义上）。 如果许可证可用，将立即返回`park` ，并在此过程中消费; 否则*可能会*阻止。 如果尚未提供许可，则致电`unpark`获得许可。 （与Semaphores不同，许可证不会累积。最多只有一个。）可靠的使用需要使用volatile（或原子）变量来控制何时停放或取消停放。 对于易失性变量访问保持对这些方法的调用的顺序，但不一定是非易失性变量访问。

方法`park`和`unpark`提供了阻止和解除阻塞线程的有效方法，这些线程没有遇到导致不推荐使用的方法`Thread.suspend`和`Thread.resume`无法用于此类目的的问题：一个线程调用`park`和另一个线程尝试`unpark`将保留活跃性，由于许可证。 此外，如果调用者的线程被中断，则会返回`park` ，并且支持超时版本。 `park`方法也可以在任何其他时间返回，“无理由”，因此通常必须在返回时重新检查条件的循环内调用。 在这个意义上， `park`可以作为“忙碌等待”的优化，不会浪费太多时间旋转，但必须与`unpark`配对才能生效。

三种形式的`park`每个也支持`blocker`对象参数。 在线程被阻塞时记录此对象，以允许监视和诊断工具识别线程被阻止的原因。 （此类工具可以使用方法[`getBlocker(Thread)`](https://www.apiref.com/java11-zh/java.base/java/util/concurrent/locks/LockSupport.html#getBlocker(java.lang.Thread))访问[阻止程序](https://www.apiref.com/java11-zh/java.base/java/util/concurrent/locks/LockSupport.html#getBlocker(java.lang.Thread)) 。）强烈建议使用这些表单而不是没有此参数的原始表单。 在锁实现中作为`blocker`提供的正常参数是`this` 。

这些方法旨在用作创建更高级别同步实用程序的工具，并且对于大多数并发控制应用程序本身并不有用。 `park`方法仅用于以下形式的构造：

```
   while (!canProceed()) { // ensure request to unpark is visible to other threads ... LockSupport.park(this); } 
```

在调用`park`之前，线程没有发布请求`park`需要锁定或阻塞。 因为每个线程只有一个许可证，所以任何中间使用`park` ，包括隐式地通过类加载，都可能导致无响应的线程（“丢失unpark”）。

**样品使用。** 以下是先进先出非重入锁定类的草图：

**线程等待唤醒机制的更新版**

- 3种让线程等待和唤醒的方法

  - wait，notify和synchronize是必须同时使用的

    ```java
    static object objectLock = new object();
    public static void main(string[ ] args)//main方法，主线程一切程序入口
    {
        new Thread(() -> {
            synchronized (objectLock){
                system.out.println(Thread.currentThread().getName()+"\t"+"-----come in");
                try {
                    objectLock.wait();
                } catch ( InterruptedException e) {
                    e.printstackTrace();
                    system.out.println( Thread.currentThread( ).getName()+"\t"+"------被唤醒");/ 
                }
            },name: "A").start( );
            new Thread(() ->{
                synchronized (objectLock){
                    objectLock.notify();
                    system.out.println(Thread.currentThread( ).getName()+"\t"+"-----通知");
                }
                ,name: "B" ).start();
    ```

- object类中的wait和notify方法实现线程等待和唤

  - ```java
    static object objectLock = new object();
    public static void main(string[ ] args)//main方法，主线程一切程序入口
        new Thread(() -> {
            synchronized (objectLock ){
                system.out.println(Thread.currentThread().getName()+"\t"+"-----come in");
                try {
                    objectLock.wait();
                } catch ( InterruptedException e) {
                    e.printstackTrace();
                    system.out.println( Thread.currentThread( ).getName()+"\t"+"------被唤醒");/ 
                }
            }, name: "A").start( );
            new Thread(() ->{
                synchronized (objectLock){
                    objectLock.notify();
                    system.out.println(Thread.currentThread( ).getName()+"\t"+"-----通知");
                }, name: "B" ).start();
    ```

- Condition接口中的await后signal方法实现线程的等待和唤醒传统的synchronized和lLock实现等待唤醒通知的约束

  - await和signal必须和lock同时使用

  - ```java
    static Lock lock = new ReentrantLock();
    static Condition condition = lock.newCondition();s
    public static void main(string[ ] args)//main方法，主线程一切程序入口
        new Thread(() -> {
            lock.lock();
            try{
                system.out.println(Thread.currentThread().getName()+"\t"+"-----come in");
                try {
                    condition.await();
                } catch ( InterruptedException e) {
                    e.printstackTrace();
                    system.out.println( Thread.currentThread( ).getName()+"\t"+"------被唤醒");/ 
                }finally{
                    lock.unlock();
                }
            }, name: "A").start( );
            new Thread(() ->{
                lock.lock();
                try{
                    condition.signal();
                    system.out.println(Thread.currentThread( ).getName()+"\t"+"-----通知");
                }finally{
                    lock.unlock();
                }
                }, name: "B" ).start();
    ```

- LockSupport类中的park等待和unpark唤醒

  - 线程先要获得并持有锁，必须在锁块(synchronized或lock)中必须要先等待后唤醒,线程才能够被唤醒

LockSupport是用来创建锁和其他同步类的基本线程阻塞原语。
LockSupport类使用了一种名为Permit(许可）的概念来做到阻塞和唤醒线程的功能，每个线程都有一个许可(permit)，permit只有两个值1和零，默认是零。
可以把许可看成是一种(0,1)信号量〈Semaphore)，但与Semaphore不同的是，许可的累加上限是1。

permit默认是O，所以一开始调用park()方法，当前线程就会阻塞，直到别的线程将当前线程的permit设置为1时，park方法会被唤醒，然后会将permit再次设置为O并返回。

调用unpark(thread)方法后，就会将thread线程的许可permit设置成1(注意多次调用unpark方法，不会累加，permit值还是1)会自动唤醒thread线程，即之前阻塞中的LockSupport.park()方法会立即返回。

```java
public static void main(String[] args) {
        Thread aThread = new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "来了");
            LockSupport.park();
            System.out.println(Thread.currentThread().getName() + "醒了过来");
        }, "a");
        Thread bThread = new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "来叫醒了");
            LockSupport.unpark(aThread);
        }, "b");

        aThread.start();
        bThread.start();
    }
```

LockSupport是用来创建锁和其他同步类的基本线程阻塞原语。
LockSupport是一个线程阻塞工具类，所有的方法都是静态方法，可以让线程在任意位置阻塞，阻塞之后也有对应的唤醒方法。归根结底，LockSupport调用的Unsafe中的native代码。
LockSupport提供park()和unpark()方法实现阻塞线程和解除线程阻塞的过程
LockSupport和每个使用它的线程都有一个许可(permit)关联。permit相当于1，0的开关，默认是O,调用一次unpark就加1变成1，
调用一次park会消费permit,也就是将1变成0，同时park立即返回。
如再次调用park会变成阻塞(因为permit为零了会阻塞在这里，一直到permit变为1)，这时调用unpark会把permit置为1。每个线程都有一个相关的permit, permit最多只有一个，重复调用unpark也不会积累凭证。
形象的理解
线程阻塞需要消耗凭证(permit)，这个凭证最多只有1个。当调用park方法时
***如果有凭证，则会直接消耗掉这个凭证然后正常退出;**

***如果无凭证，就必须阻塞等待凭证可用;**
**而unpark则相反，它会增加一个凭证，但凭证最多只能有1个，累加无效。**



#### 为什么可以先唤醒线程后阻塞线程?

因为unpark获得了一个凭证，之后再调用park方法，就可以名正言顺的凭证消费，故不会阻塞。

#### 为什么唤醒两次后阻塞两次，但最终结果还会阻塞线程?

因为凭证的数量最多为1，连续调用两次unpark和调用一次 unpark 效果一样，只会增加一个凭证;而调用两次park却需要消费两个凭证，证不够，不能放行。

### AbstractQueuedSynchronizer之AQS

抽象的队列同步器

- AbstractOwnableSynchronizer
- AbstractQueuedLongSynchronizer
- AbstractQueuedSynchronizer

是用来构建锁或者其它同步器组件的**重量级基础框架及整个Juc体系的基石**，通过内置的FIFO**队列**来完成资源获取线程的排队工作，并通过一个**int类型变量**表示持有锁的状态



![image-20220325201247755](D:\JavaProject\面试题总结Images\image-20220325201247755.png)

CLH: Craig、Landin and Hagersten队列，是一个单向链表，AQS中的队列是CLH变体的虚拟双向队列FIFO

![image-20220325201737077](D:\JavaProject\面试题总结Images\image-20220325201737077.png)

加锁会导致阻塞

有阻塞就需要排队，实现排队必然需要有某种形式的队列来进行管理

抢到资源的线程直接使用处理业务逻辑，抢不到资源的必然涉及一种排队等候机制。抢占资源失败的线程继续去等待(类似银行业务办理窗口都满了，暂时没有受理窗口的顾客只能去候客区排队等候)，但等候线程仍然保留获取锁的可能且获取锁流程仍在继续(候客区的顾客也在等着叫号，轮到了再去受理窗口办理业务)。
既然说到了排队等候机制，那么就一定会有某种队列形成，这样的队列是什么数据结构呢?
如果共享资源被占用，就需要一定的阻塞等待唤醒机制来保证锁分配。这个机制主要用的是CLH队列的变体实现的，将暂时获取不到锁的线程加入到队列中，这个队列就是AQS的抽象表现。它将请求共享资源的线程封装成队列的结点(Node)，通过CAS、自旋以及LockSupport parKk)的方式，维护'state变量的状态，使并发达到同步的控制效果。

#### AQS初识

AQS使用一个volatile的int类型的成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取的排队工作将每条要去抢占资源的线程封装成一个Node节点来实现锁的分配，通过CAS完成对State值的修改。"

```java
public abstract class AbstractQueuedSynchronizer
    extends AbstractOwnableSynchronizer
    implements java.io.Serializable {

    private static final long serialVersionUID = 7373984972572414691L;

    /**
     * 创建一个新的{@code AbstractQueuedSynchronizer}实例，初始同步状态为0。
     */
    protected AbstractQueuedSynchronizer() { }

    /**
     等待队列节点类。 他的等待队列是 "CLH"（Craig, Landin, and Hagersten）锁队列的一个变种。CLH锁通常被用于 自旋锁。 我们将其用于阻塞性同步器，但 使用相同的基本策略，即在预设的线程中保留一些控制 关于一个线程的一些控制信息在其节点的前身中。 A 每个节点中的 "状态 "字段会跟踪一个线程是否 是否应该阻塞。 当一个节点的前身释放时，该节点就会收到信号。释放。 另外，队列的每个节点都作为一个 具体通知式的监视器，持有一个等待的 线程。状态字段并不控制线程是否被 授予锁等。 一个线程可以尝试获取，如果它是
 在队列中的第一位。但是，作为第一个并不保证成功。它只是提供了竞争的权利。 所以目前被释放的 竞争者的线程可能需要重新等待。
     *
     * <p>要想排队进入一个CLH锁，你要把它原子化地拼接成新的
 尾部。要取消queue，你只需设置head字段。
     * <pre>
     *      +------+  prev +-----+       +-----+
     * head |      | <---- |     | <---- |     |  tail
     *      +------+       +-----+       +-----+
     * </pre>
     *
     * <p>Insertion into a CLH queue requires only a single atomic
     * operation on "tail", so there is a simple atomic point of
     * demarcation from unqueued to queued. Similarly, dequeuing
     * involves only updating the "head". However, it takes a bit
     * more work for nodes to determine who their successors are,
     * in part to deal with possible cancellation due to timeouts
     * and interrupts.
     *
     * 插入CLH队列只需要对 "尾巴 "进行一次原子操作。
      的操作，所以有一个简单的原子点
      从未排队到排队的分界点。同样地，去排队
      只涉及更新 "头"。然而，它需要一点
      要确定谁是他们的继任者，需要更多的工作。
      部分是为了处理由于超时和中断而可能出现的取消。
      和中断造成的取消。
     
      "prev "链接（在最初的CLH锁中没有使用），主要用于
      需要来处理取消问题。如果一个节点被取消了，它的
      继承者（通常）会被重新链接到一个没有被取消的
      前身。关于自旋锁的类似机制的解释
      关于自旋锁的类似机制的解释，见Scott和Scherer的论文，网址是
      http://www.cs.rochester.edu/u/scott/synchronization/
     
      我们还使用 "下一个 "链接来实现阻塞机制。
      每个节点的线程ID都保存在它自己的节点中，所以一个
      前身发出信号，让下一个节点通过遍历
      下一个链接来确定它是哪个线程。 确定
      继任者的确定必须避免与新排队的节点竞赛，以设置
      的 "下一个 "字段的竞赛。 这个问题的解决方法是
      在必要时，通过从原子化的 "尾部 "向后检查
      更新的 "尾部"，当一个节点的继任者看起来是空的时候，就可以解决这个问题。
      (或者，换个说法，下一个链接是一种优化
      所以我们通常不需要向后扫描）。
     
      取消引入了一些保守的基本
      算法引入了一些保守性。 由于我们必须对其他节点的取消进行轮询
      节点，我们可能无法注意到一个被取消的节点是否在我们的
      在我们前面还是后面。处理这个问题的方法是，在取消节点时，总是取消后继者的停车位
      处理这个问题的方法是，在取消节点时，总是取消后继者的停车位，让他们稳定在一个新的前任上。
      新的前任，除非我们能找到一个未取消的前任来承担这个责任。
      的前身来承担这个责任。
     
      CLH队列需要一个假头节点来启动。但是
      我们不会在构建时创建它们，因为如果从来没有竞争，这将是白费功夫
      因为如果从来没有争论的话，这将是浪费精力。相反，该节点
      构建，并在第一次争用时设置头和尾的指针。
      争用。
     
      等待条件的线程使用相同的节点，但
      使用一个额外的链接。条件只需要链接节点
      在简单的（非并发的）链接队列中，因为他们
      只有在独家持有时才会被访问。 在等待时，一个节点被
      插入到一个条件队列中。 在发出信号时，该节点被
      转移到主队列中。 状态字段的一个特殊值
      字段的特殊值被用来标记节点在哪个队列中。
     
      感谢Dave Dice, Mark Moir, Victor Luchangco, Bill
      Scherer和Michael Scott，以及JSR-166专家小组的成员。
      专家组的成员，感谢他们对这个类的设计提出了有益的想法、讨论和批评。
      对该类的设计提出了有益的意见、讨论和批评。
     */
    static final class Node {
        /** Marker to indicate a node is waiting in shared mode */
        static final Node SHARED = new Node();
        /** Marker to indicate a node is waiting in exclusive mode */
        static final Node EXCLUSIVE = null;

        /** waitStatus value to indicate thread has cancelled */
        static final int CANCELLED =  1;
        /** waitStatus value to indicate successor's thread needs unparking */
        static final int SIGNAL    = -1;
        /** waitStatus value to indicate thread is waiting on condition */
        static final int CONDITION = -2;
        /**
         * waitStatus value to indicate the next acquireShared should
         * unconditionally propagate
         */
        static final int PROPAGATE = -3;

        /**
         * Status field, taking on only the values:
         *   SIGNAL:     The successor of this node is (or will soon be)
         *               blocked (via park), so the current node must
         *               unpark its successor when it releases or
         *               cancels. To avoid races, acquire methods must
         *               first indicate they need a signal,
         *               then retry the atomic acquire, and then,
         *               on failure, block.
         *   CANCELLED:  This node is cancelled due to timeout or interrupt.
         *               Nodes never leave this state. In particular,
         *               a thread with cancelled node never again blocks.
         *   CONDITION:  This node is currently on a condition queue.
         *               It will not be used as a sync queue node
         *               until transferred, at which time the status
         *               will be set to 0. (Use of this value here has
         *               nothing to do with the other uses of the
         *               field, but simplifies mechanics.)
         *   PROPAGATE:  A releaseShared should be propagated to other
         *               nodes. This is set (for head node only) in
         *               doReleaseShared to ensure propagation
         *               continues, even if other operations have
         *               since intervened.
         *   0:          None of the above
         *
         * The values are arranged numerically to simplify use.
         * Non-negative values mean that a node doesn't need to
         * signal. So, most code doesn't need to check for particular
         * values, just for sign.
         *
         * The field is initialized to 0 for normal sync nodes, and
         * CONDITION for condition nodes.  It is modified using CAS
         * (or when possible, unconditional volatile writes).
         */
        volatile int waitStatus;

        /**
         * Link to predecessor node that current node/thread relies on
         * for checking waitStatus. Assigned during enqueuing, and nulled
         * out (for sake of GC) only upon dequeuing.  Also, upon
         * cancellation of a predecessor, we short-circuit while
         * finding a non-cancelled one, which will always exist
         * because the head node is never cancelled: A node becomes
         * head only as a result of successful acquire. A
         * cancelled thread never succeeds in acquiring, and a thread only
         * cancels itself, not any other node.
         */
        volatile Node prev;

        /**
         * Link to the successor node that the current node/thread
         * unparks upon release. Assigned during enqueuing, adjusted
         * when bypassing cancelled predecessors, and nulled out (for
         * sake of GC) when dequeued.  The enq operation does not
         * assign next field of a predecessor until after attachment,
         * so seeing a null next field does not necessarily mean that
         * node is at end of queue. However, if a next field appears
         * to be null, we can scan prev's from the tail to
         * double-check.  The next field of cancelled nodes is set to
         * point to the node itself instead of null, to make life
         * easier for isOnSyncQueue.
         */
        volatile Node next;

        /**
         * The thread that enqueued this node.  Initialized on
         * construction and nulled out after use.
         */
        volatile Thread thread;

        /**
         * Link to next node waiting on condition, or the special
         * value SHARED.  Because condition queues are accessed only
         * when holding in exclusive mode, we just need a simple
         * linked queue to hold nodes while they are waiting on
         * conditions. They are then transferred to the queue to
         * re-acquire. And because conditions can only be exclusive,
         * we save a field by using special value to indicate shared
         * mode.
         */
        Node nextWaiter;

        /**
         * Returns true if node is waiting in shared mode.
         */
        final boolean isShared() {
            return nextWaiter == SHARED;
        }

        /**
         * Returns previous node, or throws NullPointerException if null.
         * Use when predecessor cannot be null.  The null check could
         * be elided, but is present to help the VM.
         *
         * @return the predecessor of this node
         */
        final Node predecessor() throws NullPointerException {
            Node p = prev;
            if (p == null)
                throw new NullPointerException();
            else
                return p;
        }

        Node() {    // Used to establish initial head or SHARED marker
        }

        Node(Thread thread, Node mode) {     // Used by addWaiter
            this.nextWaiter = mode;
            this.thread = thread;
        }

        Node(Thread thread, int waitStatus) { // Used by Condition
            this.waitStatus = waitStatus;
            this.thread = thread;
        }
    }

    /**
     * Head of the wait queue, lazily initialized.  Except for
     * initialization, it is modified only via method setHead.  Note:
     * If head exists, its waitStatus is guaranteed not to be
     * CANCELLED.
     */
    private transient volatile Node head;

    /**
     * Tail of the wait queue, lazily initialized.  Modified only via
     * method enq to add new wait node.
     */
    private transient volatile Node tail;

    /**
     * The synchronization state.
     */
    private volatile int state;

    /**
     * Returns the current value of synchronization state.
     * This operation has memory semantics of a {@code volatile} read.
     * @return current state value
     */
    protected final int getState() {
        return state;
    }

    /**
     * Sets the value of synchronization state.
     * This operation has memory semantics of a {@code volatile} write.
     * @param newState the new state value
     */
    protected final void setState(int newState) {
        state = newState;
    }

    /**
     * Atomically sets synchronization state to the given updated
     * value if the current state value equals the expected value.
     * This operation has memory semantics of a {@code volatile} read
     * and write.
     *
     * @param expect the expected value
     * @param update the new value
     * @return {@code true} if successful. False return indicates that the actual
     *         value was not equal to the expected value.
     */
    protected final boolean compareAndSetState(int expect, int update) {
        // See below for intrinsics setup to support this
        return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
    }

    // Queuing utilities

    /**
     * The number of nanoseconds for which it is faster to spin
     * rather than to use timed park. A rough estimate suffices
     * to improve responsiveness with very short timeouts.
     */
    static final long spinForTimeoutThreshold = 1000L;

    /**
     * Inserts node into queue, initializing if necessary. See picture above.
     * @param node the node to insert
     * @return node's predecessor
     */
    private Node enq(final Node node) {
        for (;;) {
            Node t = tail;
            if (t == null) { // Must initialize
                if (compareAndSetHead(new Node()))
                    tail = head;
            } else {
                node.prev = t;
                if (compareAndSetTail(t, node)) {
                    t.next = node;
                    return t;
                }
            }
        }
    }

    /**
     * Creates and enqueues node for current thread and given mode.
     *
     * @param mode Node.EXCLUSIVE for exclusive, Node.SHARED for shared
     * @return the new node
     */
    private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        // Try the fast path of enq; backup to full enq on failure
        Node pred = tail;
        if (pred != null) {
            node.prev = pred;
            if (compareAndSetTail(pred, node)) {
                pred.next = node;
                return node;
            }
        }
        enq(node);
        return node;
    }

    /**
     * Sets head of queue to be node, thus dequeuing. Called only by
     * acquire methods.  Also nulls out unused fields for sake of GC
     * and to suppress unnecessary signals and traversals.
     *
     * @param node the node
     */
    private void setHead(Node node) {
        head = node;
        node.thread = null;
        node.prev = null;
    }

    /**
     * Wakes up node's successor, if one exists.
     *
     * @param node the node
     */
    private void unparkSuccessor(Node node) {
        /*
         * If status is negative (i.e., possibly needing signal) try
         * to clear in anticipation of signalling.  It is OK if this
         * fails or if status is changed by waiting thread.
         */
        int ws = node.waitStatus;
        if (ws < 0)
            compareAndSetWaitStatus(node, ws, 0);

        /*
         * Thread to unpark is held in successor, which is normally
         * just the next node.  But if cancelled or apparently null,
         * traverse backwards from tail to find the actual
         * non-cancelled successor.
         */
        Node s = node.next;
        if (s == null || s.waitStatus > 0) {
            s = null;
            for (Node t = tail; t != null && t != node; t = t.prev)
                if (t.waitStatus <= 0)
                    s = t;
        }
        if (s != null)
            LockSupport.unpark(s.thread);
    }

    /**
     * Release action for shared mode -- signals successor and ensures
     * propagation. (Note: For exclusive mode, release just amounts
     * to calling unparkSuccessor of head if it needs signal.)
     */
    private void doReleaseShared() {
        /*
         * Ensure that a release propagates, even if there are other
         * in-progress acquires/releases.  This proceeds in the usual
         * way of trying to unparkSuccessor of head if it needs
         * signal. But if it does not, status is set to PROPAGATE to
         * ensure that upon release, propagation continues.
         * Additionally, we must loop in case a new node is added
         * while we are doing this. Also, unlike other uses of
         * unparkSuccessor, we need to know if CAS to reset status
         * fails, if so rechecking.
         */
        for (;;) {
            Node h = head;
            if (h != null && h != tail) {
                int ws = h.waitStatus;
                if (ws == Node.SIGNAL) {
                    if (!compareAndSetWaitStatus(h, Node.SIGNAL, 0))
                        continue;            // loop to recheck cases
                    unparkSuccessor(h);
                }
                else if (ws == 0 &&
                         !compareAndSetWaitStatus(h, 0, Node.PROPAGATE))
                    continue;                // loop on failed CAS
            }
            if (h == head)                   // loop if head changed
                break;
        }
    }

    /**
     * Sets head of queue, and checks if successor may be waiting
     * in shared mode, if so propagating if either propagate > 0 or
     * PROPAGATE status was set.
     *
     * @param node the node
     * @param propagate the return value from a tryAcquireShared
     */
    private void setHeadAndPropagate(Node node, int propagate) {
        Node h = head; // Record old head for check below
        setHead(node);
        /*
         * Try to signal next queued node if:
         *   Propagation was indicated by caller,
         *     or was recorded (as h.waitStatus either before
         *     or after setHead) by a previous operation
         *     (note: this uses sign-check of waitStatus because
         *      PROPAGATE status may transition to SIGNAL.)
         * and
         *   The next node is waiting in shared mode,
         *     or we don't know, because it appears null
         *
         * The conservatism in both of these checks may cause
         * unnecessary wake-ups, but only when there are multiple
         * racing acquires/releases, so most need signals now or soon
         * anyway.
         */
        if (propagate > 0 || h == null || h.waitStatus < 0 ||
            (h = head) == null || h.waitStatus < 0) {
            Node s = node.next;
            if (s == null || s.isShared())
                doReleaseShared();
        }
    }

    // Utilities for various versions of acquire

    /**
     * Cancels an ongoing attempt to acquire.
     *
     * @param node the node
     */
    private void cancelAcquire(Node node) {
        // Ignore if node doesn't exist
        if (node == null)
            return;

        node.thread = null;

        // Skip cancelled predecessors
        Node pred = node.prev;
        while (pred.waitStatus > 0)
            node.prev = pred = pred.prev;

        // predNext is the apparent node to unsplice. CASes below will
        // fail if not, in which case, we lost race vs another cancel
        // or signal, so no further action is necessary.
        Node predNext = pred.next;

        // Can use unconditional write instead of CAS here.
        // After this atomic step, other Nodes can skip past us.
        // Before, we are free of interference from other threads.
        node.waitStatus = Node.CANCELLED;

        // If we are the tail, remove ourselves.
        if (node == tail && compareAndSetTail(node, pred)) {
            compareAndSetNext(pred, predNext, null);
        } else {
            // If successor needs signal, try to set pred's next-link
            // so it will get one. Otherwise wake it up to propagate.
            int ws;
            if (pred != head &&
                ((ws = pred.waitStatus) == Node.SIGNAL ||
                 (ws <= 0 && compareAndSetWaitStatus(pred, ws, Node.SIGNAL))) &&
                pred.thread != null) {
                Node next = node.next;
                if (next != null && next.waitStatus <= 0)
                    compareAndSetNext(pred, predNext, next);
            } else {
                unparkSuccessor(node);
            }

            node.next = node; // help GC
        }
    }
}
```

![image-20220325220441570](D:\JavaProject\面试题总结Images\image-20220325220441570.png)

![image-20220325220833123](D:\JavaProject\面试题总结Images\image-20220325220833123.png)

![image-20220325220850964](D:\JavaProject\面试题总结Images\image-20220325220850964.png)

#### 源码分析

1. 第一个线程进来的时候，首先进行加锁准备。使用cas自旋锁。比较成功之后，将当前的线程设置进去。

```java
final void lock() {
    //期望的是当前的值为0，然后设置为1
    //其中state是有aqs来完成的，0表示当前没有线程持有锁，1表示当前线程中存在一个线程正在使用锁
            if (compareAndSetState(0, 1))
                setExclusiveOwnerThread(Thread.currentThread());
            else
                acquire(1);
        }
```

1.1 其中cas锁的源码为：

```java
protected final boolean compareAndSetState(int expect, int update) {
    // See below for intrinsics setup to support this
    //当前线程，希望在主存的stateoffset位置的值，为expect，如果是则将值改为update，否则进行自旋
    return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}
```

```c++
UNSAFE_ENTRY(jobject, Unsafe_CompareAndExchangeReference(JNIEnv *env, jobject unsafe, jobject obj, jlong offset, jobject e_h, jobject x_h)) {
  oop x = JNIHandles::resolve(x_h);
  oop e = JNIHandles::resolve(e_h);
  oop p = JNIHandles::resolve(obj);
  assert_field_offset_sane(p, offset);
  oop res = HeapAccess<ON_UNKNOWN_OOP_REF>::oop_atomic_cmpxchg_at(p, (ptrdiff_t)offset, e, x);
  return JNIHandles::make_local(env, res);
} UNSAFE_END
```

2. 第二个线程进来的时候，发现已经有线程在这里等待了。所以第二个线程进行acquire尝试获取锁，并且将state为1作为参数传递给acquire

```java
/**
在独占模式下获取，无视中断。通过调用至少一次tryAcquire来实现，成功后返回。否则线程会被排队，可能会反复阻塞和解阻塞，调用tryAcquire直到成功。这个方法可以用来实现Lock.lock方法。
Params:
arg - 获取参数。这个值会传达给tryAcquire，但在其他方面是不被解释的，可以代表任何你喜欢的东西。
*/
    public final void acquire(int arg) {
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
    }
```

2.1 首先进行tryAcquire(1),再一次进行尝试拿到锁，如果拿到了锁，则可以执行代码，如果没有拿到锁，则返回false

```java
final boolean nonfairTryAcquire(int acquires) {
            final Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
```

2.2执行addWaiter(null)方法,为当前线程和给定模式创建和排队的节点。参数。模式 - Node.EXCLUSIVE表示独占，Node.SHARED表示共享。
返回新节点

![image-20220325231600782](D:\JavaProject\面试题总结Images\image-20220325231600782.png)

```java
private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        // Try the fast path of enq; backup to full enq on failure
        Node pred = tail;
        if (pred != null) {
            node.prev = pred;
            if (compareAndSetTail(pred, node)) {
                pred.next = node;
                return node;
            }
        }
        enq(node);
        return node;
    }

private Node enq(final Node node) {
        for (;;) {
            Node t = tail;
            if (t == null) { // 初始化一个哨兵节点，一个空node
                if (compareAndSetHead(new Node()))
                    tail = head;
            } else {
                node.prev = t;
                if (compareAndSetTail(t, node)) {
                    t.next = node;
                    return t;
                }
            }
        }
    }
```

2.3 之后将新生成的CLU队列作为参数执行acquireQueued()方法

```java
/**
各种风味的获取，在独占/共享和
     * 控制模式。 每种模式基本相同，但又有恼人的不同。 由于异常机制（包括确保我们在tryAcquire抛出异常时取消）和其他控制的相互作用，只有一点点的因子是可能的，至少不会对性能造成太大伤害。

对于已经在队列中的线程，以排他性的不可中断的模式进行获取。被条件等待方法以及获取方法所使用。
参数。
node - 节点
arg - 获取的参数
返回。
如果在等待时被打断，返回true
*/

final boolean acquireQueued(final Node node, int arg) {
        boolean failed = true;
        try {
            boolean interrupted = false;
            for (;;) {
                final Node p = node.predecessor();
                if (p == head && tryAcquire(arg)) {
                    setHead(node);
                    p.next = null; // help GC
                    failed = false;
                    return interrupted;
                }
                if (shouldParkAfterFailedAcquire(p, node) &&
                    parkAndCheckInterrupt())
                    interrupted = true;
            }
        } finally {
            if (failed)
                cancelAcquire(node);
        }
    }

// predecessor
//这里首先是拿到哨兵接点，先判断哨兵接点是不是为空。
final Node predecessor() throws NullPointerException {
            Node p = prev;
            if (p == null)
                throw new NullPointerException();
            else
                return p;
        }
/**
之后判断哨兵接点是头接点嘛？因为是排队的，所以应该从第一个接点开始判断，去尝试去抢锁，看是否可以抢锁成功。
失败之后会执行shouldParkAfterFailedAcquire()方法
判断当前的接点的waitStatus是否是-1，如果不是-1，也不大于0，则compareAndSetWaitStatus(pred, ws, Node.SIGNAL);将当前的哨兵节点的waitStatus值设置为-1
之后会再一次抢锁，如果还是抢锁失败，则执行shouldParkAfterFailedAcquire()方法
这一次哨兵节点的值为-1 == Node.SIGNAL

*/
private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
        int ws = pred.waitStatus;
        if (ws == Node.SIGNAL)
            /*
             * This node has already set status asking a release
             * to signal it, so it can safely park.
             */
            return true;
        if (ws > 0) {
            /*
             * Predecessor was cancelled. Skip over predecessors and
             * indicate retry.
             */
            do {
                node.prev = pred = pred.prev;
            } while (pred.waitStatus > 0);
            pred.next = node;
        } else {
            /*
             * waitStatus must be 0 or PROPAGATE.  Indicate that we
             * need a signal, but don't park yet.  Caller will need to
             * retry to make sure it cannot acquire before parking.
             */
            compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
        }
        return false;
    }

/**
之后执行park方法，将执行方法的线程挂起
*/
private final boolean parkAndCheckInterrupt() {
        LockSupport.park(this);
        return Thread.interrupted();
    }
```

2.4 此时队列中的线程已经在排队了，除非执行方法的线程进行唤醒，否则将会一直阻塞下去

2.4.1 接下来是unlock流程

2.4.1.1首先进行解锁，解锁的时候调用的是sync的release方法

```java
public void unlock() {
    sync.release(1);
}
```

2.4.1.2解锁的第一步，先尝试去释放锁资源，这个arg = 1，tryRelease(1)

```java
public final boolean release(int arg) {
    if (tryRelease(arg)) {
        //拿到当前的哨兵节点，如果当前的waitStatus!= 0说明，当前队列中存在park的线程，所以需要unparkSuccessor()
        Node h = head;
        if (h != null && h.waitStatus != 0)
            unparkSuccessor(h);
        return true;
    }
    return false;
}
```

2.4.2因为State的信号值为1，所以getState() == 1,则c == 0；

```java
protected final boolean tryRelease(int releases) {
    int c = getState() - releases;
    if (Thread.currentThread() != getExclusiveOwnerThread())
        //如果当前的线程不是正在执行业务的线程，则会报错
        throw new IllegalMonitorStateException();
    boolean free = false;
    //设置的空闲状态为FALSE
    if (c == 0) {
        //因为线程业务已经完成了，所以将执行窗口置为true
        free = true;
        //这是办理业务的线程为null
        setExclusiveOwnerThread(null);
    }
    setState(c);
    return free;
}
```

2.4.3 紧接着上面2.4.1.2拿到当前的哨兵节点，如果当前的waitStatus!= 0说明，当前队列中存在park的线程，所以需要unparkSuccessor()

```java
private void unparkSuccessor(Node node) {
    /*
     * If status is negative (i.e., possibly needing signal) try
     * to clear in anticipation of signalling.  It is OK if this
     * fails or if status is changed by waiting thread.
     */
    //哨兵接点的waitStatus == -1，这个时候ws = -1
    int ws = node.waitStatus;
    if (ws < 0)
        //设置当前哨兵接点的值为0
        compareAndSetWaitStatus(node, ws, 0);

    /*
     * Thread to unpark is held in successor, which is normally
     * just the next node.  But if cancelled or apparently null,
     * traverse backwards from tail to find the actual
     * non-cancelled successor.
     */
    //紧接着找到第一个等待线程，第一次等待线程并不为空值
    Node s = node.next;
    //第一次的时候，s是第一个等待的线程，线程不为空，并且线程的waitStatus==-1
    if (s == null || s.waitStatus > 0) {
        //将s置空
        s = null;
        for (Node t = tail; t != null && t != node; t = t.prev)
            if (t.waitStatus <= 0)
                //将等待队列中的值依次遍历，然后将最开始的头接点设置给s
                s = t;
    }
    //因为阻塞队列中的存在线程，所以s != null
    if (s != null)
        //将s线程唤醒，此时s为第一个等待的线程
        LockSupport.unpark(s.thread);
}
```

2.5 因为第一个线程已经被唤醒了，所以必须执行还没有执行完的流程

接着2.3的流程

```java
final boolean acquireQueued(final Node node, int arg) {
        boolean failed = true;
        try {
            boolean interrupted = false;
            for (;;) {
                //拿到哨兵节点
                final Node p = node.predecessor();
                //哨兵节点等于头接点，并且此时信号灯为0，线程可以抢占资源
                if (p == head && tryAcquire(arg)) {
                    //设置头接点为node，此时node是抢占资源的节点
                    setHead(node);
                    /**
                    private void setHead(Node node) {
                    //将头结点指向队列当前节点
                        head = node;
                        //将当前节点的线程置空
                        node.thread = null;
                        //将指向哨兵节点的指针置空
                        node.prev = null;
                    }
                    */
                    //将哨兵节点指向后置节点的指针置为空
                    p.next = null; // help GC，处罚GC
                    failed = false;
                    return interrupted;
                }
                if (shouldParkAfterFailedAcquire(p, node) &&
                    parkAndCheckInterrupt())
                    interrupted = true;
            }
        } finally {
            if (failed)
                cancelAcquire(node);
        }
    }
```

## spring AOP详解：

### spring aop顺序

spring 4.3.13.RELEASE + spring boot 1.5.9.RELEASE

- 环绕通知前->before->业务代码->环绕通知后->after->afterReturning
- 环绕通知前->before->业务代码->after->afterThrowing

spring 5.2.8.RELEASE  + spring boot 2.3.3.RELEASE

- 环绕通知前->before->业务代码->afterReturning->after->环绕通知后
- 环绕通知前->before->业务代码->afterThrowing->after

![image-20220326100143255](D:\JavaProject\面试题总结Images\image-20220326100143255.png)

### spring 循环依赖

循环依赖关系
如果你使用占主导地位的构造函数注入，就有可能产生无法解决的循环依赖情况。

比如说。类A通过构造函数注入需要类B的一个实例，而类B通过构造函数注入需要类A的一个实例。如果你将A类和B类的Bean配置为相互注入，Spring IoC容器会在运行时检测到这种循环引用，并抛出一个BeanCurrentlyInCreationException。

一个可能的解决方案是编辑一些类的源代码，通过设置器而不是构造器进行配置。或者，避免构造器注入，只使用设置器注入。**换句话说，虽然不推荐这样做，但你可以用setter注入来配置循环依赖关系。**

与典型的情况（没有循环依赖关系）不同，Bean A和Bean B之间的循环依赖关系迫使其中一个Bean在被完全初始化之前被注入到另一个Bean中（一个典型的鸡生蛋蛋生鸡的情况）。

循环依赖的code

```java
// main
package com.haust.spring.aop.circular;

public class CircularDependenciesDemo {
    public static void main(String[] args) {
        new ServiceA(new ServiceB(new ServiceA()));
    }
}

// serviceA
package com.haust.spring.aop.circular;

public class ServiceA {
    private ServiceB serviceB;

    public ServiceA(ServiceB serviceB){
        this.serviceB = serviceB;
    }
}

//serviceB
package com.haust.spring.aop.circular;

public class ServiceB {
    private ServiceA serviceA;

    public ServiceB(ServiceA serviceA){
        this.serviceA = serviceA;
    }

}

```

```java
//使用setter方法来解决循环依赖的问题
public static void main(String[] args) {
//        new ServiceA(new ServiceB(new ServiceA()));
        ServiceASetter aSetter = new ServiceASetter();
        ServiceBSetter bSetter = new ServiceBSetter();
        aSetter.setServiceBSetter(bSetter);
        bSetter.setServiceASetter(aSetter);

    }

//serviceA
package com.haust.spring.aop.circular;

public class ServiceASetter {
    private ServiceBSetter serviceBSetter;

    public ServiceASetter(){}

    public ServiceASetter(ServiceBSetter serviceBSetter){
        this.serviceBSetter = serviceBSetter;
    }

    public void setServiceBSetter(ServiceBSetter serviceBSetter) {
        this.serviceBSetter = serviceBSetter;
    }
}
//ServiceB
package com.haust.spring.aop.circular;

public class ServiceBSetter {
    private ServiceASetter serviceASetter;

    public ServiceBSetter(ServiceASetter serviceASetter) {
        this.serviceASetter = serviceASetter;
    }

    public ServiceBSetter() {
    }

    public void setServiceASetter(ServiceASetter serviceASetter) {
        this.serviceASetter = serviceASetter;
    }
}

```

- 默认的单例(singleton)的场景是支持循环依赖的，不报错
- 原型(Prototype)的场景是不支持循环依赖的，会报错 BeanCurrentlyInCreationException

#### spring三级缓存

singleton底层使用的是spring 的三层缓存来解决的循环依赖问题

只有单例的bean会通过三级缓存提前暴露来解决循环依赖的问题，而非单例的bean，每次从容器中获取都是一个新的对象，都会重新创建，所以非单例的bean是没有缓存的，不会将其放到三级缓存中。

如果每一次都需要new对象，则不存在缓存，所以无法解决循环依赖问题

```java
//第一级缓存，这里面放的都是已经实例化之后的singleton对象
private final Map<String, Object> singletonObjects = new ConcurrentHashMap(256);
//第三季缓存，这里面放的是用来创建对象的Factories
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap(16);
//这里面放的是早期的Singleton对象，并没有完成真正的初始化，有些属性可能还没有赋值
private final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap(16);
```

#### 循环依赖Debug

- 第一层singletonObjects存放的是已经初始化好了的Bean,
- 第二层earlySingletonObjects存放的是实例化了，但是未初始化的Bean,
- 第三层singletonFactories存放的是FactoryBean。假如A类实现了FactoryBean,那么依赖注入的时候不是A类，而是A类产生的Bean

实例说明：

- 1A创建过程中需要B，于是A将自己放到三级缓里面，去实例化B
- 2B实例化的时候发现需要A，于是B先查一级缓存，没有，再查二级缓存，还是没有，再查三级缓存，找到了A然后把三级缓存里面的这个A放到二级缓存里面，并删除三级缓存里面的A
- 3B顺利初始化完毕，将自己放到一级缓存里面（此时B里面的A依然是创建中状态)
- 然后回来接着创建A，此时B已经创建结束，直接从一级缓存里面拿到B，然后完成创建，并将A自己放到一级缓存里面。

第一步，加载一些文件和jar，这里可以跳过，直接来到bean的创建

```java
public AnnotationConfigApplicationContext(Class<?>... componentClasses) {
        this();
        this.register(componentClasses);
        this.refresh();
    }
```

第二步，查看refresh()

```java
public void refresh() throws BeansException, IllegalStateException {
    synchronized(this.startupShutdownMonitor) {
        StartupStep contextRefresh = this.applicationStartup.start("spring.context.refresh");
        this.prepareRefresh();
        ConfigurableListableBeanFactory beanFactory = this.obtainFreshBeanFactory();
        this.prepareBeanFactory(beanFactory);

        try {
            this.postProcessBeanFactory(beanFactory);
            StartupStep beanPostProcess = this.applicationStartup.start("spring.context.beans.post-process");
            this.invokeBeanFactoryPostProcessors(beanFactory);
            this.registerBeanPostProcessors(beanFactory);
            beanPostProcess.end();
            this.initMessageSource();
            this.initApplicationEventMulticaster();
            this.onRefresh();
            this.registerListeners();
            this.finishBeanFactoryInitialization(beanFactory);
            this.finishRefresh();
        } catch (BeansException var10) {
            if (this.logger.isWarnEnabled()) {
                this.logger.warn("Exception encountered during context initialization - cancelling refresh attempt: " + var10);
            }

            this.destroyBeans();
            this.cancelRefresh(var10);
            throw var10;
        } finally {
            this.resetCommonCaches();
            contextRefresh.end();
        }

    }
}
```

第三步，进入到finishBeanFactoryInitialization中，查看bean的初始化

```java
protected void finishBeanFactoryInitialization(ConfigurableListableBeanFactory beanFactory) {
        if (beanFactory.containsBean("conversionService") && beanFactory.isTypeMatch("conversionService", ConversionService.class)) {
            beanFactory.setConversionService((ConversionService)beanFactory.getBean("conversionService", ConversionService.class));
        }

        if (!beanFactory.hasEmbeddedValueResolver()) {
            beanFactory.addEmbeddedValueResolver((strVal) -> {
                return this.getEnvironment().resolvePlaceholders(strVal);
            });
        }

        String[] weaverAwareNames = beanFactory.getBeanNamesForType(LoadTimeWeaverAware.class, false, false);
        String[] var3 = weaverAwareNames;
        int var4 = weaverAwareNames.length;

        for(int var5 = 0; var5 < var4; ++var5) {
            String weaverAwareName = var3[var5];
            this.getBean(weaverAwareName);
        }

        beanFactory.setTempClassLoader((ClassLoader)null);
        beanFactory.freezeConfiguration();
        beanFactory.preInstantiateSingletons();
    }

```

第四步preInstantiateSingletons中，查看bean的初始化

```java
public void preInstantiateSingletons() throws BeansException {
        if (this.logger.isTraceEnabled()) {
            this.logger.trace("Pre-instantiating singletons in " + this);
        }
   //在这里有所有的初始化定义的bean，serviceA，和serviceB都在里面
        List<String> beanNames = new ArrayList(this.beanDefinitionNames);
 //此时var2中包含着所有的beanName
        Iterator var2 = beanNames.iterator();

        while(true) {
            String beanName;
            Object bean;
            do {
                while(true) {
                    RootBeanDefinition bd;
                    do {
                        do {
                            do {
                                if (!var2.hasNext()) {
                                    var2 = beanNames.iterator();

                                    while(var2.hasNext()) {
                                        beanName = (String)var2.next();
                                        Object singletonInstance = this.getSingleton(beanName);
                                        if (singletonInstance instanceof SmartInitializingSingleton) {
                                            StartupStep smartInitialize = this.getApplicationStartup().start("spring.beans.smart-initialize").tag("beanName", beanName);
                                            SmartInitializingSingleton smartSingleton = (SmartInitializingSingleton)singletonInstance;
                                            if (System.getSecurityManager() != null) {
                                                AccessController.doPrivileged(() -> {
                                                    smartSingleton.afterSingletonsInstantiated();
                                                    return null;
                                                }, this.getAccessControlContext());
                                            } else {
                                                smartSingleton.afterSingletonsInstantiated();
                                            }

                                            smartInitialize.end();
                                        }
                                    }

                                    return;
                                }

                                beanName = (String)var2.next();
                                bd = this.getMergedLocalBeanDefinition(beanName);
                            } while(bd.isAbstract());
                        } while(!bd.isSingleton());
                    } while(bd.isLazyInit());

                    if (this.isFactoryBean(beanName)) {
                        bean = this.getBean("&" + beanName);
                        break;
                    }
//拿到所有bean然后进行getBean
                    this.getBean(beanName);
                }
            } while(!(bean instanceof FactoryBean));

            FactoryBean<?> factory = (FactoryBean)bean;
            boolean isEagerInit;
            if (System.getSecurityManager() != null && factory instanceof SmartFactoryBean) {
                SmartFactoryBean var10000 = (SmartFactoryBean)factory;
                ((SmartFactoryBean)factory).getClass();
                isEagerInit = (Boolean)AccessController.doPrivileged(var10000::isEagerInit, this.getAccessControlContext());
            } else {
                isEagerInit = factory instanceof SmartFactoryBean && ((SmartFactoryBean)factory).isEagerInit();
            }

            if (isEagerInit) {
                this.getBean(beanName);
            }
        }
    }
```

第五步，进入到getBean中

```java
public Object getBean(String name) throws BeansException {
        return this.doGetBean(name, (Class)null, (Object[])null, false);
    }
```

第六步，执行底层逻辑doGetBean

```java
protected <T> T doGetBean(String name, @Nullable Class<T> requiredType, @Nullable Object[] args, boolean typeCheckOnly) throws BeansException {
    //查看当前初始化的bean是否存在别名
    String beanName = this.transformedBeanName(name);
    //获取beanName的实例化对象，第一次当然为空值
    Object sharedInstance = this.getSingleton(beanName);
    //定义bean的实例
    Object beanInstance;
    if (sharedInstance != null && args == null) {
        if (this.logger.isTraceEnabled()) {
            if (this.isSingletonCurrentlyInCreation(beanName)) {
                this.logger.trace("Returning eagerly cached instance of singleton bean '" + beanName + "' that is not fully initialized yet - a consequence of a circular reference");
            } else {
                this.logger.trace("Returning cached instance of singleton bean '" + beanName + "'");
            }
        }

        beanInstance = this.getObjectForBeanInstance(sharedInstance, name, beanName, (RootBeanDefinition)null);
    } else {
        //判断当前创建的对象是不是Prototype类型的，当然不是，这里是singleton类型
        if (this.isPrototypeCurrentlyInCreation(beanName)) {
            throw new BeanCurrentlyInCreationException(beanName);
        }
//获取创建父类的BeanFactory，当前类没有父类，并且是第一次创建，所以parentBeanFactory == null
        BeanFactory parentBeanFactory = this.getParentBeanFactory();
        
        if (parentBeanFactory != null && !this.containsBeanDefinition(beanName)) {
            String nameToLookup = this.originalBeanName(name);
            if (parentBeanFactory instanceof AbstractBeanFactory) {
                return ((AbstractBeanFactory)parentBeanFactory).doGetBean(nameToLookup, requiredType, args, typeCheckOnly);
            }

            if (args != null) {
                return parentBeanFactory.getBean(nameToLookup, args);
            }

            if (requiredType != null) {
                return parentBeanFactory.getBean(nameToLookup, requiredType);
            }

            return parentBeanFactory.getBean(nameToLookup);
        }

        //这里标记serviceA，代表着我已经见过这个对象了，我要把serviceA发到我的map里面了
        if (!typeCheckOnly) {
            this.markBeanAsCreated(beanName);
        }

        StartupStep beanCreation = this.applicationStartup.start("spring.beans.instantiate").tag("beanName", name);

        try {
            if (requiredType != null) {
                beanCreation.tag("beanType", requiredType::toString);
            }
//在spring中将所有的类都抽象成了一个RootBean
            RootBeanDefinition mbd = this.getMergedLocalBeanDefinition(beanName);
            this.checkMergedBeanDefinition(mbd, beanName, args);
            String[] dependsOn = mbd.getDependsOn();
            String[] var12;
            if (dependsOn != null) {
                var12 = dependsOn;
                int var13 = dependsOn.length;

                for(int var14 = 0; var14 < var13; ++var14) {
                    String dep = var12[var14];
                    if (this.isDependent(beanName, dep)) {
                        throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Circular depends-on relationship between '" + beanName + "' and '" + dep + "'");
                    }

                    this.registerDependentBean(dep, beanName);

                    try {
                        this.getBean(dep);
                    } catch (NoSuchBeanDefinitionException var31) {
                        throw new BeanCreationException(mbd.getResourceDescription(), beanName, "'" + beanName + "' depends on missing bean '" + dep + "'", var31);
                    }
                }
            }

            if (mbd.isSingleton()) {
                //这里再一次调用getSingleton
                sharedInstance = this.getSingleton(beanName, () -> {
                    try {
                        //在这里创建Bean对象
                        return this.createBean(beanName, mbd, args);
                    } catch (BeansException var5) {
                        this.destroySingleton(beanName);
                        throw var5;
                    }
                });
                beanInstance = this.getObjectForBeanInstance(sharedInstance, name, beanName, mbd);
            } else if (mbd.isPrototype()) {
                var12 = null;

                Object prototypeInstance;
                try {
                    this.beforePrototypeCreation(beanName);
                    prototypeInstance = this.createBean(beanName, mbd, args);
                } finally {
                    this.afterPrototypeCreation(beanName);
                }

                beanInstance = this.getObjectForBeanInstance(prototypeInstance, name, beanName, mbd);
            } else {
                String scopeName = mbd.getScope();
                if (!StringUtils.hasLength(scopeName)) {
                    throw new IllegalStateException("No scope name defined for bean '" + beanName + "'");
                }

                Scope scope = (Scope)this.scopes.get(scopeName);
                if (scope == null) {
                    throw new IllegalStateException("No Scope registered for scope name '" + scopeName + "'");
                }

                try {
                    Object scopedInstance = scope.get(beanName, () -> {
                        this.beforePrototypeCreation(beanName);

                        Object var4;
                        try {
                            var4 = this.createBean(beanName, mbd, args);
                        } finally {
                            this.afterPrototypeCreation(beanName);
                        }

                        return var4;
                    });
                    beanInstance = this.getObjectForBeanInstance(scopedInstance, name, beanName, mbd);
                } catch (IllegalStateException var30) {
                    throw new ScopeNotActiveException(beanName, scopeName, var30);
                }
            }
        } catch (BeansException var32) {
            beanCreation.tag("exception", var32.getClass().toString());
            beanCreation.tag("message", String.valueOf(var32.getMessage()));
            this.cleanupAfterBeanCreationFailure(beanName);
            throw var32;
        } finally {
            beanCreation.end();
        }
    }

    return this.adaptBeanInstance(name, beanInstance, requiredType);
}
```

第七步，查看getSingleton方法

```java
@Nullable
public Object getSingleton(String beanName) {
    return this.getSingleton(beanName, true);
}
```

第八步，查看当前实例的getSingleton

```java
public Object getSingleton(String beanName, ObjectFactory<?> singletonFactory) {
    //这里拿到beanName，必须要有beanName
        Assert.notNull(beanName, "Bean name must not be null");
    
        synchronized(this.singletonObjects) {
            //这里是一级缓存，从一级缓存中拿serviceA，因为第一次创建实例，这里并没有serviceA，所以singletonObject应该为null
            Object singletonObject = this.singletonObjects.get(beanName);
            //这里判断，当前对象是不是正在被销毁的实例
            if (singletonObject == null) {
                if (this.singletonsCurrentlyInDestruction) {
                    //当此工厂的单子处于销毁状态时，不允许创建单子Bean（不要在销毁方法中向BeanFactory请求一个Bean）。
                    throw new BeanCreationNotAllowedException(beanName, "Singleton bean creation not allowed while singletons of this factory are in destruction (Do not request a bean from a BeanFactory in a destroy method implementation!)");
                }
//创建单例对象'serviceA'的共享实例
                if (this.logger.isDebugEnabled()) {
                    this.logger.debug("Creating shared instance of singleton bean '" + beanName + "'");
                }

                this.beforeSingletonCreation(beanName);
                boolean newSingleton = false;
                boolean recordSuppressedExceptions = this.suppressedExceptions == null;
                if (recordSuppressedExceptions) {
                    this.suppressedExceptions = new LinkedHashSet();
                }

                try {
                    singletonObject = singletonFactory.getObject();
                    newSingleton = true;
                } catch (IllegalStateException var16) {
                    singletonObject = this.singletonObjects.get(beanName);
                    if (singletonObject == null) {
                        throw var16;
                    }
                } catch (BeanCreationException var17) {
                    BeanCreationException ex = var17;
                    if (recordSuppressedExceptions) {
                        Iterator var8 = this.suppressedExceptions.iterator();

                        while(var8.hasNext()) {
                            Exception suppressedException = (Exception)var8.next();
                            ex.addRelatedCause(suppressedException);
                        }
                    }

                    throw ex;
                } finally {
                    if (recordSuppressedExceptions) {
                        this.suppressedExceptions = null;
                    }

                    this.afterSingletonCreation(beanName);
                }

                if (newSingleton) {
                    this.addSingleton(beanName, singletonObject);
                }
            }

            return singletonObject;
        }
    }
```

执行createBean方法

```java
protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args) throws BeanCreationException {
        if (this.logger.isTraceEnabled()) {
            this.logger.trace("Creating instance of bean '" + beanName + "'");
        }

        RootBeanDefinition mbdToUse = mbd;
        Class<?> resolvedClass = this.resolveBeanClass(mbd, beanName, new Class[0]);
        if (resolvedClass != null && !mbd.hasBeanClass() && mbd.getBeanClassName() != null) {
            mbdToUse = new RootBeanDefinition(mbd);
            mbdToUse.setBeanClass(resolvedClass);
        }

        try {
            mbdToUse.prepareMethodOverrides();
        } catch (BeanDefinitionValidationException var9) {
            throw new BeanDefinitionStoreException(mbdToUse.getResourceDescription(), beanName, "Validation of method overrides failed", var9);
        }

        Object beanInstance;
        try {
            //解决实例化前的问题
            beanInstance = this.resolveBeforeInstantiation(beanName, mbdToUse);
            if (beanInstance != null) {
                return beanInstance;
            }
        } catch (Throwable var10) {
            throw new BeanCreationException(mbdToUse.getResourceDescription(), beanName, "BeanPostProcessor before instantiation of bean failed", var10);
        }

        try {
            //来到这里掉用doCreateBean方法
            beanInstance = this.doCreateBean(beanName, mbdToUse, args);
            if (this.logger.isTraceEnabled()) {
                this.logger.trace("Finished creating instance of bean '" + beanName + "'");
            }

            return beanInstance;
        } catch (ImplicitlyAppearedSingletonException | BeanCreationException var7) {
            throw var7;
        } catch (Throwable var8) {
            throw new BeanCreationException(mbdToUse.getResourceDescription(), beanName, "Unexpected exception during bean creation", var8);
        }
    }
```

接下来调用doCreateBean方法

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args) throws BeanCreationException {
    //bean包装器
    BeanWrapper instanceWrapper = null;
    if (mbd.isSingleton()) {
        instanceWrapper = (BeanWrapper)this.factoryBeanInstanceCache.remove(beanName);
    }

    if (instanceWrapper == null) {
        //对象在这里创建instance实例
        instanceWrapper = this.createBeanInstance(beanName, mbd, args);
    }

    Object bean = instanceWrapper.getWrappedInstance();
    Class<?> beanType = instanceWrapper.getWrappedClass();
    if (beanType != NullBean.class) {
        mbd.resolvedTargetType = beanType;
    }

    synchronized(mbd.postProcessingLock) {
        if (!mbd.postProcessed) {
            try {
                this.applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
            } catch (Throwable var17) {
                throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Post-processing of merged bean definition failed", var17);
            }

            mbd.postProcessed = true;
        }
    }

    boolean earlySingletonExposure = mbd.isSingleton() && this.allowCircularReferences && this.isSingletonCurrentlyInCreation(beanName);
    if (earlySingletonExposure) {
        if (this.logger.isTraceEnabled()) {
            this.logger.trace("Eagerly caching bean '" + beanName + "' to allow for resolving potential circular references");
        }

        this.addSingletonFactory(beanName, () -> {
            return this.getEarlyBeanReference(beanName, mbd, bean);
        });
    }

    Object exposedObject = bean;

    try {
        this.populateBean(beanName, mbd, instanceWrapper);
        exposedObject = this.initializeBean(beanName, exposedObject, mbd);
    } catch (Throwable var18) {
        if (var18 instanceof BeanCreationException && beanName.equals(((BeanCreationException)var18).getBeanName())) {
            throw (BeanCreationException)var18;
        }

        throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Initialization of bean failed", var18);
    }

    if (earlySingletonExposure) {
        Object earlySingletonReference = this.getSingleton(beanName, false);
        if (earlySingletonReference != null) {
            if (exposedObject == bean) {
                exposedObject = earlySingletonReference;
            } else if (!this.allowRawInjectionDespiteWrapping && this.hasDependentBean(beanName)) {
                String[] dependentBeans = this.getDependentBeans(beanName);
                Set<String> actualDependentBeans = new LinkedHashSet(dependentBeans.length);
                String[] var12 = dependentBeans;
                int var13 = dependentBeans.length;

                for(int var14 = 0; var14 < var13; ++var14) {
                    String dependentBean = var12[var14];
                    if (!this.removeSingletonIfCreatedForTypeCheckOnly(dependentBean)) {
                        actualDependentBeans.add(dependentBean);
                    }
                }

                if (!actualDependentBeans.isEmpty()) {
                    throw new BeanCurrentlyInCreationException(beanName, "Bean with name '" + beanName + "' has been injected into other beans [" + StringUtils.collectionToCommaDelimitedString(actualDependentBeans) + "] in its raw version as part of a circular reference, but has eventually been wrapped. This means that said other beans do not use the final version of the bean. This is often the result of over-eager type matching - consider using 'getBeanNamesForType' with the 'allowEagerInit' flag turned off, for example.");
                }
            }
        }
    }

    try {
        this.registerDisposableBeanIfNecessary(beanName, bean, mbd);
        return exposedObject;
    } catch (BeanDefinitionValidationException var16) {
        throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Invalid destruction signature", var16);
    }
}
```

接下来到createBeanInstance方法

```java
protected BeanWrapper createBeanInstance(String beanName, RootBeanDefinition mbd, @Nullable Object[] args) {
    //解析BeanClass
    Class<?> beanClass = this.resolveBeanClass(mbd, beanName, new Class[0]);
    //反射之前判断这个类是否存在，判断是不是共有的，是不是被允许访问
    if (beanClass != null && !Modifier.isPublic(beanClass.getModifiers()) && !mbd.isNonPublicAccessAllowed()) {
        throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Bean class isn't public, and non-public access not allowed: " + beanClass.getName());
    } else {
        //拿到父类的实例
        Supplier<?> instanceSupplier = mbd.getInstanceSupplier();
        if (instanceSupplier != null) {
            return this.obtainFromSupplier(instanceSupplier, beanName);
        } else if (mbd.getFactoryMethodName() != null) {
            return this.instantiateUsingFactoryMethod(beanName, mbd, args);
        } else {
            boolean resolved = false;
            boolean autowireNecessary = false;
            if (args == null) {
                synchronized(mbd.constructorArgumentLock) {
                    if (mbd.resolvedConstructorOrFactoryMethod != null) {
                        resolved = true;
                        autowireNecessary = mbd.constructorArgumentsResolved;
                    }
                }
            }

            if (resolved) {
                return autowireNecessary ? this.autowireConstructor(beanName, mbd, (Constructor[])null, (Object[])null) : this.instantiateBean(beanName, mbd);
            } else {
                Constructor<?>[] ctors = this.determineConstructorsFromBeanPostProcessors(beanClass, beanName);
                if (ctors == null && mbd.getResolvedAutowireMode() != 3 && !mbd.hasConstructorArgumentValues() && ObjectUtils.isEmpty(args)) {
                    ctors = mbd.getPreferredConstructors();
                    return ctors != null ? this.autowireConstructor(beanName, mbd, ctors, (Object[])null) : this.instantiateBean(beanName, mbd);
                } else {
                    return this.autowireConstructor(beanName, mbd, ctors, args);
                }
            }
        }
    }
}
```

紧接着进行instantiateBean方法

```java
protected BeanWrapper instantiateBean(String beanName, RootBeanDefinition mbd) {
        try {
            Object beanInstance;
            if (System.getSecurityManager() != null) {
                beanInstance = AccessController.doPrivileged(() -> {
                    return this.getInstantiationStrategy().instantiate(mbd, beanName, this);
                }, this.getAccessControlContext());
            } else {
                beanInstance = this.getInstantiationStrategy().instantiate(mbd, beanName, this);
            }

            BeanWrapper bw = new BeanWrapperImpl(beanInstance);
            this.initBeanWrapper(bw);
            return bw;
        } catch (Throwable var5) {
            throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Instantiation of bean failed", var5);
        }
    }
```

serviceA在创建的时候发现需要创建serviceB，所以将A暂时存到了singletonFactories中，在serviceA中调用了doCreateBean方法去创建serviceB，serviceB创建完之后this.populateBean(beanName, mbd, instanceWrapper);方法

```java
protected void populateBean(String beanName, RootBeanDefinition mbd, @Nullable BeanWrapper bw) {
    if (bw == null) {
        if (mbd.hasPropertyValues()) {
            throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Cannot apply property values to null instance");
        }
    } else {
        if (!mbd.isSynthetic() && this.hasInstantiationAwareBeanPostProcessors()) {
            Iterator var4 = this.getBeanPostProcessorCache().instantiationAware.iterator();

            while(var4.hasNext()) {
                InstantiationAwareBeanPostProcessor bp = (InstantiationAwareBeanPostProcessor)var4.next();
                if (!bp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
                    return;
                }
            }
        }

        PropertyValues pvs = mbd.hasPropertyValues() ? mbd.getPropertyValues() : null;
        int resolvedAutowireMode = mbd.getResolvedAutowireMode();
        if (resolvedAutowireMode == 1 || resolvedAutowireMode == 2) {
            MutablePropertyValues newPvs = new MutablePropertyValues((PropertyValues)pvs);
            if (resolvedAutowireMode == 1) {
                this.autowireByName(beanName, mbd, bw, newPvs);
            }

            if (resolvedAutowireMode == 2) {
                this.autowireByType(beanName, mbd, bw, newPvs);
            }

            pvs = newPvs;
        }

        boolean hasInstAwareBpps = this.hasInstantiationAwareBeanPostProcessors();
        boolean needsDepCheck = mbd.getDependencyCheck() != 0;
        PropertyDescriptor[] filteredPds = null;
        if (hasInstAwareBpps) {
            if (pvs == null) {
                pvs = mbd.getPropertyValues();
            }

            PropertyValues pvsToUse;
            for(Iterator var9 = this.getBeanPostProcessorCache().instantiationAware.iterator(); var9.hasNext(); pvs = pvsToUse) {
                InstantiationAwareBeanPostProcessor bp = (InstantiationAwareBeanPostProcessor)var9.next();
                pvsToUse = bp.postProcessProperties((PropertyValues)pvs, bw.getWrappedInstance(), beanName);
                if (pvsToUse == null) {
                    if (filteredPds == null) {
                        filteredPds = this.filterPropertyDescriptorsForDependencyCheck(bw, mbd.allowCaching);
                    }

                    pvsToUse = bp.postProcessPropertyValues((PropertyValues)pvs, filteredPds, bw.getWrappedInstance(), beanName);
                    if (pvsToUse == null) {
                        return;
                    }
                }
            }
        }

        if (needsDepCheck) {
            if (filteredPds == null) {
                filteredPds = this.filterPropertyDescriptorsForDependencyCheck(bw, mbd.allowCaching);
            }

            this.checkDependencies(beanName, mbd, filteredPds, (PropertyValues)pvs);
        }

        if (pvs != null) {
            this.applyPropertyValues(beanName, mbd, bw, (PropertyValues)pvs);
        }

    }
}
```

最后的最后，将创建完成的singletion对象加入到singletonObject中

```java
if (newSingleton) {
    this.addSingleton(beanName, singletonObject);
}

//addSingleton
protected void addSingleton(String beanName, Object singletonObject) {
        synchronized(this.singletonObjects) {
            this.singletonObjects.put(beanName, singletonObject);
            this.singletonFactories.remove(beanName);
            this.earlySingletonObjects.remove(beanName);
            this.registeredSingletons.add(beanName);
        }
    }
```

![image-20220326145910127](D:\JavaProject\面试题总结Images\image-20220326145910127.png)

Spring 创建 bean主要分为两个步骤，创建原始bean对象，接着去填充对象属性和初始化每次创建bean之前，我们都会从缓存中餐下有没有该bean，因为是单例，只能有一个
当我们创建 beanA的原始对象后，并把它放到三级缓存中，接下来就该填充对象属性了，这时候发现依赖了beanB，接着就又去创建beanB，同样的流程，创建完beanB填充属性时又发现它依赖了beanA又是同样的流程，
不同的是:
这时候可以在三级缓存中查到刚放进去的原始对象beanA，所以不需要继续创建，用它注入 beanB，完成 beanB的创建既然beanB创建好了，所以 beanA就可以完成填充属性的步骤了，接着执行剩下的逻辑，闭环完成

Spring解决循环依赖依靠的是Bean的“*中间态]这个概念，而这个中间态指的是已经实例化但还没初始化的状态……>半成品。实例化的过程又是通过构造器创建的，如果A还没创建好出来怎么可能提前曝光，所以构造器的循环依赖无法解决。
Spring为了解决单例的循环依赖问题，使用了三级缓存。
其中一级缓存为单例池〈 singletonObjects)
二级缓存为提前曝光对象( earlySingletonObjects)三级缓存为提前曝光对象工厂 ( singletonFactories） 。
假设A、B循环引用，实例化A的时候就将其放入三级缓存中，接着填充属性的时候，发现依赖了B，同样的流程也是实例化后放入三级缓存，按着去填充属性时又发现自己依赖A，这时候从缓存中查找到早期暴露的A，没有AOP代理的话，直接将A的原始对象注入B，完成B的初始化后，进行属性填充和初始化，这时候B完成后，就去完成剩下的A的步骤，如果有AOP代理，就进行AOP处理获取代理后的对象A，注入B，走剩下的流程。

- 1调用doGetBean()方法，想要获取beanA，于是调用getSingleton()方法从缓存中查找beanA
- 2在getSingleton()方法中，从一级缓存中查找，没有，返回null
- 3 doGetBean()方法中获取到的beanA)为null，于是走对应的处理逻辑，调用getSingleton()的重载方法（参数为ObjectFactory的)
- 4在getSingleton()方法中，先将beanA_name添加到一个集合中，用于标记该bean正在创建中。然后回调匿名内部类的creatBean方法
- 5进入AbstractAutowireCapableBeanFactory#ndoCreateBean，先反射调用构造器创建出beanA的实例，然后判断:是否为单例、是否允许提前暴露引用(对于单例一般为true)、是否正在创建中(即是否在第四步的集合中〉。判断为true则将beanA添加到【三级缓存】中
- 6对beanA进行属性填充，此时检测到beanA依赖于beanB，于是开始查找beanB
- 7调用doGetBean()方法，和上面beanA的过程一样，到缓存中查找beanB，没有则创建，然后给beanB填充属性
- 8此时beanB依赖于beanA，调用getSingleton()获取beanA，依次从一级、二级、三级缎存中找，此时从一级较任E"狄取到eanAS创建1L），心过创建工厂获取到singletonObject，此时这个singletonObject指向的就是上面在doCreateBean()方法中实例化的beanA
- 9这样beanB就获取到了beanA的依赖，于是beanB顺利完成实例化，并将beanA从三级缓存移动到二级缓存中
- 10随后beanA维续他的属性填充工作，此时也获取到了beanB，beanA也随之完成了创建，回到getSingleton()方法中继续向下执行，将beanA从二级缓存移动到一级缓存中

## Redis

### 常用的数据类型

- string 字符
- list 列表
- hash 散列
- set 集合
- zset 有序集合
- bitmap 位图
- HyperLogLog 统计
- GEO 地理
- Stream 流

### string类型

- set k1 v1
- get k1 v1
- 设置多个
- mset k1 v1 k2 v2 k3 v3
- mget k1 k2 k3
- m == more
- incr k1 递增一
- incrby key increment 递增指定数
- decr key 递减一
- decrby key increment 递减指定数
- strlen key获取字符串长度
- setnx key value 
- set key value [EX second] [PX millisecond] [NX|XX]

### map类型

map ===>> Map<String, Map<object, object>>

- hset key field value 设置hash的值
- hget key field 获取hash的值
- hmset key field value [key field value] [key field value] 批量设置
- hmget key field [field]  批量获取
- hgetall key 获取所有的
- hlen 获取某个key内的全部数量
- hdel 删除一个key

购物车场景

### list类型

- lpush key value[value...] 向列表左边添加元素
- rpush key value[value...] 向列表右边添加元素
- lrange key start stop 查看列表
- llen key 获取列表中元素中的个数

微信文章订阅公众号

### set类型

- [SADD](http://www.redis.cn/commands/sadd.html)

  - 添加一个或多个指定的member元素到集合的 key中.指定的一个或者多个元素member 如果已经在集合key中存在则忽略.如果集合key 不存在，则新建集合key,并添加member元素到集合key中.

    如果key 的类型不是集合则返回错误.

- [SCARD](http://www.redis.cn/commands/scard.html)

  - 返回集合存储的key的基数 (集合元素的数量).

- [SDIFF](http://www.redis.cn/commands/sdiff.html)

  - 返回一个集合与给定集合的差集的元素

- [SDIFFSTORE](http://www.redis.cn/commands/sdiffstore.html)

  - 该命令类似于 [SDIFF](http://www.redis.cn/commands/sdiff.html), 不同之处在于该命令不返回结果集，而是将结果存放在`destination`集合中.如果`destination`已经存在, 则将其覆盖重写.

- [SINTER](http://www.redis.cn/commands/sinter.html)

  - 返回指定所有的集合的成员的交集.

- [SINTERSTORE](http://www.redis.cn/commands/sinterstore.html)

  - 这个命令与[SINTER](http://www.redis.cn/commands/sinter.html)命令类似, 但是它并不是直接返回结果集,而是将结果保存在 destination集合中.如果destination 集合存在, 则会被重写.

- [SISMEMBER](http://www.redis.cn/commands/sismember.html)

  - 返回成员 member 是否是存储的集合 key的成员.

- [SMEMBERS](http://www.redis.cn/commands/smembers.html)

  - 返回key集合所有的元素.该命令的作用与使用一个参数的[SINTER](http://www.redis.cn/commands/sinter.html) 命令作用相同.

- [SMOVE](http://www.redis.cn/commands/smove.html)

  - 将member从source集合移动到destination集合中. 对于其他的客户端,在特定的时间元素将会作为source或者destination集合的成员出现.

    如果source 集合不存在或者不包含指定的元素,这smove命令不执行任何操作并且返回0.否则对象将会从source集合中移除，并添加到destination集合中去，如果destination集合已经存在该元素，则smove命令仅将该元素充source集合中移除. 如果source 和destination不是集合类型,则返回错误.

- [SPOP](http://www.redis.cn/commands/spop.html)

  - 从存储在`key`的集合中移除并返回一个或多个随机元素。

    此操作与`SRANDMEMBER`类似，它从一个集合中返回一个或多个随机元素，但不删除元素。

    `count`参数将在更高版本中提供，但是在2.6、2.8、3.0中不可用。

- [SRANDMEMBER](http://www.redis.cn/commands/srandmember.html)

  - 仅提供key参数，那么随机返回key集合中的一个元素.

    Redis 2.6开始，可以接受 count 参数，如果count是整数且小于元素的个数，返回含有 count 个不同的元素的数组，如果count是个整数且大于集合中元素的个数时，仅返回整个集合的所有元素，当count是负数，则会返回一个包含count的绝对值的个数元素的数组，如果count的绝对值大于元素的个数，则返回的结果集里会出现一个元素出现多次的情况.

    仅提供key参数时，该命令作用类似于SPOP命令，不同的是SPOP命令会将被选择的随机元素从集合中移除，而SRANDMEMBER仅仅是返回该随记元素，而不做任何操作.

- [SREM](http://www.redis.cn/commands/srem.html)

  - 在key集合中移除指定的元素. 如果指定的元素不是key集合中的元素则忽略 如果key集合不存在则被视为一个空的集合，该命令返回0.

    如果key的类型不是一个集合,则返回错误.

- [SSCAN](http://www.redis.cn/commands/sscan.html)

- [SUNION](http://www.redis.cn/commands/sunion.html)

  - 返回给定的多个集合的并集中的所有成员.

- [SUNIONSTORE](http://www.redis.cn/commands/sunionstore.html)

  - 该命令作用类似于[SUNION](http://www.redis.cn/commands/sunion.html)命令,不同的是它并不返回结果集,而是将结果存储在destination集合中.

    如果destination 已经存在,则将其覆盖.

微信抽奖 srandmember set1 [count]/spop set1 [count]/ scard set1 检查

微信朋友圈点赞 sadd userid:message member ,显示点赞的人smembers userid:message 

微博的共同关注

QQ内推

### zset类型

- [BZPOPMAX](http://www.redis.cn/commands/bzpopmax.html)
- [BZPOPMIN](http://www.redis.cn/commands/bzpopmin.html)
- [ZADD](http://www.redis.cn/commands/zadd.html)
- [ZCARD](http://www.redis.cn/commands/zcard.html)
- [ZCOUNT](http://www.redis.cn/commands/zcount.html)
- [ZINCRBY](http://www.redis.cn/commands/zincrby.html)
- [ZINTERSTORE](http://www.redis.cn/commands/zinterstore.html)
- [ZLEXCOUNT](http://www.redis.cn/commands/zlexcount.html)
- [ZPOPMAX](http://www.redis.cn/commands/zpopmax.html)
- [ZPOPMIN](http://www.redis.cn/commands/zpopmin.html)
- [ZRANGE](http://www.redis.cn/commands/zrange.html)
- [ZRANGEBYLEX](http://www.redis.cn/commands/zrangebylex.html)
- [ZRANGEBYSCORE](http://www.redis.cn/commands/zrangebyscore.html)
- [ZRANK](http://www.redis.cn/commands/zrank.html)
- [ZREM](http://www.redis.cn/commands/zrem.html)
- [ZREMRANGEBYLEX](http://www.redis.cn/commands/zremrangebylex.html)
- [ZREMRANGEBYRANK](http://www.redis.cn/commands/zremrangebyrank.html)
- [ZREMRANGEBYSCORE](http://www.redis.cn/commands/zremrangebyscore.html)
- [ZREVRANGE](http://www.redis.cn/commands/zrevrange.html)
- [ZREVRANGEBYLEX](http://www.redis.cn/commands/zrevrangebylex.html)
- [ZREVRANGEBYSCORE](http://www.redis.cn/commands/zrevrangebyscore.html)
- [ZREVRANK](http://www.redis.cn/commands/zrevrank.html)
- [ZSCAN](http://www.redis.cn/commands/zscan.html)
- [ZSCORE](http://www.redis.cn/commands/zscore.html)
- [ZUNIONSTORE](http://www.redis.cn/commands/zunionstore.html)

热搜可以使用

### redis分布式锁

- Redis除了拿来做缓存，你还见过基于Redis的什么用法?
- Redis 做分布式锁的时候有需要注意的问题?
- 如果是Redis是单点部署的，会带来什么问题?
- 集群模式下，比如主从模式，有没有什么问题呢?
- 那你简单的介绍一下Redlock 吧?
- 你简历上写redisson，你谈谈Redis分布式锁如何续期?看门狗知道吗?




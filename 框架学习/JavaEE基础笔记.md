# JavaEE基础笔记

## 1、8基本数据类型

Java中的数据类型分为2类

- 基本数据类型：boolean、char、byte、short、int、long、float、double
- 引用数据类型：除基本数据类型外，都是所谓的引用类型[数组、class类、接口]

| 数据类型 | 默认值   | 大小  |
| -------- | -------- | ----- |
| boolean  | false    | 1比特 |
| char     | '\u0000' | 2字节 |
| byte     | 0        | 1字节 |
| short    | 0        | 2字节 |
| int      | 0        | 4字节 |
| long     | 0L       | 8字节 |
| float    | 0.0f     | 4字节 |
| double   | 0.0      | 8字节 |

### 1、布尔

布尔(boolean)仅存储两个值：true和false，通常用于判断

```java
boolean flag = true;
```

### 2、byte

byte的取值范围在-128和127之间，包含127。最小值为-128，最大值为127，默认值为0。

```java
byte a = 10;
byte b = -10;
```

### 3、short

short 的取值范围在 -32,768 和 32,767 之间，包含 32,767。最小值为 -32,768，最大值为 32,767，默认值为 0。

```java
short s = 10000;
short r = -20000;
```

### 4、int

int 的取值范围在 -2,147,483,648（-2 ^ 31）和 2,147,483,647（2 ^ 31 -1）（含）之间，默认值为 0。如果没有特殊需求，整型数据就用 int。

```java
int a = 100000;
int b = -200000;
```

### 5、long

long 的取值范围在 -9,223,372,036,854,775,808(-2^63) 和 9,223,372,036,854,775,807(2^63 -1)（含）之间，默认值为 0。如果 int 存储不下，就用 long，整型数据就用 int。

```java
long a = 1000000L;
long b = -10000000L;
```

### 6、float

float 是单精度的浮点数，遵循 IEEE 754（二进制浮点数算术标准），取值范围是无限的，默认值为 0.0f。float 不适合用于精确的数值，比如说货币。

```java
float f1 = 5.34f;
```

### 7、double

double 是双精度的浮点数，遵循 IEEE 754（二进制浮点数算术标准），取值范围也是无限的，默认值为 0.0。double 同样不适合用于精确的数值，比如说货币。

```java
double d1 = 12.3
```

**那精确的数值用什么表示呢？最好使用 BigDecimal，它可以表示一个任意大小且精度完全准确的浮点数。针对货币类型的数值，也可以先乘以 100 转成整型进行处理。**

Tips：单精度是这样的格式，1 位符号，8 位指数，23 位小数，有效位数为 7 位。

​			双精度是这样的格式，1 位符号，11 位指数，52 为小数，有效位数为 16 位。

### 8、char

char 可以表示一个 16 位的 Unicode 字符，其值范围在 '\u0000'（0）和 '\uffff'（65,535）（包含）之间。

```java
char letterA = 'A'; // 英文单引号包括
```

## 2、流程控制语句

### 1、if-else相关

![img](JavaEE%E5%9F%BA%E7%A1%80%E7%AC%94%E8%AE%B0.assets/thirteen-01-16519799907233-16519799996205.png)

#### 1、if语句

if 语句的格式如下：

```java
if(布尔表达式){  
// 如果条件为 true，则执行这块代码
} 
```

示例：

```java
public class Demo001 {
    public static void main(String[] args) {

        int age =  20;
        if (age < 30) {
            System.out.println("青春年华");
        }

    }
}

青春年华
```

#### 2、if-else 语句

if-else 语句的格式如下:

```java
if(布尔表达式){  
// 条件为 true 时执行的代码块
}else{  
// 条件为 false  时执行的代码块
}  
```

示例：

```java
public class IfElseExample {
    public static void main(String[] args) {
        int age = 31;
        if (age < 30) {
            System.out.println("青春年华");
        } else {
            System.out.println("而立之年");
        }
    }
}
```

还有一个判断闰年（被 4 整除但不能被 100 整除或者被 400 整除）的例子：

```java
public class IfElseExample {
    public static void main(String[] args) {
        int year = 2020;
        if (((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0)) {
            System.out.println("闰年");
        } else {
            System.out.println("普通年份");
        }
    }
}
```

如果执行语句比较简单的话，可以使用三元运算符来代替 if-else 语句，如果条件为 true，返回 ? 后面 : 前面的值；如果条件为 false，返回 : 后面的值。

```java
public class IfElseExample {
    public static void main(String[] args) {
        int num = 21;
        System.out.println(num % 2 == 0 ? "偶数" : "奇数");
    }
}
```

## 3、Java变量的作用域

### 3.1、局部变量

在方法体内声明的变量被称为局部变量，该变量只能在该方法内使用，类中的其他方法并不知道该变量

声明局部变量时的注意事项：

- 局部变量声明在方法、构造方法或者语句块中。
- 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，将会被销毁。
- 访问修饰符不能用于局部变量。
- 局部变量只在声明它的方法、构造方法或者语句块中可见。
- 局部变量是在栈上分配的。
- 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。

### 3.2、成员变量

在类内部但在方法体外声明的变量称为成员变量，或者实例变量。之所以称为实例变量，是因为该变量只能通过类的实例（对象）来访问

声明成员变量时的注意事项：

- 成员变量声明在一个类中，但在方法、构造方法和语句块之外。
- 当一个对象被实例化之后，每个成员变量的值就跟着确定。
- 成员变量在对象创建的时候创建，在对象被销毁的时候销毁。
- 成员变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息。
- 成员变量可以声明在使用前或者使用后。
- 访问修饰符可以修饰成员变量。
- 成员变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把成员变量设为私有。通过使用访问修饰符可以使成员变量对子类可见；成员变量具有默认值。数值型变量的默认值是 0，布尔型变量的默认值是 false，引用类型变量的默认值是 null。变量的值可以在声明时指定，也可以在构造方法中指定。

### 3.3、静态变量

通过static关键字声明的变量被称为静态变量（类变量），它可以直接被类访问

声明静态变量时的注意事项：

- 静态变量在类中以static关键字声明，但必须在方法构造方法和语句块之外
- 无论一个类创建了多少个对象，类只拥有静态变量的一份拷贝
- 静态变量除了被声明为常量外很少使用
- 静态变量储存在静态储存区
- 静态变量在程序开始时创建，在程序结束时销毁
- 与成员变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型
- 静态变量的默认值和实例变量相似
- 静态变量可以在静态语句块中初始化

### 3.4、常量

在 Java 中，有些数据的值是不会发生改变的，这些数据被叫做常量——使用 final 关键字修饰的成员变量。常量的值一旦给定就无法改变！

常量在程序运行过程中主要作用有两个作用：

- 代表常数，便于修改
- 增强程序的可读性
- Java要求常量的变量名必须的大写

## 4、Java方法

### 4.1、方法

方法的声明反映了方法的一些信息，比如说可见性、返回类型、方法名和参数。

**访问权限：指定了方法的可见性。**

- public：该方法可以被所有类访问
- private：该方法只能定义它的类中访问
- protected：该方法可以被同一个包中的类访问，或者不同包中的子类访问
- default：该方法如果没有使用任何访问权限修饰符，Java 默认它使用 default 修饰符，该方法只能被同一个包中类可见

**返回类型：方法返回的数据类型，可以是基本数据类型、对象和集合，如果不需要返回数据，则使用void关键字**

**方法名：方法名最好反映出方法的功能，最好做到见名知意**

- **方法名最好是一个动词，并且以小写字母开头。如果方法名包含两个以上单词，那么第一个单词最好是动词，然后是形容词或者名词，并且要以驼峰式的命名方式命名。比如：**

	- 一个单词的方法名：`sum()`

	- 多个单词的方法名：`stringComparision()`
	- 一个方法可能与同一个类中的另外一个方法同名，这被称为方法重载

**参数：参数被放在一个圆括号内，如果有多个参数，可以使用逗号隔开。参数包括两部分，参数类型和参数名。没有参数时圆括号是空的。**

**方法签名：每个方法都有一个签名，包括方法名和参数**

**方法体：方法体放在一对花括号内用来执行特定的任务**

### 4.2、构造方法

创建构造方法的规则

**构造方法必须符合以下规则：**

- 构造方法的名字必须和类名一样
- 构造方法没有返回类型，包括void
- 构造方法不能是抽象的、静态的、最终的、同步的，也就是说，构造方法不能通过abstract、static、final、synchronized关键字修饰

**简单解析一下最后一条规则。**

- 由于构造方法不能被子类继承，所以用 final 和 abstract 修饰没有意义；
- 构造方法用于初始化一个对象，所以用 static 修饰没有意义；
- 多个线程不会同时创建内存地址相同的同一个对象，所以用 synchronized 修饰没有必要

### 4.3、代码初始化块

对象在创建的时候会执行代码初始化块。

可以通过代码初始化块执行一个更复杂的操作，比如为集合填充值。

```java
public class Bike {
    List<String> list;
    
    {
        list = new ArrayList<>();
        list.add("陈豪");
		list.add("chen");
    }
    
    public static void main(String[] args) {
        System.out.println(new Bike().list);
    }
}
```

对于代码初始化来说，有三个规则：

- 类实例化的时候执行代码初始化块
- 代码初始化块是放在构造方法中执行的，只不过位置比较靠前
- 代码初始化块里的执行顺序是从前到后的

### 4.4、接口

允许在接口中定义默认方法的理由很充分，因为一个接口可能有多个实现类，这些类就必须实现接口中定义的抽象类，否则编译器就会报错。假如我们需要在所有的实现类中追加某个具体的方法，在没有 `default` 方法的帮助下，我们就必须挨个对实现类进行修改。

由之前的例子我们就可以得出下面这些结论：

- 接口中允许定义变量
- 接口中允许定义抽象方法
- 接口中允许定义静态方法（Java 8 之后）
- 接口中允许定义默认方法（Java 8 之后）

## 5、Java中的关键字

### 5.1、static

static 关键字的作用：**方便在没有创建对象的情况下进行调用**，包括变量和方法。也就是说，只要类被加载了，就可以通过类名进行访问。

#### 5.1.1、静态变量

在声明变量的时候使用了 static 关键字，那么这个变量就被称为静态变量。静态变量只在类加载的时候获取一次内存空间，这使得静态变量很节省内存空间。

```java
public class Counter {
    /**
     * int count = 0;
     */
    static int count = 0;

    Counter() {
        count++;
        System.out.println(count);
    }

    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();
    }

}
// 输出为1 2 3
// 输出为1 1 1
```

#### 5.1.2、静态方法

方法上加了 static 关键字，那么它就是一个静态方法。

静态方法有以下这些特征。

- 静态方法属于这个类而不是这个类的对象
- 调用静态方法的时候不需要创建这个类的对象
- 静态方法可以访问静态变量

```java
public class StaticMethodStudent {
    String name;
    int age;
    static String school = "郑州大学";
    public StaticMethodStudent(String name, int age) {
        this.name = name;
        this.age = age;
    }
    static void change() {
        school = "河南大学";
    }
    void out() {
        System.out.println(name + " " + age + " " + school);
    }
    public static void main(String[] args) {
        StaticMethodStudent.change();
        StaticMethodStudent s1 = new StaticMethodStudent("陈浩", 20);
        StaticMethodStudent s2 = new StaticMethodStudent("陈浩", 20);

        s1.out();
        s2.out();
    }
}
```

#### 5.1.3、静态代码块

static 关键字，外加一个大括号括起来的代码被称为静态代码块。

```java
public class StaticBlock {
    static {
        System.out.println("静态代码块");
    }
    public static void main(String[] args) {
        System.out.println("main 方法");
    }
}
```

**静态代码块通常用来初始化一些静态变量，它会优先于 `main()` 方法执行。**

```java
public class StaticBlockDemo {
    public static List<String> writes = new ArrayList<>();
    static {
        writes.add("成");
        writes.add("功");
        writes.add("上");
        writes.add("岸");

        System.out.println("第一块");
    }
    static {
        writes.add("陈");
        writes.add("豪");

        System.out.println("第二块");
    }
}
```

writes 是一个静态的 ArrayList，所以不太可能在声明的时候完成初始化，因此需要在静态代码块中完成初始化。

**静态代码块在初始集合的时候，真的非常有用。在实际的项目开发中，通常使用静态代码块来加载配置文件到内存当中。**

### 5.2、this和super

this关键字的作用：

- 调用当前类的方法
- this()可以调用当前类的的构造方法
- this可以作为参数方法中传递
- this可以作为参数在方法中传递
- this可以作为方法的返回值，返回当前类的对象

#### 5.2.1、指向当前对象

```java
public class WithThisStudent {

    String name;
    int age;

    WithThisStudent(String name, int age) {
        // name = name;
        // age = age;
        this.name = name;
        this.age = age;
    }

    void out() {
        System.out.println(name + " " + age);
    }

    public static void main(String[] args) {
        WithThisStudent s1 = new WithThisStudent("java小豪", 22);
        WithThisStudent s2 = new WithThisStudent("Java", 20);

        s1.out();
        s2.out();
    }

}
```

#### 5.2.2、调用当前类的方法

```java
public class InvokeCurrentClassMethod {
    public InvokeCurrentClassMethod () {
    }

    void method1() {
    }

    void method2() {
        this.method1();
    }

    public static void main(String[] args) {
        (new InvokeCurrentClassMethod()).method1();
    }
}
```

**字节码文件为**

```java
public class InvokeCurrentClassMethod {
    public InvokeCurrentClassMethod() {
    }

    void method1() {
    }

    void method2() {
        this.method1();
    }

    public static void main(String[] args) {
        (new InvokeCurrentClassMethod()).method1();
    }
}
```

**在一个类中使用 this 关键字来调用另外一个方法，如果没有使用的话，编译器会自动帮我们加上。**

#### 5.2.3、调用当前类的构造方法

**示例：**

```java
// 在有参构造方法 InvokeConstrutor(int count) 中，使用了 this() 来调用无参构造方InvokeConstrutor()
public class InvokeConstrutor {
    InvokeConstrutor() {
        System.out.println("Hello");
    }
    InvokeConstrutor(int count) {
        this();
        System.out.println(count);
    }

    public static void main(String[] args) {
        InvokeConstrutor invokeConstrutor = new InvokeConstrutor(10);
    }
}
```

**也可以在无参构造方法中使用 `this()` 并传递参数来调用有参构造方法。**

```java
public class InvokeParamConstrutor {
    InvokeParamConstrutor() {
        this(10);
        System.out.println("hello");
    }

    InvokeParamConstrutor(int count) {
        System.out.println(count);
    }

    public static void main(String[] args) {
        InvokeParamConstrutor invokeConstrutor = new InvokeParamConstrutor();
    }
}
```

**注意：`this()` 必须放在构造方法的第一行，否则就报错了。**

#### 5.2.4、作为参数在方法中传递

```java
public class ThisAsParam {

    void method1(ThisAsParam p) {
        System.out.println(p);
    }
    void method2() {
        method1(this);
    }

    public static void main(String[] args) {
        ThisAsParam thisAsParam = new ThisAsParam();
        System.out.println(thisAsParam);
        thisAsParam.method2();
    }
}
```

**this在作为参数在方法中传递，它指向的是当前类的对象。**

#### 5.2.5、作为参数在构造方法中传递

```java
public class ThisAsConstrutorParam {
    int count = 10;

    ThisAsConstrutorParam() {
        Data data = new Data(this);
        data.out();
    }
    class Data {
        ThisAsConstrutorParam param;
        Data(ThisAsConstrutorParam param) {
            this.param = param;
        }
        void out() {
            System.out.println(param.count);
        }
    }
}
```

**`this` 关键字也可以作为参数在构造方法中传递，它指向的是当前类的对象。当我们需要在多个类中使用一个对象的时候，这非常有用。**

#### 5.2.6、作为方法的返回值

````java
public class ThisAsMethodResult {
    ThisAsMethodResult getThisAsMethodResult() {
        return this;
    }
    void out() {
        System.out.println("hello");
    }
    public static void main(String[] args) {
        new ThisAsMethodResult().getThisAsMethodResult().out();
    }
}
````

#### 5.2.7、super

**super 关键字的用法主要有三种：**

- 指向父类对象；
- 调用父类的方法；
- `super()` 可以调用父类的构造方法。

### 5.3、不可变对象

#### 5.3.1、什么是不可变类

一个类的对象在通过构造方法创建后如果状态不会再改变，那么它就是一个不可变类（immutable）类。它的所有成员变量的赋值仅在构造方法中完成，不会提供任何 setter 方法供外部类去修改。

**为了保护状态的原子性、可见性、有序性，我们程序员可以说是竭尽所能。其中，synchronized（同步）关键字是最简单最入门的一种解决方案。**

#### 5.3.2、常见的不可变类

1、常量池的需要

字符串常量池是 Java 堆内存中一个特殊的存储区域，当创建一个 String 对象时，假如此字符串在常量池中不存在，那么就创建一个；假如已经存，就不会再创建了，而是直接引用已经存在的对象。这样做能够减少 JVM 的内存开销，提高效率。

2、hashCode的需要

因为字符串是不可变的，所以在创建的时候，其hashCode的值就被缓存了，因此非常作为哈希值（比如作为HashMap的键），多次调用值返回同一个值，来提高效率。

3、线程安全

就像之前说的那样，如果对象的状态是可变的，那么在多线程环境下，就很容易造成不可预期的结果。而 String 是不可变的，就可以在多个线程之间共享，不需要同步处理。

## 6、深入理解Java泛型

泛型使用的方法一般有三种：

![image-20220514232522840](JavaEE%E5%9F%BA%E7%A1%80%E7%AC%94%E8%AE%B0.assets/image-20220514232522840.png)

- 泛型类

```java
public class Generic<T> {
    private T key;
    
    public Generic(T key) {
        this.key = key;
    }
    
    public T getKey() {
        return key;
    }
}
```

- 泛型接口

```java
public interface Generator<T> {
    public T method();
}
```

**实现泛型接口，指定类型**

```java
public class GeneratorImpl<T> implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```

- 泛型接口

```java
public static <E> void printArray(E[] inputArray) {
    for (E element : inputArray) {
        System.ot.printf("%s ", element);
    }
    Sysytem.out.println();
}
```

==使用==

```java
// 创建不同类型的数组类型：Integer,Double,Character
Integer[] intArray = {1, 2, 3};
String[] stringArray = {"Hello", "World"};
pringArray( intArray );
pringArray( stringArray );
```







==泛型常用的通配符有哪些？==

**常用的通配符为： T，E，K，V，？**

- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个 java 类型
- K V (key value) 分别代表 java 键值中的 Key Value
- E (element) 代表 Element




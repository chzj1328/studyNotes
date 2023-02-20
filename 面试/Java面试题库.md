# Java面试题库

## 23/02/18

### 错题解析

#### **6.若有下列定义，下列哪个表达式返回false？**(B)

```Java
String s = "hello";
String t = "hello";
char c[] = {'h', 'e', 'l', 'l', 'o'};
```

A：s.equals(t);

B：t.equals(c);

C：s==t;

D：t.equals(new String("hello"));

**解析：**s和t两个String类型变量都是常量池中的字符串，只有变量名不同是可以用双等号判断是否相等的，内存都是常量池中的字符串。

因此A，C正确，String类型重写了equals方法，用于判断字符串内容是否相等，t和new出来的"hello"内容显然是相等的，D正确。

String底层源码的equals()方法处有判断这个参数是不是String类的实例，如果不是则不执行判断直接返回false。B错误。

#### 7.子类要调用继承自父类的方法，必须使用super关键字。(B)

A：正确
B：错误

**解析：**

- 子类构造函数调用弗雷构造函数用super

- 子类重写父类方法后，若想调用父类中被重写的方法，用super

- 未被重写的方法可以直接调用
- 子类访问父类的私有方法用super

#### 8.以下代码的输出的正确结果是(8)

```java
public class Test {
    public static void main(String args[]) {
        String s = "祝你考出好成绩！";
        System.out.println(s.length());
    }
}
```

A：24

B：16

C：15

D：8

**解析：**

jdk1.8string底层是char[]数组，一个中文对应一个字符；jdk1.9string底层变成了byte[]数组，一个中文对应2个byte，长度是15。这里默认应该是1.8.

```java
public int length() {
    return value.length;
}
private final char value[];
```

![image-20230218222558683](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/blogImage/202302182226095.png)

#### 9.下面字段声明中哪一个在interface主体内是合法的?(B)

A：private final static int answer = 42;

B：public static int answer = 42;

C：final static answer = 42;

D：int answer;

**解析：**接口中的属性在不提供修饰符修饰的情况下，会自动加上public static final 

  注意（在1.8的编译器下可试）： 

  （1）属性不能用private，protected,default 修饰，因为默认是public 

  （2）如果属性是基本数据类型，需要赋初始值，若是引用类型，也需要初始化，因为默认有final修饰，必须赋初始值； 

  （3）接口中常规的来说不能够定义方法体，所以无法通过get和set方法获取属性值，所以属性不属于对象，属于类（接口），因为默认使用static修饰。

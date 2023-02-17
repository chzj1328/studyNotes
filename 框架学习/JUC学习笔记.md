# JUC

## 1、线程和进程

**获取CPU核数**

```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/15
 * @description 测试
 */
public class Test {
    public static void main(String[] args) {
        // 获取CPU核数
        // CPU 密集型，IO密集型
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
```



> 线程有几个状态

```java
public enum State {
  // 新生
  NEW,

  // 运行
  RUNNABLE,

  // 阻塞
  BLOCKED,

  // 等待
  WAITING,

  // 超时等待
  TIMED_WAITING,
  
  // 终止
  TERMINATED;
}
```



> wait/sleep区别

**1、来自不同的类**

wait => Object

sleep => Thread

**2、关于锁的释放**

wait会释放锁，sleep不会释放锁

**3、使用范围是不同的**

wait必须在同步代码块中

sleep可以在任何地方执行

**4、是否需要捕获异常**

wait 不需要捕获异常

sleep 必须要捕获异常

## 2、Lock锁

> 传统 Synchronized

**线程就是一个单独的资源类，没有任何附属操作:**

- 属性和方法

```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/15
 * @description 基本卖票
 */
public class SaleTicketDemo01 {
    public static void main(String[] args) {
        // 真正的多线程开发，公司中的开发
        // 线程就是一个单独的资源类，没有任何附属操作
        Ticket ticket = new Ticket();

        // lambda表达式 () -> {}
        new Thread(() -> {
            for (int i = 0; i < 60; i++) {
                ticket.sale();
            }
        }, "A").start();
        new Thread(() -> {
            for (int i = 0; i < 60; i++) {
                ticket.sale();
            }
        }, "B").start();
        new Thread(() -> {
            for (int i = 0; i < 60; i++) {
                ticket.sale();
            }
        }, "C").start();

    }
}

/**资源类  OOP编程**/
class Ticket {
    /**属性、方法**/
    private int number = 50;

    /**
     * 卖票方式
     * synchronized 本质：队列，锁
     */
    public synchronized void sale() {
        if (number > 0) {
            System.out.println(Thread.currentThread().getName() + "卖出了" + (number--) + "票，剩余："+ number);
        }
    }
}
```



> Lock 接口

![image-20221215225340368](JUC%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20221215225340368.png)

![image-20221215225823603](JUC%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20221215225823603-16711163125391.png)

**ReentrantLock的源码：**

![image-20221215230401704](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221215230401704.png)

公平锁：十分公平，可以先来后到

**非公平锁：十分不公平，可以插队（默认）**

**Lock锁实现线程安全：**

```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/15
 * @description 卖票2
 */
public class SaleTicketDemo02 {
    public static void main(String[] args) {
        // 真正的多线程开发，公司中的开发
        // 线程就是一个单独的资源类，没有任何附属操作
        Ticket2 ticket = new Ticket2();

        // lambda表达式 () -> {}
        new Thread(() -> { for (int i = 0; i < 60; i++) ticket.sale(); }, "A").start();
        new Thread(() -> { for (int i = 0; i < 60; i++) ticket.sale(); }, "B").start();
        new Thread(() -> { for (int i = 0; i < 60; i++) ticket.sale(); }, "C").start();
        
    }
}

/**
 * Lock三部曲
 * 1. new ReentrantLock();
 * 2. lock.Lock(); // 加锁
 * 3. finally => lock.unlock(); // 解锁
 */
class Ticket2 {
    /**属性、方法**/
    private int number = 50;

    Lock lock = new ReentrantLock();
    /**
     * 卖票方式
     * synchronized 本质：队列，锁
     */
    public void sale() {
        // 加锁
        lock.lock();
        try {
            // 业务代码
            if (number > 0) {
                System.out.println(Thread.currentThread().getName() + "卖出了" + (number--) + "票，剩余："+ number);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            // 解锁
            lock.unlock();
        }
    }
}
```



> Synchronized 和 Lock 区别

1、Synchronized 内置Java关键字，Lock一个Java类

2、Synchronized 无法判断锁的状态，Lock 可以判断是否获取到了锁

3、Synchronized  会自动释放锁，lock必须要手动释放锁！如果不释放锁，**就会产生死锁**

4、Synchronized 线程1(获得锁， 阻塞) 和 线程2(等待， 一直等待)，Lock锁就不一定会等待（lock.tryLock()；尝试获取锁）

5、Synchronized 可重入锁，不可以中断的，非公平的；Lock，可重入锁，可以判断锁，非公平（可以自己设置）

6、Synchronized 适合锁少量的代码同步问题，Lock适合锁大量的同步代码问题



> 锁是什么， 如何判断锁的是谁？





## 3、生产者和消费者问题

**面试必会：**单例模式、排序算法、生产者和消费者、死锁

> 生产者和消费者问题 Synchronized 版



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/15
 * @description 生产者和消费者测试1
 */
public class Test1 {
    // 线程之间的通信问题：生产者和消费者 等待唤醒和通知唤醒
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();
    }
}

/**
 * 判断等待， 业务， 通知
 */
class Data {

    private int number = 0;

    /**
     * +1
     */
    public synchronized void increment() throws InterruptedException {
        if (number != 0) {
            // 等待操作
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName() + "=>" + number);
        // 通知其他线程，我 +1 完毕了
        this.notifyAll();
    }

    /**
     * -1
     */
    public synchronized void decrement() throws InterruptedException {
        if (number == 0) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "=>" + number);
        // 通知其他线程，我 -1 完毕了
        this.notifyAll();
    }

}
```



> 问题存在， A、B、C、D

**存在虚假唤醒**

![image-20221215235137931](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221215235137931.png)

**if 改为 while**防止虚假唤醒

```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/15
 * @description 生产者和消费者测试1
 */
public class Test1 {
    // 线程之间的通信问题：生产者和消费者 等待唤醒和通知唤醒
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();

        new Thread(() -> {
            for (int i = 0; i <10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

/**
 * 判断等待， 业务， 通知
 */
class Data {

    private int number = 0;

    /**
     * +1
     */
    public synchronized void increment() throws InterruptedException {
        while (number != 0) {
            // 等待操作
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName() + "=>" + number);
        // 通知其他线程，我 +1 完毕了
        this.notifyAll();
    }

    /**
     * -1
     */
    public synchronized void decrement() throws InterruptedException {
        while (number == 0) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "=>" + number);
        // 通知其他线程，我 -1 完毕了
        this.notifyAll();
    }

}
```



> JUC版的生产者和消费者问题

通过Lock了解到Condition

![image-20221216000251305](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221216000251305.png)

**代码实现：**

```java
package com.chen.pc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description 生产者和消费者测试2
 */
public class Test2 {
    // 线程之间的通信问题：生产者和消费者 等待唤醒和通知唤醒
    public static void main(String[] args) {
        Data1 data = new Data1();


        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

/**
 * 判断等待， 业务， 通知
 */
class Data1 {

    private int number = 0;

    Lock lock = new ReentrantLock();

    Condition condition = lock.newCondition();



    /**
     * +1
     */
    public  void increment() throws InterruptedException {
        lock.lock();
        try {

            while (number != 0) {
                // 等待操作
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName() + "=>" + number);
            // 通知其他线程，我 +1 完毕了
            condition.signalAll();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }
    }

    /**
     * -1
     */
    public void decrement() throws InterruptedException {
        lock.lock();
        try {
            while (number == 0) {
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName() + "=>" + number);
            // 通知其他线程，我 -1 完毕了
            condition.signalAll();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }
    }

}
```



> Condition 精准的通知和唤醒线程

**代码实现：**

```java
package com.chen.pc;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description Condition实现精准唤醒和通知
 */
public class Test3 {
    public static void main(String[] args) {
        Data2 data = new Data2();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printA();
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printB();
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                data.printC();
            }
        }, "C").start();

    }
}

/**
 * A 执行完调用B， B执行完调用C， C执行完调用A
 */
class Data2 {


    private Lock lock = new ReentrantLock();

    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int number = 1;

    public void printA() {
        lock.lock();
        try {
            // 业务代码 判断 -> 执行 -> 通知
            while (number != 1) {
                // 等待
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName() + "AAAAAAA");
            // 唤醒B
            number = 2;
            condition2.signal();
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }

    }

    public void printB() {
        lock.lock();
        try {
            // 业务代码 判断 -> 执行 -> 通知
            while (number != 2) {
                // 等待
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName() + "BBBBBBB");
            number = 3;
            condition3.signal();
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }
    }

    public void printC() {
        lock.lock();
        try {
            // 业务代码 判断 -> 执行 -> 通知
            while (number != 3) {
                // 等待
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName() + "CCCCCCC");
            number = 1;
            condition1.signal();
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }
    }
}
```



## 4、8锁现象



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description 8锁现象的八个问题
 * <p>
 *     1、标准情况下，两个线程先打印 发短信还是 打电话？ 1/发短信 2/打电话
 *     2、sendSms延迟4秒，两个线程先打印 发短信还是 打电话？ 1/发短信 2/打电话
 * </p>
 */
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        // 锁的存在
        new Thread(() -> {
            phone.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        new Thread(() -> {
            phone.call();
        }, "B").start();
    }
}


class Phone {
    // synchronized 锁的对象是方法调用者
    // 两个方法用的是同一个锁，谁想拿到谁执行
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
}
```



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description 8锁现象解释
 * <p>
 *     3、增加了一个普通方法，先执行发短信还是Hello 普通方法
 *     4、两个对象，两个同步方法                  //打电话
 * </p>
 */
public class Test2 {
    public static void main(String[] args) {
        // 两个对象
        Phone2 phone = new Phone2();
        Phone2 phone2 = new Phone2();

        // 锁的存在
        new Thread(() -> {
            phone.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        new Thread(() -> {
            phone2.call();
        }, "B").start();
    }
}


class Phone2 {
    /**
     * synchronized 锁的对象是方法调用者
     */
    public synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }

    /**
     * 这里没有锁，不是同步方法，不受锁的影响
     */
    public void hello() {
        System.out.println("hello");
    }
}
```



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description 8锁现象测试
 * <p>
 *      5、增加两个静态的同步方法, 只有一个对象先打印 发短信？ 打电话？   // 发短信
 *      6、两个对象！增加两个静态的同步方法，先打印 发短信？ 打电话？     // 发短信
 * </p>
 */
public class Test3 {
    public static void main(String[] args) {

        Phone3 phone1 = new Phone3();
        Phone3 phone2 = new Phone3();

        // 锁的存在
        new Thread(() -> {
            phone1.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        new Thread(() -> {
            phone2.call();
        }, "B").start();
    }
}


class Phone3 {
    /**
     * synchronized 锁的对象是方法调用者
     * static 静态方法
     * 类一加载就有了！ 锁的是Class
     */
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");
    }
    public static synchronized void call() {
        System.out.println("打电话");
    }
}
```



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description 8锁现象解释
 * <p>
 *     7、一个静态同步方法，一个普通同步方法， 一个对象 先打印 发短信？ 打电话？ // 打电话
 *     8、一个静态同步方法，一个普通同步方法， 两个对象 先打印 发短信？ 打电话？ // 打电话
 * </p>
 */
public class Test4 {
    public static void main(String[] args) {

        Phone4 phone1 = new Phone4();
        Phone4 phone2 = new Phone4();

        // 锁的存在
        new Thread(() -> {
            phone1.sendSms();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        new Thread(() -> {
            phone2.call();
        }, "B").start();
    }
}


class Phone4 {
    /**
     * 静态的同步方法
     * 锁的Class类模板
     */
    public static synchronized void sendSms() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("发短信");
    }

    /**
     * 普通的同步方法
     * 锁的调用者
     */
    public synchronized void call() {
        System.out.println("打电话");
    }
}
```



> 小结

new this 具体的一个手机

static Class 唯一的一个手机模板

## 5、集合类不安全

> List 不安全



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/16
 * @description List不安全
 * <p>
 *     java.util.ConcurrentModificationException 并发修改异常！
 * </p>
 */
public class ListTest {
    public static void main(String[] args) {
        // 并发下 ArrayList 不安全的
        // List<String> list1 = new ArrayList<>();

        /**
         * 解决方法：
         *  1.List<String> list = new Vector<>();
         *  2.List<String> list = Collections.synchronizedList(new ArrayList<>());
         *  3.List<String> list = new CopyOnWriteArrayList<>();
         */
        // CopyOnWrite 写入时复制 COW 计算机程序设计领域的优化策略
        // 多个线程调用的时候，list 读取的时候，固定的， 写入（覆盖）
        // 写入的时候避免覆盖，造成数据问题
        List<String> list = new CopyOnWriteArrayList<>();
        for (int i = 1; i <= 10; i++) {
            new Thread(() -> {
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            }, String.valueOf(i)).start();
        }
    }
}
```



> Set 不安全



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/17
 * @description Set不安全的
 * <p>
 *     1、java.util.ConcurrentModificationException 并发修改异常！
 *     解决方法：
 *      1、Set<String> set = Collections.synchronizedSet(new HashSet<>());
 *      2、
 * </p>
 */
public class SetTest {
    public static void main(String[] args) {
//        Set<String> set = new HashSet<>();
//        Set<String> set = Collections.synchronizedSet(new HashSet<>());
        Set<String> set = new CopyOnWriteArraySet<>();

        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            }, String.valueOf(i)).start();
        }
    }
}
```



> HashSet 底层是什么？

```java

private transient HashMap<E,Object> map;

public HashSet() {
  map = new HashMap<>();
}
// add set 本质就是 map key是无法重复的
public boolean add(E e) {
  return map.put(e, PRESENT)==null;
}
```



> HashMap 不安全



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/17
 * @description HashMap不安全
 * <p>
 *     java.util.ConcurrentModificationException
 *     解决方法：
 *          1、Map<String, String> map = new ConcurrentHashMap<>();
 * </p>
 */
public class MapTest {
    public static void main(String[] args) {
        // map是这样用的吗？ 工作中不用HashMap
        // 默认等价于什么？ new HashMap<>(16, 0.75)
        // Map<String, Object> map = new HashMap<>();

        Map<String, String> map = new ConcurrentHashMap<>();

        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            }, String.valueOf(i)).start();
        }
    }
}
```



> ConcurrentHashMap的原理

```java
/**
 * @throws NullPointerException if the specified key or value is null
 */
public V put(@NotNull K key, @NotNull V value) {
  return putVal(key, value, false);
}
```



## 6、Callable

![image-20221220193035760](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221220193035760.png)

1、可以有返回值

2、可以抛出异常

3、方法不同 run()/call()



> 代码测试

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description Callable测试类
 */
public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // new Thread(new Runnable()).start();
        // new Thread(new FutureTask<V>()).start();
        // new Thread(new FutureTask<V>( Callable )).start();
        new Thread().start(); // 怎么启动Callable

        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread);
        new Thread(futureTask, "A").start();
        // 结果会被缓存，效率高
        new Thread(futureTask, "B").start();

        // 该get方法可能会产生阻塞！把他放在最后、或者使用一步通信来处理
        Integer o = (Integer) futureTask.get();  // 获取callable的返回结果
        System.out.println(o);
    }
}

class MyThread implements Callable<Integer> {

    /**
     * Computes a result, or throws an exception if unable to do so
     */
    @Override
    public Integer call() {
        System.out.println("call()");
        return 1024;
    }
}
```



## 7、常用的辅助类

### 7.1、CountDownLatch

- 允许一个或多个线程等待直到在其他线程中执行的一组操作完成的同步辅助。
- A `CountDownLatch`用给定的*计数*初始化。 `await`方法阻塞，直到由于`countDown()`方法的调用而导致当前计数达到零，之后所有等待线程被释放，并且任何后续的`await`  调用立即返回。  这是一个一次性的现象 - 计数无法重置。 如果您需要重置计数的版本，请考虑使用`CyclicBarrier`  。 
- A `CountDownLatch`是一种通用的同步工具，可用于多种用途。  一个`CountDownLatch`为一个计数的CountDownLatch用作一个简单的开/关锁存器，或者门：所有线程调用`await`在门口等待，直到被调用`countDown()`的线程打开。  一个`CountDownLatch`初始化*N*可以用来做一个线程等待，直到*N个*线程完成某项操作，或某些动作已经完成N次。 
- `CountDownLatch`一个有用的属性是，它不要求调用`countDown`线程等待计数到达零之前继续，它只是阻止任何线程通过`await`，直到所有线程可以通过。 

```java
import java.util.concurrent.CountDownLatch;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description CountDownLatch测试
 */
public class CountDownLatchTest {
    public static void main(String[] args) throws InterruptedException {
        // 总数是6， 必须要执行任务的时候，再使用！
        CountDownLatch downLatch = new CountDownLatch(6);
        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + "Go out");
                downLatch.countDown();  // 数量 -1
            }, String.valueOf(i)).start();
        }

        downLatch.await(); // 等待计数器归0，然后再向下执行

        System.out.println("Close Door");
    }
}
```

`downLatch.countDown(); `// 数量-1

`downLatch.await();` // 等待计数器归0，然后再向下执行

每次有线程调用 downLatch()数量-1，假设计数器变为0， downLatch.await()就会被唤醒，继续执行

### 7.2、CyclicBarrier

- 允许一组线程全部等待彼此达到共同屏障点的同步辅助。循环阻塞在涉及固定大小的线程方的程序中很有用，这些线程必须偶尔等待彼此。屏障被称为*循环*  ，因为它可以在等待的线程被释放之后重新使用。
- A `CyclicBarrier`支持一个可选的`Runnable`命令，每个屏障点运行一次，在派对中的最后一个线程到达之后，但在任何线程释放之前。  在任何一方继续进行之前，此*屏障操作*对更新共享状态很有用。

```java'
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description CyclicBarrier测试
 */
public class CyclicBarrierTest {
    public static void main(String[] args) {
        /**
         * 集齐 7 颗龙珠召唤神龙
         */
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7, () -> {
            System.out.println("召唤神龙成功");
        });

        for (int i = 1; i <= 7; i++) {
            final int temp = i;
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + "收集" + temp + "颗龙珠");
                try {
                    cyclicBarrier.await(); // 等待
                } catch (InterruptedException | BrokenBarrierException e) {
                    throw new RuntimeException(e);
                }
            }).start();
        }
    }
}
```



### 7.3、Semaphore

- 一个计数信号量。在概念上，信号量维持一组许可证。如果有必要，每个`acquire()`都会阻塞，直到许可证可用，然后才能使用它。每个`release()`添加许可证，潜在地释放阻塞获取方。但是，没有使用实际的许可证对象;`Semaphore`只保留可用数量的计数，并相应地执行。
- 信号量通常用于限制线程数，而不是访问某些（物理或逻辑）资源。
- 在获得项目之前，每个线程必须从信号量获取许可证，以确保某个项目可用。  当线程完成该项目后，它将返回到池中，并将许可证返回到信号量，允许另一个线程获取该项目。 请注意，当[调用`acquire()`时，不会保持同步锁定，因为这将阻止某个项目返回到池中。  信号量封装了限制对池的访问所需的同步，与保持池本身一致性所需的任何同步分开。 
- 此类的构造函数可选择接受*公平*参数。  当设置为false时，此类不会保证线程获取许可的顺序。 特别是，  *闯入*是允许的，也就是说，一个线程调用`acquire()`可以提前已经等待线程分配的许可证-在等待线程队列的头部逻辑新的线程将自己。  当公平设置为真时，信号量保证调用`acquire`方法的线程被选择以按照它们调用这些方法的顺序获得许可（先进先出;  FIFO）。 请注意，FIFO排序必须适用于这些方法中的特定内部执行点。  因此，一个线程可以在另一个线程之前调用`acquire`  ，但是在另一个线程之后到达排序点，并且类似地从方法返回。 另请注意， 未定义的`tryAcquire`方法不符合公平性设置，但将采取任何可用的许可证。 
- 通常，用于控制资源访问的信号量应该被公平地初始化，以确保线程没有被访问资源。  当使用信号量进行其他类型的同步控制时，非正常排序的吞吐量优势往往超过公平性。 

```java
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description Semaphore测试
 */
public class SemaphoreTest {
    public static void main(String[] args) {
        // 线程数量：停车位
        Semaphore semaphore = new Semaphore(3);

        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                // acquire() 得到
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName() + "抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName() + "离开车位");
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                } finally {
                    // release() 释放
                    semaphore.release();
                }
            }, String.valueOf(i)).start();
        }
    }
}
```

`semaphore.acquire();`获得，假设如果已经满了，等待，等待被释放为止

`semaphore.release();`释放，会将当前的信号量释放 +1 ，然后唤醒等待的线程



## 8、读写锁

- A `ReadWriteLock`维护一对关联的`locks` ，一个用于只读操作，一个用于写入。`read lock`可以由多个阅读器线程同时进行，只要没有作者。 `write lock`是独家的。
- 所有`ReadWriteLock`实现必须保证的存储器同步效应`writeLock`操作（如在指定`Lock`接口）也保持相对于所述相关联的`readLock`  。 也就是说，一个线程成功获取读锁定将会看到在之前发布的写锁定所做的所有更新。 
- 读写锁允许访问共享数据时的并发性高于互斥锁所允许的并发性。 它利用了这样一个事实：一次只有一个线程（  *写入*线程）可以修改共享数据，在许多情况下，任何数量的线程都可以同时读取数据（因此*读取器*线程）。  从理论上讲，通过使用读写锁允许的并发性增加将导致性能改进超过使用互斥锁。  实际上，并发性的增加只能在多处理器上完全实现，然后只有在共享数据的访问模式是合适的时才可以。 

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description ReadWriteLock测试
 * <p>
 *     独占锁(写锁)    一次只能被一个线程占有
 *     共享锁(读锁)    多个线程可以同时占有
 *     读-读  可以共存
 *     读-写  不能共存
 *     写-写  不能共存
 * </p>
 */
public class ReadWriteLockTest {
    public static void main(String[] args) {

        MyCacheLock myCache = new MyCacheLock();

        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.put(temp + "", temp + "");
            }, String.valueOf(i)).start();
        }

        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.get(temp + "");
            }, String.valueOf(i)).start();
        }

    }
}

/**
 * 加锁的
 */
class MyCacheLock {
    private volatile Map<String, Object> map = new HashMap<>();

    /**读写锁，更细粒度的读写锁**/
    private ReadWriteLock lock = new ReentrantReadWriteLock();

    // 存，写，只希望同时只有一个线程写
    public void put(String key, Object value) {
        lock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "写入" + key);
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + "写入完毕");
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.writeLock().unlock();
        }
    }

    // 取，读，所有人都可以读
    public void get(String key) {
        lock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "读取" + key);
            map.get(key);
            System.out.println(Thread.currentThread().getName() + "读取OK");
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.readLock().unlock();
        }
    }
}

class MyCache {
    private volatile Map<String, Object> map = new HashMap<>();
    // 存，写
    public void put(String key, Object value) {
        System.out.println(Thread.currentThread().getName() + "写入" + key);
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "写入完毕");
    }

    // 取，读
    public void get(String key) {
        System.out.println(Thread.currentThread().getName() + "读取" + key);
        map.get(key);
        System.out.println(Thread.currentThread().getName() + "读取OK");
    }
}
```



## 9、阻塞队列

阻塞队列：

![image-20221220214834089](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221220214834089.png)

**BlockingQueue**

![image-20221220220330621](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221220220330621.png)

**使用队列**：添加、移除

- 四组API

| 方式         | 抛出异常  | 不会抛出异常，有返回值 | 阻塞等待 | 超时等待  |
| ------------ | --------- | ---------------------- | -------- | --------- |
| 添加         | add()     | offer()                | put()    | offer(,,) |
| 删除         | remove()  | poll()                 | take()   | poll(,)   |
| 判断队列队头 | element() | peek()                 | -        | -         |

**抛出异常：**

```java
import java.util.concurrent.ArrayBlockingQueue;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/20
 * @description 测试
 */
public class ArrayBlockingQueue1 {
    public static void main(String[] args) {
        test1();
    }

    /**
     * 抛出异常
     */
    public static void test1() {
        // 队列的大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));

        // java.lang.IllegalStateException: Queue full 抛出异常！
        // System.out.println(blockingQueue.add("d"));
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());

        // java.util.NoSuchElementException 抛出异常！
        // System.out.println(blockingQueue.remove());
    }
}
```

**有返回值，不抛出异常：**

```java
/**
 * 有返回值，不抛出异常
 */
public static void test2() {
    // 队列的大小
    ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

    System.out.println(blockingQueue.offer("a"));
    System.out.println(blockingQueue.offer("b"));
    System.out.println(blockingQueue.offer("c"));
    // System.out.println(blockingQueue.offer("d"));  // FALSE！ 不抛异常

    System.out.println(blockingQueue.poll());
    System.out.println(blockingQueue.poll());
    System.out.println(blockingQueue.poll());
    System.out.println(blockingQueue.poll());  // null 不抛出异常
}
```

**阻塞等待：**

```java
/**
 * 等待，阻塞（一直阻塞）
 */
public static void test3() throws InterruptedException {
    // 队列的大小
    ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

    // 一直阻塞
    blockingQueue.put("a");
    blockingQueue.put("b");
    blockingQueue.put("c");
    // blockingQueue.put("d"); // 队列没有位置了,一直阻塞

    System.out.println(blockingQueue.take());
    System.out.println(blockingQueue.take());
    System.out.println(blockingQueue.take());
    // System.out.println(blockingQueue.take()); // 队列为空，一直阻塞
}
```

**超时等待，自动退出：**

```java
/**
 * 等待，阻塞（超时退出）
 */
public static void test4() throws InterruptedException {
    // 队列的大小
    ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

    blockingQueue.offer("a");
    blockingQueue.offer("b");
    blockingQueue.offer("c");
    blockingQueue.offer("d",2, TimeUnit.SECONDS); // 等待超过两秒就退出

    System.out.println(blockingQueue.poll());
    System.out.println(blockingQueue.poll());
    System.out.println(blockingQueue.poll());
    blockingQueue.poll(2, TimeUnit.SECONDS);
}
```



> SynchronousQueue 同步队列



```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;
import java.util.concurrent.TimeUnit;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description
 * <p>
 *     同步队列 和其他的BlockingQueue 不一样，SynchronousQueue 不存储元素
 *     put 一个元素，就必须从里面先take取出来，否则不能在put进去值
 * </p>
 */
public class SynchronousQueueTest {
    public static void main(String[] args) {

        // 同步队列
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>();

        new Thread(() -> {
            try {
                System.out.println(Thread.currentThread().getName() + " put 1");
                blockingQueue.put("1");
                System.out.println(Thread.currentThread().getName() + " put 2");
                blockingQueue.put("2");
                System.out.println(Thread.currentThread().getName() + " put 3");
                blockingQueue.put("3");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }, "T1").start();

        new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + " = " +blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + " = " +blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + " = " +blockingQueue.take());
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }, "T2").start();
    }
}
```



## 10、线程池（重点）

线程池：三大方法、7大参数、4种拒绝策略

**线程池的好处：**

1、降低资源的消耗

2、提高相应的速度

3、方便管理

==线程复用、可以控制最大并发数，管理线程==



> 线程池：三大方法



```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description 线程池的使用
 */
public class Demo01 {
    public static void main(String[] args) {
        // 单个线程
        // ExecutorService threadPool = Executors.newSingleThreadExecutor();
        // 创建一个固定的线程池的大小
        // ExecutorService threadPool = Executors.newFixedThreadPool(5);
        // 可伸缩的，遇强则强，遇弱则弱
        ExecutorService threadPool = Executors.newCachedThreadPool();

        try {
            for (int i = 0; i < 10; i++) {
                // 使用了线程池后，使用线程池来创建线程
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " OK");
                });
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            // 使用线程池之后一定要关闭
            threadPool.shutdown();
        }
    }
}
```



> 7大参数

```java
public static ExecutorService newSingleThreadExecutor() {
  return new FinalizableDelegatedExecutorService
    (new ThreadPoolExecutor(1, 1,
                            0L, TimeUnit.MILLISECONDS,
                            new LinkedBlockingQueue<Runnable>()));
}
public static ExecutorService newFixedThreadPool(int nThreads) {
  return new ThreadPoolExecutor(nThreads, nThreads,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>());
}
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}

// 本质ThreadPoolExecutor
public ThreadPoolExecutor(int corePoolSize, // 核心线程池大小
                          int maximumPoolSize, // 最大核心线程池大小
                          long keepAliveTime,  // 超时了没有人调用就会释放
                          TimeUnit unit, // 超时单位
                          BlockingQueue<Runnable> workQueue, // 阻塞队列
                          ThreadFactory threadFactory, // 线程工厂
                          RejectedExecutionHandler handler // 拒绝策略) {
  if (corePoolSize < 0 ||
      maximumPoolSize <= 0 ||
      maximumPoolSize < corePoolSize ||
      keepAliveTime < 0)
    throw new IllegalArgumentException();
  if (workQueue == null || threadFactory == null || handler == null)
    throw new NullPointerException();
  this.acc = System.getSecurityManager() == null ?
    null :
  AccessController.getContext();
  this.corePoolSize = corePoolSize;
  this.maximumPoolSize = maximumPoolSize;
  this.workQueue = workQueue;
  this.keepAliveTime = unit.toNanos(keepAliveTime);
  this.threadFactory = threadFactory;
  this.handler = handler;
}
```



> 手动创建线程池



```java
import java.util.concurrent.*;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description 线程池的使用
 * <p>
 *     new ThreadPoolExecutor.AbortPolicy(): 线程池满了之后，抛出异常
 *     new ThreadPoolExecutor.CallerRunsPolicy(): 当前线程执行
 *     new ThreadPoolExecutor.DiscardPolicy(): 队列满了，丢掉任务，不会抛出异常
 *     new ThreadPoolExecutor.DiscardOldestPolicy(): 队列满了,尝试和最早的去竞争，也不会抛出异常
 * </p>
 */
public class Demo01 {
    public static void main(String[] args) {
        // 单个线程
        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy());

        try {
            // 最大承载： Deque + max
            // 超过 java.util.concurrent.RejectedExecutionException
            for (int i = 1; i <= 9; i++) {
                // 使用了线程池后，使用线程池来创建线程
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " OK");
                });
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            // 使用线程池之后一定要关闭
            threadPool.shutdown();
        }
    }
}
```



> 四种拒绝策略

```java
// new ThreadPoolExecutor.AbortPolicy(): 线程池满了之后，抛出异常
// new ThreadPoolExecutor.CallerRunsPolicy(): 当前线程执行
// new ThreadPoolExecutor.DiscardPolicy(): 队列满了，丢掉任务，不会抛出异常
// new ThreadPoolExecutor.DiscardOldestPolicy(): 队列满了,尝试和最早的去竞争，也不会抛出异常
```



**最大线程如何定义：**

- CPU密集型：Runtime.getRuntime().availableProcessors(), 
- IO密集型： 判断程序中十分耗IO的线程，



## 11、四大函数式接口

> 函数式接口：只有一个方法的接口

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
// foreach(消费者的函数式接口)
```



**测试函数式(Function)接口：**

![image-20221221220850710](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221221220850710.png)

```java
import java.util.function.Function;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description
 * <p>
 *     Function 函数型接口,有一个参数，一个返回类型
 * </p>
 */
public class Demo01 {
    public static void main(String[] args) {
        /*Function<String, String> function = new Function<String, String>() {
            @Override
            public String apply(String str) {
                return str;
            }
        };*/
        Function<String, String> function = (str) -> { return str; };

        System.out.println(function.apply("abc"));
    }
}
```



**测试断定型(Predicate)接口：**

![image-20221221220107324](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221221220107324.png)

```java
import java.util.function.Predicate;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description 断定型接口: 有一个参数输入，返回值只能是 布尔值！
 */
public class Demo02 {
    public static void main(String[] args) {
        /*// 判断字符串是否为空
        Predicate<String> predicate = new Predicate<String>() {
            @Override
            public boolean test(String str) {
                return str.isEmpty();
            }
        };*/
        Predicate<String> predicate = String::isEmpty;
        System.out.println(predicate.test(""));
    }
}
```



>  Consumer 消费性接口

![image-20221221221434328](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221221221434328.png)

```java
import java.util.function.Consumer;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description Consumer消费型接口: 只有输入， 没有返回值
 */
public class Demo03 {
    public static void main(String[] args) {

        /*Consumer<String> consumer = new Consumer<String>() {
            @Override
            public void accept(String str) {
                System.out.println(str);
            }
        };*/
        Consumer<String> consumer = (str) -> {
            System.out.println(str);
        };
        consumer.accept("asd");
    }
}
```



> Supplier 供给型接口

![image-20221221222011616](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221221222011616.png)

```java
import java.util.function.Supplier;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/21
 * @description Supplier供给型接口, 没有参数，只有返回值
 */
public class Demo04 {
    public static void main(String[] args) {

        /*Supplier<Integer> supplier = new Supplier<Integer>() {
            @Override
            public Integer get() {
                System.out.println("get()");
                return 1024;
            }
        };*/
        Supplier<Integer> supplier = () -> {return 1024;};

        System.out.println(supplier.get());

    }
}
```



## 12、Stream流式计算



```java
import java.util.Arrays;
import java.util.List;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 * <p>
 *     要求：现在有五个用户！筛选
 *     1、ID必须是偶数
 *     2、年龄必须大于23岁
 *     3、用户名转为大写字母
 *     4、用户名字母倒着排序
 *     5、只输出一个用户
 * </p>
 */
public class Test {
    public static void main(String[] args) {
        User user1 = new User(1, "a", 21);
        User user2 = new User(2, "b", 22);
        User user3 = new User(3, "c", 23);
        User user4 = new User(4, "d", 24);
        User user5 = new User(6, "e", 25);
        List<User> list = Arrays.asList(user1, user2, user3, user4, user5);

        // 链式编程
        list.stream()
                .filter(u -> {return u.getId() % 2 == 0;})
                .filter(user -> {return user.getAge() > 23;})
                .map(user -> {return user.getName().toUpperCase();})
                .sorted((u1, u2) -> {return u2.compareTo(u1);})
                .limit(1)
                .forEach(System.out::println);
    }
}
```



## 13、ForkJoin

> 什么是ForkJoin

**ForkJoin：**在并行执行任务！提高效率，大数据量！把大任务拆成小任务

> ForkJoin的特点：工作窃取

**当本线程的任务执行完毕之后，回去找一部分其他线程未执行的任务去执行**

ForkJoin里面维护的都是双端队列

> ForkJoin的操作



```java
import java.util.concurrent.RecursiveTask;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description ForkJoin测试
 * <p>
 *     // 1、forkJoin 通过它来执行
 *     // 2、计算任务 forkJoinPool.execute(ForkJoinTask task)
 *     // ForkJoinTask
 * </p>
 */
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;
    private Long end;

    // 临界值
    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }

    /**
     * The main computation performed by this task.
     *
     * @return the result of the computation
     */
    @Override
    protected Long compute() {
        if ((end - start) > temp) {
            // 分支合并计算
            long sum = 0L;
            for (Long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        } else { // forkJoin
            long mid = (start + end) / 2;
            ForkJoinDemo task1 = new ForkJoinDemo(start, mid);
            // 拆分任务
            task1.fork();
            ForkJoinDemo task2 = new ForkJoinDemo(mid + 1, end);
            task2.fork();

            return task1.join() + task2.join();
        }
    }
}

import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.LongStream;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 */
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // test1();     // 676
        // test2();     // 8909
        test3();        // 289
    }

    //
    public static void test1() {
        long sum = 0L;
        long start = System.currentTimeMillis();
        for (long i = 1L; i <= 10_0000_0000; i++) {
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum = " + sum + "时间：" +(end - start));
    }

    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(1L, 10_0000_0000L);
        // 执行任务
        // forkJoinPool.execute(task);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);
        Long sum = submit.get();
        long end = System.currentTimeMillis();
        System.out.println("sum = " + sum + "时间：" +(end - start));
    }

    public static void test3() {
        long start = System.currentTimeMillis();
        // Stream并行流
        LongStream.rangeClosed(0L, 10_0000_0000L).parallel().reduce(0, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("sum = " + "时间：" +(end - start));
    }
}
```



## 14、异步回调



```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.TimeUnit;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 * <p>
 *     异步调用： CompletableFuture, 异步执行：成功回调、失败回调
 * </p>
 */
public class Demo01 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {

        // 没有返回值的异步回调
//        CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(() -> {
//            try {
//                TimeUnit.SECONDS.sleep(2);
//            } catch (InterruptedException e) {
//                throw new RuntimeException(e);
//            }
//            System.out.println(Thread.currentThread().getName() + "runAsync => Void");
//        });
//        System.out.println("1111");
//        completableFuture.get();

        // 有返回值的  supplyAsync 异步回调
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName() + "supplyAsync => Integer");
            return 1024;
        });

        System.out.println(completableFuture.whenComplete((t, u) -> {
            System.out.println("t = " + t); // 正常的结果返回
            System.out.println("u = " + u); // 错误信息：打印错误信息
        }).exceptionally((e) -> {
            System.out.println(e.getMessage());
            return 233;
        }).get());
    }
}
```



## 15、JMM

> 请你谈谈你对Volatile的理解

**Volatile：**是Java虚拟机提供**轻量级的同步机制**

1、保证可见性

2、不保证原子性

3、禁止指令重排



> 什么是JMM

JMM：Java内存模型，不存在的东西

**关于JMM的一些同步约定：**

1、线程解锁前，必须把共享变量==立刻==刷回主存

2、线程加锁前，必须读取主存中的最新到工作内存中

3、加锁和解锁是同一把锁



![image-20221222205042908](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222205042908.png)



**Java内存模型定义了以下八种操作来完成：**

- **lock（锁定）**：作用于主内存的变量，把一个变量标识为一条线程独占状态。
- **unlock（解锁）**：作用于主内存变量，把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。
- **read（读取）**：作用于主内存变量，把一个变量值从主内存传输到线程的工作内存中，以便随后的load动作使用
- **load（载入）**：作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。
- **use（使用）**：作用于工作内存的变量，把工作内存中的一个变量值传递给执行引擎，每当虚拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。
- **assign（赋值）**：作用于工作内存的变量，它把一个从执行引擎接收到的值赋值给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
- **store（存储）**：作用于工作内存的变量，把工作内存中的一个变量的值传送到主内存中，以便随后的write的操作。
- **write（写入）**：作用于主内存的变量，它把store操作从工作内存中一个变量的值传送到主内存的变量中。

**Java内存模型还规定了在执行上述八种基本操作时，必须满足如下规则：**

- 如果要把一个变量从主内存中复制到工作内存，就需要按顺寻地执行read和load操作， 如果把变量从工作内存中同步回主内存中，就要按顺序地执行store和write操作。但Java内存模型只要求上述操作必须按顺序执行，而没有保证必须是连续执行。
- 不允许read和load、store和write操作之一单独出现
- 不允许一个线程丢弃它的最近assign的操作，即变量在工作内存中改变了之后必须同步到主内存中。
- 不允许一个线程无原因地（没有发生过任何assign操作）把数据从工作内存同步回主内存中。
- 一个新的变量只能在主内存中诞生，不允许在工作内存中直接使用一个未被初始化（load或assign）的变量。即就是对一个变量实施use和store操作之前，必须先执行过了assign和load操作。
- 一个变量在同一时刻只允许一条线程对其进行lock操作，但lock操作可以被同一条线程重复执行多次，多次执行lock后，只有执行相同次数的unlock操作，变量才会被解锁。lock和unlock必须成对出现
- 如果对一个变量执行lock操作，将会清空工作内存中此变量的值，在执行引擎使用这个变量前需要重新执行load或assign操作初始化变量的值
- 如果一个变量事先没有被lock操作锁定，则不允许对它执行unlock操作；也不允许去unlock一个被其他线程锁定的变量。
- 对一个变量执行unlock操作之前，必须先把此变量同步到主内存中（执行store和write操作）。



## 16、Volatile

> 1、保证可见性



```java
import java.util.concurrent.TimeUnit;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 */
public class JMMDemo {
    /**
     * 不加 volatile 程序会进入死循环
     * 家加 volatile 可以保证可见性
     */
    private volatile static int num = 0;
    public static void main(String[] args) {

        new Thread(() -> {
            while (num == 0) {

            }
        }).start();

        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        num = 1;
        System.out.println(num);
    }
}
```



> 2、不保证原子性



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 * <p>
 *     不保证原子性
 * </p>
 */
public class VDemo02 {

    // volatile 不保证原子性
    private volatile static int num = 0;

    public static void add() {
        num++;
    }
    public static void main(String[] args) {

        new Thread(() -> {
            for (int i = 1; i <= 200; i++) {
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }
        }).start();

        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + " " + num);
    }
}
```

**如果不加lock和synchronized，怎么保证原子性？**

使用原子类：解决原子性问题。

```java
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试
 * <p>
 *     不保证原子性
 * </p>
 */
public class VDemo02 {

    // volatile 不保证原子性
    // 原子类的 Integer
    private volatile static AtomicInteger num = new AtomicInteger();

    public static void add() {
//        num++;
        num.getAndIncrement(); // AtomicInteger 加一操作
    }
    public static void main(String[] args) {

        new Thread(() -> {
            for (int i = 1; i <= 200; i++) {
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }
        }).start();

        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + " " + num);
    }
}
```



> 指令重排

什么是指令重排：源代码 ——> 编译器优化重排 ——> 指令并行也可能会重排 ——> 内存系统也会重排 ——> 执行

**处理器在指令重排的时候，考虑：数据之间的依赖性！**

![image-20221222212907700](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222212907700.png)



**volatile可以避免指令重排：**

内存屏障。CPU指令。作用：

1、保证特定的操作执行顺序！

2、可以保证某些变量的内存可见性

![image-20221222213233201](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222213233201.png)

## 17、彻底玩转单例模式

> 饿汉式

```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 饿汉式单例
 * <p>
 *     创建即私有
 * </p>
 */
public class Hungry {

    // 可能会造成空间浪费
    byte[] data1 = new byte[1024 * 1024];
    byte[] data2 = new byte[1024 * 1024];
    byte[] data3 = new byte[1024 * 1024];
    byte[] data4 = new byte[1024 * 1024];

    private Hungry() {

    }

    private final static Hungry HUNGRY = new Hungry();

    public static Hungry getInstance() {
        return HUNGRY;
    }

}
```



> DCL 懒汉式

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 懒汉式单例
 */
public class LazyMan {

    private static boolean chen = false;
    private LazyMan() {
        synchronized (LazyMan.class){
            if (chen == false) {
                chen = true;
            } else {
                throw new RuntimeException("不要试图用反射破坏异常");
            }
            /*if (lazyMan != null) {
                throw new RuntimeException("不要试图用反射破坏异常");
            }*/
        }
        System.out.println(Thread.currentThread().getName() + "ok");
    }

    private volatile static LazyMan lazyMan;

    public static LazyMan getInstance() {
        // 加锁，双重检测锁模式的 懒汉模式 DCL模式
        if (lazyMan == null) {
            synchronized (LazyMan.class) {
                if (lazyMan == null) {
                    // 不是原子性操作
                    lazyMan = new LazyMan();
                }
            }
        }
        return lazyMan;
    }


    // 反射
    public static void main(String[] args) throws Exception {
        // LazyMan instance = LazyMan.getInstance();

        Field chen1 = LazyMan.class.getDeclaredField("chen");
        chen1.setAccessible(true);
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(String。class, int.class);
        declaredConstructor.setAccessible(true);
        LazyMan instance = declaredConstructor.newInstance();

        chen1.set(instance, false);
        LazyMan instance2 = declaredConstructor.newInstance();

        System.out.println(instance);
        System.out.println(instance2);

    }
}

// 1、分配内存空间
// 2、执行构造方法，初始化对象
// 3、把这个对象指向这个空间
```



> 静态内部类



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 静态内部类
 */
public class Holder {
    
    private Holder() {
        
    }
    
    public static Holder getInstance() {
        return InnerClass.HOLDER;
    }
    
    public static class InnerClass{
        private static final Holder HOLDER = new Holder();
    }
}
```



> 单例不安全，反射：使用枚举



```java
/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 枚举单例
 */
public enum EnumSingle {

    INSTANCE;

    public EnumSingle getInstance() {
        return INSTANCE;
    }
}

class Test {
    public static void main(String[] args) {

        EnumSingle instance1 = EnumSingle.INSTANCE;

    }
}
```



## 18、深入理解CAS

> 什么是CAS

```java
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description CAS测试
 */
public class CASDemo {

    // CAS compareAndSet: 比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);

        // public final boolean compareAndSet(int expect, int update) 期望、更新
        // 如果我期望的值拿到了就更新，否则就不更新
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());
    }
}
```



> Unsafe类

![image-20221222222946800](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222222946800.png)

![image-20221222223513050](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222223513050.png)

![image-20221222223554731](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222223554731.png)



CAS： 比较并交换，比较当前工作中的值和主内存中的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环！

**缺点：**

1、循环会耗时

2、一次性只能保证一个共享变量的原子性

3、ABA问题



> CAS：ABA问题（狸猫换太子）



![image-20221222224130455](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222224130455.png)

```java
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description CAS测试
 */
public class CASDemo {

    // CAS compareAndSet: 比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);

        // public final boolean compareAndSet(int expect, int update) 期望、更新
        // 如果我期望的值拿到了就更新，否则就不更新
        // ============= 捣乱的线程 ==============
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        System.out.println(atomicInteger.compareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());

        // ============= 期望的线程 ==============
        System.out.println(atomicInteger.compareAndSet(2020, 6666));
        System.out.println(atomicInteger.get());
    }
}
```



## 19、原子引用

> 解决ABA问题，引入原子引用！对应的思想：乐观锁

带版本号的 原子操作！

```java
import java.util.Date;
import java.util.Objects;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicStampedReference;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description CAS测试
 */
public class CASDemo {

    // CAS compareAndSet: 比较并交换
    public static void main(String[] args) {
//        AtomicInteger atomicInteger = new AtomicInteger(2020);

        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference<>(123,1);


        // public final boolean compareAndSet(int expect, int update) 期望、更新
        // 如果我期望的值拿到了就更新，否则就不更新
        // ============= 捣乱的线程 ==============
        /*System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        System.out.println(atomicInteger.compareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());

        // ============= 期望的线程 ==============
        System.out.println(atomicInteger.compareAndSet(2020, 6666));
        System.out.println(atomicInteger.get());*/

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            System.out.println("a1 => " + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            System.out.println(atomicStampedReference.compareAndSet(123, 124,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));
            System.out.println("a2 => " + atomicStampedReference.getStamp());

            System.out.println(atomicStampedReference.compareAndSet(124, 123,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1));
            System.out.println("a3 => " + atomicStampedReference.getStamp());

        }, "A").start();

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp(); // 获得版本号
            System.out.println("b1 => " + stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            System.out.println(atomicStampedReference.compareAndSet(123, 125,
                    stamp, stamp + 1));

        }, "B").start();

    }
}
```



## 20、各种锁的理解

### 1、可重入锁

> Synchronized

```java
package com.chen.lock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 可重入锁
 * Synchronized
 */
public class Demo1 {
    public static void main(String[] args) {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            phone1.sms();
        }, "A").start();

        new Thread(() -> {
            phone1.sms();
        }, "B").start();

    }
}
class Phone {

    public synchronized void sms() {
        System.out.println(Thread.currentThread().getName() + "sms()");
        // call也有锁
        call();
    }

    public synchronized void call() {
        System.out.println(Thread.currentThread().getName() + "call()");
    }
}
```



> lock

```java
package com.chen.lock;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 可重入锁Lock版
 */
public class Demo2 {
    public static void main(String[] args) {
        Phone1 phone1 = new Phone1();

        new Thread(() -> {
            phone1.sms();
        }, "A").start();

        new Thread(() -> {
            phone1.sms();
        }, "B").start();

    }
}
class Phone1 {

    Lock lock = new ReentrantLock();

    public void sms() {
        // 获得锁
        lock.lock(); // 细节：lock锁必须配对，否则就会产生死锁
        try {
            System.out.println(Thread.currentThread().getName() + "sms()");
            // call也有锁
            call();
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            // 释放锁
            lock.unlock();
        }
    }

    public  void call() {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call()");
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();
        }
    }
}
```

### 2、自旋锁

**spinlock:**

![image-20221222232726646](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222232726646.png)

**自定义自旋锁测试：**

```java
package com.chen.lock;

import java.util.concurrent.atomic.AtomicReference;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 自旋锁
 */
public class SpinLockDemo {

    AtomicReference<Thread> atomicReference = new AtomicReference<>();

    // 加锁
    public void myLock() {
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "===> myLock");

        while (!atomicReference.compareAndSet(null, thread)) {

        }
    }

    // 解锁
    public void myUnLock() {
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName() + "===> myUnLock");

        atomicReference.compareAndSet(thread, null);
    }
}
```

**测试：**

```java
package com.chen.lock;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 测试自旋锁
 */
public class TestSpinLock {
    public static void main(String[] args) {
//        ReentrantLock reentrantLock = new ReentrantLock();
//        reentrantLock.lock();
//        reentrantLock.unlock();
        // 底层使用自旋锁
        SpinLockDemo lock = new SpinLockDemo();

        new Thread(() -> {
            lock.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                throw new RuntimeException(e);
            } finally {
                lock.myUnLock();
            }
        }, "T1").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        new Thread(() -> {
            lock.myLock();
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (Exception e) {
                throw new RuntimeException(e);
            } finally {
                lock.myUnLock();
            }
        }, "T2").start();
    }
}
```



### 3、死锁

> 死锁是什么？

![image-20221222234258178](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222234258178.png)

**死锁测试**

```java
package com.chen.lock;

import java.util.concurrent.TimeUnit;

/**
 * @author java小豪
 * @version 1.0.0
 * @date 2022/12/22
 * @description 死锁测试
 */
public class DeadLockDemo {
    public static void main(String[] args) {
        String lockA = "lockA";
        String lockB = "lockB";
        new Thread(new MyThread(lockA, lockB), "T1").start();
        new Thread(new MyThread(lockB, lockA), "T2").start();
    }
}

class MyThread implements Runnable {

    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA) {
            System.out.println(Thread.currentThread().getName() + "lock:" + lockA + "=> get" + lockB);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            synchronized (lockB) {
                System.out.println(Thread.currentThread().getName() + "lock:" + lockB + "=> get" + lockA);
            }

        }
    }
}
```



> 解决问题

1、使用 `jps -l` 定位进程号 

![image-20221222235536200](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222235536200.png)

2、使用 `jstack 进程号`找到死锁问题

![image-20221222235651187](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221222235536200.png)


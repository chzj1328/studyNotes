# 一、List

**集合主要分为两组（单列集合，双列集合）**

- **单列集合：存放单个元素**

- - **Collection：两个重要的接口** **List Set**

```java
ArrayList<String> arrayList = new ArrayList<>();
arrayList.add("jack");
arrayList.add("tom");
```

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1654695206344-a626dcbf-07cb-4ae5-b2d1-143d6278b422.png)

- **双列集合：键值对的形式存元素K-V**

- - **Map**

```java
HashMap<String, Object> hashMap = new HashMap<>(64);
hashMap.put("NO1", "北京");
hashMap.put("NO2", "上海");
```

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1654695188306-b008c411-ee47-4da5-a025-e8301806be51.png)

## 1.2、Conllection接口和常用方法

### 1.2.1、常用方法

- add:添加单个元素
- remove:删除指定元素
- contains:查找元素是否存在
- size:获取元素的个数
- isEmpty:判断集合是否为空
- clear:清空集合
- addAll:添加多个元素
- containsAll:查找多个元素是否都存在
- removeAll:删除多个元素

```java
/**
 * @author java小豪
 * @date 2022/6/8
 */
public class CollectionMethod {
    public static void main(String[] args) {
        List<Object> list = new ArrayList<>();
        // add:添加单个元素
        list.add("jack");
        // list.add(new Integer(10))
        list.add(10);
        list.add(true);
        System.out.println("List=" + list);

        // remove:删除指定元素
        // 通过下标删除元素
        list.remove(0);
        // 指定删除的元素
        list.remove(true);
        System.out.println("List=" + list);

        // contains:查找元素是否存在
        System.out.println(list.contains("jack"));

        // size:获取元素的个数
        System.out.println(list.size());

        // isEmpty:判断集合是否为空
        System.out.println(list.isEmpty());

        // clear:清空集合
        //list.clear();
        //System.out.println(list);

        // addAll:添加多个元素
        ArrayList<Object> arrayList = new ArrayList<>();
        arrayList.add("红楼梦");
        arrayList.add("三国演义");
        list.addAll(arrayList);
        System.out.println(list);

        // containsAll:查找多个元素是否都存在
        System.out.println(list.containsAll(arrayList));

        // removeAll:删除多个元素
        list.add("聊斋");
        list.removeAll(arrayList);
        System.out.println(list);
        
    }
}
```

### 1.2.2、Conllection接口遍历元素方式

- 使用Iterator(迭代器)

```java
/**
 * @author java小豪
 * @date 2022/6/8
 */
public class CollectionIterator {
    public static void main(String[] args) {
        Collection<Object> collection = new ArrayList<>();
        collection.add(new Book("三国演义", "罗贯中", 50));
        collection.add(new Book("小李飞刀", "古龙", 70.0));
        collection.add(new Book("红楼梦", "曹雪芹", 60.0));
//        System.out.println("collection = " + collection);

        // 得到迭代器
        Iterator<Object> iterator = collection.iterator();
        // 使用while循环遍历集合
        while (iterator.hasNext()) {
            Object obj =  iterator.next();
            System.out.println("obj = " + obj);
        }

        // 重置迭代器
        iterator = collection.iterator();
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj = " + obj);
        }

    }

}

class Book {
    private String name;
    private String author;
    private double price;

    public Book(String name, String author, double price) {
        this.name = name;
        this.author = author;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                '}';
    }
}
```

- 增强for循环：只能用来遍历集合和数组

- - 基本语法：for(元素类型 元素名 : 集合名或数组名) {	访问元素	}

```java
/**
 * @author java小豪
 * @date 2022/6/8
 */
public class CollectionFor {
    public static void main(String[] args) {
        Collection<Object> collection = new ArrayList<>();
        collection.add(new Book("三国演义", "罗贯中", 50));
        collection.add(new Book("小李飞刀", "古龙", 70.0));
        collection.add(new Book("红楼梦", "曹雪芹", 60.0));

        // 增强for
        // 增强for底层仍然是迭代器
        for (Object book : collection) {
            System.out.println("book = " + book);
        }

        // 增强for用在数组上
        int[] nums = {1, 6, 7, 9, 10};
        for (int i : nums) {
            System.out.println("i = " + i);
        }
    }
}
```

## 1.3、List接口和常用方法

### 1.3.1、介绍

1) List集合类中元素有序、且可重复

2) List集合中的每个元素有其对应的顺序索引，即支持索引。

3) List容器中的元素对应一个整数的序号记载在容器中的位置，可以根据序号取出容器中的元素.

```java
/**
 * @author java小豪
 * @date 2022/6/9
 */
public class List_ {
    public static void main(String[] args) {

        // List集合类中元素有序、且可重复
        List<Object> list = new ArrayList<>();
        list.add("jack");
        list.add("mary");
        list.add("tom");
        list.add("chen");
        System.out.println("list = " + list);

        // List集合中的每个元素有其对应的顺序索引，即支持索引。
        System.out.println(list.get(2));
    }
}
```

### 1.3.2、常用方法

- void add(int index, Object ele):在index位置插入ele元素
- boolean addAll(int index, Collection eles):从index位置开始将eles中的元素插入
- Object get(int index):获取指定index位置的元素
- int indexOf(Object obj):返回obj在当前集合中首次出现的位置
- int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
- Object remove(int index):移除指定index位置的元素，并返回此元素
- Object set(int index, Object ele):设置指定index位置元素为ele
- *List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合*

```java
/**
 * @author java小豪
 * @date 2022/6/9
 */
public class ListMethod {
    public static void main(String[] args) {
        List<Object> list = new ArrayList<>();
        //void add(int index, Object ele):在index位置插入ele元素
        list.add("张三丰");
        list.add("李小龙");
        // 在index = 1 的位置插入一个对象
        list.add(1, "宋江");
        System.out.println("list = " + list);
        //boolean addAll(int index, Collection eles):从index位置开始将eles中的元素插入
        List<Object> list2 = new ArrayList<>();
        list2.add("jack");
        list2.add("tom");
        list.addAll(1, list2);
        System.out.println("list = " + list);
        //Object get(int index):获取指定index位置的元素
        System.out.println(list.get(3));
        //int indexOf(Object obj):返回obj在当前集合中首次出现的位置
        System.out.println(list.indexOf("tom"));
        //int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
        list.add("tom");
        System.out.println("list = " + list);
        System.out.println(list.lastIndexOf("tom"));
        //Object remove(int index):移除指定index位置的元素，并返回此元素
        list.remove(0);
        System.out.println("List = " + list);
        //Object set(int index, Object ele):设置指定index位置元素为ele,相当于是替换
        list.set(1, "玛丽");
        System.out.println("List = " + list);
        // List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
        // 返回子集合范围[fromIndex, toIndex)
        List returnList = list.subList(0, 2);
        System.out.println("returnList = " + returnList);

    }

}
```

### 1.3.3、练习

- *添加十个以上的 String "hello"*
- *在2号位插入一个元素*
- *获取第6个元素*
- *删除第7个元素*
- *修改第8个元素*
- *使用迭代器遍历集合*

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class ListExercise {
    public static void main(String[] args) {
        List<Object> list = new ArrayList<>();
        // 添加十个以上的 String "hello"
        for (int i = 0; i < 12; i++) {
            list.add("hello" + i);
        }
        System.out.println("list = " + list);

        // 在2号位插入一个元素"chen"
        list.add(1, "chen");
        System.out.println("list = " + list);

        // 获取第6个元素
        System.out.println("第6个元素 = " + list.get(5));

        // 删除第7个元素
        list.remove(6);
        System.out.println("list = " + list);

        // 修改第8个元素
        list.set(7, "三国演义");
        System.out.println("list = " + list);

        // 使用迭代器遍历集合
        Iterator<Object> iterator = list.iterator();
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj = " + obj);
        }
    }
}
```

### 1.3.4、List集合的三种遍历方式

- 迭代器Iterator
- 增强for
- 普通for

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class ListFor {
    public static void main(String[] args) {

        // List 接口的实现子类 Vector LinkedList
        // List<Object> list = new ArrayList<>();
         List<Object> list = new LinkedList<>();
        // List<Object> list = new Vector<>();
        list.add("jack");
        list.add("tom");
        list.add("天龙八部");
        list.add("北京烤鸭");

        // 遍历
        // 1.迭代器
        Iterator<Object> iterator = list.iterator();
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj = " + obj);
        }
        System.out.println("=================");
        //2.增强for循环
        for (Object x : list) {
            System.out.println(x);
        }

        System.out.println("================");
        //3.普通for循环
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }

    }
}
```

### 1.3.5、List排序练习

**Book类**

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class Book {
    private String name;
    private String author;
    private int price;

    public Book(String name, String author, int price) {
        this.name = name;
        this.author = author;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "名称："+ name +"\t\t价格：" + price+ "\t\t作者：" + author;
    }
}
```

**main:**

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class ListExercise2 {
    public static void main(String[] args) {

        // List<Object> list = new Vector<>();
        List<Object> list = new ArrayList<>();
        // List<Object> list = new LinkedList<>();
        list.add(new Book("红楼梦", "曹雪芹", 70));
        list.add(new Book("西游记", "吴承恩", 10));
        list.add(new Book("水浒传", "施耐庵", 90));
        list.add(new Book("三国志", "罗贯中", 80));
        // 遍历
        for (Object o : list) {
            System.out.println(o);
        }

        // 冒泡排序
        sort(list);
        System.out.println("=========");

        // 排序后遍历
        for (Object o : list) {
            System.out.println(o);
        }

    }
    public static void sort(List<Object> list) {
        for (int i = 0; i < list.size() - 1; i++) {
            for (int j = 0; j < list.size() - 1 - i; j++) {
                // 取出对象Book
                Book book1 = (Book) list.get(j);
                Book book2 = (Book) list.get(j + 1);
                if (book1.getPrice() > book2.getPrice()) {
                    list.set(j, book2);
                    list.set(j + 1, book1);
                }
            }
        }
    }
}
```

## 1.4、ArrayList底层结构和源码分析

### 1.4.1、注意事项

1. ArrayList可以存放空值null
2. ArrayList是由数组来实现数据存储的
3. ArrayList是线程不安全的(执行效率高),多线程情况下，不建议使用ArrayList

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class ArrayListDetail {
    public static void main(String[] args) {
        ArrayList<String> arrayList = new ArrayList<>();
        arrayList.add(null);
        arrayList.add("jack");
        arrayList.add(null);

        System.out.println("arrayList = " + arrayList);
    }
}
```

### 1.4.2、ArrayList扩容机制

1. ArrayList中维护一个Object类型的数组elementData  *transient* Object[] elementData;
2. *transient ：表示瞬间，短暂的*

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1654869427162-be0031b5-bde7-4985-a22b-a86b84c7904f.png)

1. 当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第一次添加，则扩容elementData为10，如需再次扩容，则扩容elementData为1.5倍。

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291004488.png)

1. 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如需扩容，则直接扩容elementData为1.5倍。

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291004119.png)

### 1.4.3、ArrayList源码

#### 一、当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第一次添加，则扩容elementData为10，如需再次扩容，则扩容elementData为1.5倍。

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1654874691591-eb2b8357-278e-4584-a283-1f22d609e485.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005049.png)

**演示代码**

```java
/**
 * @author java小豪
 * @date 2022/6/10
 */
public class ArrayListSource {
    public static void main(String[] args) {
        // 使用午餐构造器创建ArrayList对象
        ArrayList<Integer> list = new ArrayList<>();
        // ArrayList<Integer> list = new ArrayList<>(8);
        // 使用for给list集合添加 1-10数据
        for (int i = 1; i <= 10; i++) {
            list.add(i);
        }
        // 使用for给list集合添加11-15数据
        for (int i = 11; i <= 15; i++) {
            list.add(i);
        }

        list.add(200);
        list.add(100);
    }

}
```

## 1.5、Vector底层结构和源码分析

### 1.5.1、基本介绍

1. Vector类的定义说明

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005296.png)

1. Vector底层也是一个对象数组，*protected* Object[] elementData;
2. Vector是线程同步的，即线程安全，Vector类的操作方法带有synchronized

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005835.png)

### 1.5.2、Vector底层和ArrayList的比较

|           | 底层结构 | 版本   | 线程安全（同步）效率 | 扩容倍数                                                     |
| --------- | -------- | ------ | -------------------- | ------------------------------------------------------------ |
| ArrayList | 可变数组 | JDK1.2 | 不安全，效率高       | 如果有参构造1.5倍如果是无参1.第一次102.从第二次开始按照1.5倍 |
| Vector    | 可变数组 | JDK1.0 | 安全效率高           | 如果是无参，默认10，满后直接按2倍扩容如果指定大小，则每次直接按2倍扩容 |

### 1.5.3、源码解读

#### 一、创建Vector对象时使用无参构造器源码

```java
/**
 * @author java小豪
 * @date 2022/6/11
 */
public class Vector_ {
    public static void main(String[] args) {
        // 无参构造器
        Vector<Integer> vector = new Vector<>();
        for (int i = 0; i < 10; i++) {
            vector.add(i);
        }
        vector.add(100);
        System.out.println("vector = " + vector);
        // 源码解读
        // 1. new Vector() 底层
        /*
            public Vector() {
                this(10);
            }
            2. vector.add(i)
            2.1 // 添加数据到Vector集合
                public synchronized boolean add(E e) {
                    modCount++;
                    ensureCapacityHelper(elementCount + 1);
                    elementData[elementCount++] = e;
                    return true;
                }
            2.2  // 确定是否需要扩容 条件： minCapacity - elementData.length > 0
                private void ensureCapacityHelper(int minCapacity) {
                    // overflow-conscious code
                    if (minCapacity - elementData.length > 0)
                    grow(minCapacity);
                }
            2.3 // 如果需要的数组大小 不够用， 就扩容 扩容算法
                // int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                //                                   capacityIncrement : oldCapacity);
            private void grow(int minCapacity) {
                // overflow-conscious code
                int oldCapacity = elementData.length;
                int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                                 capacityIncrement : oldCapacity);
                if (newCapacity - minCapacity < 0)
                    newCapacity = minCapacity;
                if (newCapacity - MAX_ARRAY_SIZE > 0)
                    newCapacity = hugeCapacity(minCapacity);
                elementData = Arrays.copyOf(elementData, newCapacity);
            }
         */
    }
}
```

#### 二、分析图

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005405.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005961.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005067.png)



## 1.6、LinkedList底层结构

### 1.6.1、说明

1. LinkedList实现了双向链表和双端队列特点
2. 可以添加任意元素(元素可以重复),包括null
3. 线程不安全，没有实现同步

### 1.6.2、LinkedList的底层操作机制

1. LinkedList底层维护了一个双向链表
2. LinkedList中维护了两个属性first和last分别指向首节点和尾节点
3. 每个节点（Node对象），里面有维护了prev、next、item三个属性，其中通过prev指向前一个，通过next指向后一个节点。最终实现双向链表
4. LinkedList的元素添加和删除，不是通过数组完成的，相对来说效率较高
5. 模拟简单的双向链表

```java
/**
 * @author java小豪
 * @date 2022/6/11
 */
public class LinkedList01 {
    public static void main(String[] args) {
        // 模拟简单的双向链表

        Node jack = new Node("jack");
        Node tom = new Node("tom");
        Node chen = new Node("chen");

        // 连接三个节点，形成双向链表
        // jack -> tom -> chen
        jack.next = tom;
        tom.next = chen;
        // chen -> tom -> jack
        chen.prev = tom;
        tom.prev = jack;

        // 让first引用指向jack，就是双向链表的头节点
        Node first = jack;
        // 让last引用指向chen，就是双向链表的尾节点
        Node last = chen;

        // 遍历 从头到尾遍历
        while (true) {
            if (first == null) {
                break;
            }
            // 输出信息
            System.out.println(first);
            first = first.next;
        }

        // 遍历 从尾到头遍历
        System.out.println("===从尾到头遍历===");
        while (true) {
            if (last == null) {
                break;
            }
            // 输出信息
            System.out.println(last);
            last = last.prev;
        }

        // 添加元素

        // 1.创建一个Node结点， name 是张飞
        Node zf = new Node("张飞");
        zf.next = chen;
        zf.prev = tom;
        chen.prev = zf;
        tom.next = zf;

        // 让first 再次指向jack
        first = jack;
        System.out.println("===从头到尾遍历===");
        while (true) {
            if (first == null) {
                break;
            }
            // 输出信息
            System.out.println(first);
            first = first.next;
        }

    }
}

/**
 * 定义一个Node类
 */
class Node {
    /**存放数据*/
    public Object item;
    /**指向下一个节点*/
    public Node next;
    /**指向前一个节点*/
    public Node prev;

    public Node(Object name) {
        this.item = name;
    }
    public String toString() {
        return "Node name = " + item;
    }
}
```

### 1.6.3、LinkedList底层源码

#### 1.6.3.1、添加元素

- linkedList.add(1);

```java
// 1、LinkedList<Integer> linkedList = new LinkedList<>();
   public LinkedList() {}
// 2、这是LinkedList 的属性first = null last = null
// 3、执行add方法
   public boolean add(E e) {
       linkLast(e);
       return true;
   }
// 4、将新的结点，加入到双向链表的最后
   void linkLast(E e) {
       final Node<E> l = last;
       final Node<E> newNode = new Node<>(l, e, null);
       last = newNode;
       if (l == null)
           first = newNode;
       else
           l.next = newNode;
       size++;
       modCount++;
    }
```

- **流程图**

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005118.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005049.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005657.png)

- **执行 add 方法**

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005246.png)

- **执行 linkList**

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656463990987-c707c8b0-8685-4906-8cab-8514ef3a66eb.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005078.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005636.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206291005017.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656464185047-3e0030a4-f6ed-42db-ac5f-d481fd831d8a.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656464198485-5c268c8c-8d91-4692-bbc7-7fc5e41dd909.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656464211712-95726909-b5c2-4ed9-a187-846f207a8c76.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656464226122-018f1484-3fa7-4893-8808-f9296f2d616d.png)

#### 1.6.3.2、删除元素

- linkedList.remove();

```java
// 1、linkedList.remove();// 默认删除第一个元素
// 2、执行 removeFirst() 方法
    public E remove() {
        return removeFirst();
    }
// 3、执行
    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }
// 4、执行  unlinkFirst, 将 f 指向的双向链表的第一个结点拿掉
    private E unlinkFirst(Node<E> f) {
        // assert f == first && f != null;
        final E element = f.item;
        final Node<E> next = f.next;
        f.item = null;
        f.next = null; // help GC
        first = next;
        if (next == null)
            last = null;
        else
            next.prev = null;
        size--;
        modCount++;
        return element;
    }
```

- **流程图**

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467078912-63bed6ed-b0bb-4678-a14b-97f8bc782b23.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467104207-889c5366-284a-4601-857e-c921658d0e3d.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467156043-45a22a97-5d29-41d9-98af-3f96cb3a269a.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467183996-c924ff30-e085-4484-8313-13c77fa29605.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467200353-ef94d9e5-be7c-4791-89b6-60225d32fb2d.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467212112-c73280e7-669f-4201-b607-867af8db7ae8.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467222932-d03ddc00-8750-4457-88a2-05c81070a4a7.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467239624-1fc5af26-ba02-4d41-8666-4306dcd861df.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467252510-047fb1a3-dd4d-4009-b8d7-c4e129b98eef.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467265531-747c268a-6fec-4b68-b6f9-6097811aeea4.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467282227-e1d48922-3cd0-483d-ae4c-dede22547137.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467295565-8fb7ccb8-a348-418a-b970-16f4d12af9cf.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467321485-b5d35ec6-7293-4cd0-a525-35e100356d25.png)

![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/1656467336076-8a7a0857-5c14-462f-9815-e036c56f5a52.png)

#### 1.6.3.3、LinkedList的其他方法

- **get()**
- **remove(int index)**
- **set(int index)**

**等等的源码都可以依照以上方法进行追溯。**

### **1.6.4、ArrayList和LinkedList比较**

- ArrayList和LinkedList比较

|            | 底层结构 |      增删效率      | 改查效率 |
| :--------: | :------: | :----------------: | :------: |
| ArrayList  | 可变数组 | 较低<br />数组扩容 |   较高   |
| LinkedLIst | 双向链表 | 较高，通过链表追加 |   较低   |

- 如何选择ArrayList和LinkedList：
  - 如果改查比较多，选择ArrayList
  - 如果增删比较多，选择LinkedList
  - 一般来说，80%-90%都是查询，因此大部分情况下会选择ArrayList

## 1.7、Set

### 1.7.1、Set接口基本介绍

- 无序（添加和取出的顺序不一致），没有索引
- 不允许重复元素，最多包含一个null
- Set接口的常用实现类有：HashSet、TreeSet

### 1.7.2、Set接口的常用方法

- **和List接口一样，Set接口也是Collection的子接口，所以常用方法和Collection接口一样**
  - add:添加单个元素
  - remove:删除指定元素
  - contains:查找元素是否存在
  - size:获取元素的个数
  - isEmpty:判断集合是否为空
  - clear:清空集合
  - addAll:添加多个元素
  - containsAll:查找多个元素是否都存在
  - removeAll:删除多个元素

- **遍历方式：**
  - 使用迭代器
  - 增强for

### 1.7.3、Set接口实现类—HashSet

**1、HashSet的全面说明**

- HashSet实现了Set接口
- HashSet实际上是HashMap

```java
public HashSet() {
		map = new HashMap<>();
}
```

- 可以存放null值，但是只能有一个null
- HashSet不保证元素是有序的，取决于hash后，在确定索引结果
- 不能有重复的元素/对象

**2、HashSet案例说明**

```java
/**
 * @author java小豪
 * @date 2022/6/29
 */
@SuppressWarnings({"all"})
public class HashSet01 {
    public static void main(String[] args) {

        // 说明
        // 1.在执行add方法后，会返回一个boolean值
        // 2.如果添加成功，会返回 true, 否则返回false
        // 3.通过 remove 指定删除那哪个对象
        HashSet hashSet = new HashSet<>();
        System.out.println(hashSet.add("john"));// T
        System.out.println(hashSet.add("lucy"));// T
        System.out.println(hashSet.add("john"));// F
        System.out.println(hashSet.add("jack"));// T
        System.out.println(hashSet.add("Rose"));// T
        hashSet.remove("john");
        System.out.println("set = " + hashSet);


        hashSet = new HashSet<>();
        // 4 HashSet 不能添加相同的元素/数据？
        hashSet.add("lucy");// 添加成功
        hashSet.add("lucy");// 不能添加
        hashSet.add(new Dog("tom")); // OK
        hashSet.add(new Dog("tom")); // OK
        System.out.println("hashSet = " + hashSet);

        // 再加深，非常经典的面试题
        // 看源码再分析
        hashSet.add(new String("chen")); //OK
        hashSet.add(new String("chen")); //False
        System.out.println("hashSet = " + hashSet);
    }
}
class Dog {
    private String name;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

### 1.7.4、HashSet底层机制说明

**一、HashSet底层是HashMap,HashMap底层是(数组+链表+红黑树)**

**简单的数组+链表的模拟**

```java
/**
 * @author java小豪
 * @date 2022/6/29
 */
public class HashSetStructure {
    public static void main(String[] args) {
        // 模拟一个HashSet的底层 (HashMap 的底层结构)

        // 1.创建一个数组， 数组的类型是Node[]
        // 2.有些人，直接把 Node[] 数组称为 表
        Node[] table = new Node[16];
        System.out.println("table = " + table);
        // 3.创建结点
        Node john = new Node("john", null);

        table[2] = john;
        Node jack  = new Node("jack", null);
        // 将Jack结点挂在到john后面
        john.next = jack;
        Node rose = new Node("Rose", null);
        // 将 rose结点
        jack.next = rose;
        System.out.println("table = " + table);

        Node lucy = new Node("lucy", null);
        table[3] = lucy;
        System.out.println("table = " + table);


    }
}

@SuppressWarnings({"all"})
class Node{ // 结点， 存储数据， 可以指向下一个结点， 从而形成链表
    // 存放数据
    Object item;
    // 指向下一个结点
    Node next;

    public Node(Object item, Node next) {
        this.item = item;
        this.next = next;
    }
}
```

**图解**

![image-20220629205819435](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/image-20220629205819435.png)

![image-20220629205913111](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/image-20220629205913111.png)

**二、HashSet是添加元素是如何实现的(hash()+equals())**

1. HashSet底层是HashMap
2. 添加一个元素时，先得到hash值-会转成—>索引值
3. 找到存储数据表table，看这个索引位置是否已经存放的有元素
4. 如果没有，直接加入
5. 如果有，调用equals比较，如果相同，就放弃添加，如果不相同，则添加到最后
6. 在Java8中，如果一条链表的元素个数到达TREEFIY_THRESHOLD(默认是 8 )，并且table的大小 >= MIN_TREEFIY_CAPACITY(默认64)，就会进行树化(红黑树)


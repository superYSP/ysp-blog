## JavaSE_第11章 集合与迭代器

### 1 集合的概念

集合是java中提供的一种容器，可以用来存储多个数据。

集合和数组既然都是容器，它们有啥区别呢？

- 数组的长度是固定的。集合的长度是可变的。
- 数组中可以存储基本数据类型值，也可以存储对象，而集合中只能存储对象

集合主要分为两大系列：Collection和Map，Collection 表示一组对象，Map表示一组映射关系或键值对。

### 2 Collection接口

Collection 层次结构中的根接口。Collection 表示一组对象，这些对象也称为 collection 的元素。一些 collection 允许有重复的元素，而另一些则不允许。一些 collection 是有序的，而另一些则是无序的。JDK 不提供此接口的任何直接实现：它提供更具体的子接口（如 Set 和 List、Queue）实现。此接口通常用来传递 collection，并在需要最大普遍性的地方操作这些 collection。

Collection是所有单列集合的父接口，因此在Collection中定义了单列集合(List和Set)通用的一些方法，这些方法可用于操作所有的单列集合。方法如下：

### **1、添加元素**

（1）add(E obj)：添加元素对象到当前集合中

（2）addAll(Collection<? extends E> other)：添加other集合中的所有元素对象到当前集合中，即this = this ∪ other

### **2、删除元素**

（1） boolean remove(Object obj) ：从当前集合中删除第一个找到的与obj对象equals返回true的元素。

（2）boolean removeAll(Collection<?> coll)：从当前集合中删除所有与coll集合中相同的元素。即this = this - this ∩ coll

（3）boolean removeIf(Predicate<? super E> filter) ：删除满足给定条件的此集合的所有元素。

（4）boolean retainAll(Collection<?> coll)：从当前集合中删除两个集合中不同的元素，使得当前集合仅保留与c集合中的元素相同的元素，即当前集合中仅保留两个集合的交集，即this = this ∩ coll；

### **3 查询与获取元素**

（1）boolean isEmpty()：判断当前集合是否为空集合。

（2）boolean contains(Object obj)：判断当前集合中是否存在一个与obj对象equals返回true的元素。

（3）boolean containsAll(Collection<?> c)：判断c集合中的元素是否在当前集合中都存在。即c集合是否是当前集合的“子集”。

（4）int size()：获取当前集合中实际存储的元素个数

（5）Object[] toArray()：返回包含当前集合中所有元素的数组

### 3 Iterator接口

在程序开发中，经常需要遍历集合中的所有元素。针对这种需求，JDK专门提供了一个接口`java.util.Iterator`。`Iterator`接口也是Java集合中的一员，但它与`Collection`、`Map`接口有所不同，`Collection`接口与`Map`接口主要用于存储元素，而`Iterator`主要用于迭代访问（即遍历）`Collection`中的元素，因此`Iterator`对象也被称为迭代器。

想要遍历Collection集合，那么就要获取该集合迭代器完成迭代操作，下面介绍一下获取迭代器的方法：

- `public Iterator iterator()`: 获取集合对应的迭代器，用来遍历集合中的元素的。

下面介绍一下迭代的概念：

- **迭代**：即Collection集合元素的通用获取方式。在取元素之前先要判断集合中有没有元素，如果有，就把这个元素取出来，继续在判断，如果还有就再取出出来。一直把集合中的所有元素全部取出。这种取出方式专业术语称为迭代。

Iterator接口的常用方法如下：

- `public E next()`:返回迭代的下一个元素。
- `public boolean hasNext()`:如果仍有元素可以迭代，则返回 true。

## JavaSE_第12章 集合的重要接口

### 1 List接口介绍

`java.util.List`接口继承自`Collection`接口，是单列集合的一个重要分支，习惯性地会将实现了`List`接口的对象称为List集合。

List接口特点：

- List集合所有的元素是以一种线性方式进行存储的，例如，存元素的顺序是11、22、33。那么集合中，元素的存储就是按照11、22、33的顺序完成的）
- 它是一个元素存取有序的集合。即元素的存入顺序和取出顺序有保证。
- 它是一个带有索引的集合，通过索引就可以精确的操作集合中的元素（与数组的索引是一个道理）。
- 集合中可以有重复的元素，通过元素的equals方法，来比较是否为重复的元素。

### 2 List接口中常用方法

List作为Collection集合的子接口，不但继承了Collection接口中的全部方法，而且还增加了一些根据元素索引来操作集合的特有方法，如下：

List除了从Collection集合继承的方法外，List 集合里添加了一些根据索引来操作集合元素的方法。

1、添加元素

- void add(int index, E ele)
- boolean addAll(int index, Collection<? extends E> eles)

2、获取元素

- E get(int index)
- List subList(int fromIndex, int toIndex)

3、获取元素索引

- int indexOf(Object obj)
- int lastIndexOf(Object obj)

4、删除和替换元素

- E remove(int index)
- E set(int index, E ele)

### 3 List接口的实现类们

List接口的实现类有很多，常见的有：

ArrayList：动态数组

Vector：动态数组

LinkedList：双向链表

当然，还有很多List接口的实现类这里没有列出来，基础阶段先了解这几个。

### (1)ArrayList与Vector的区别？

它们的底层物理结构都是数组，我们称为动态数组。

- ArrayList是新版的动态数组，线程不安全，效率高，Vector是旧版的动态数组，线程安全，效率低。
- 动态数组的扩容机制不同，ArrayList扩容为原来的1.5倍，Vector扩容增加为原来的2倍。
- 数组的初始化容量，如果在构建ArrayList与Vector的集合对象时，没有显式指定初始化容量，那么Vector的内部数组的初始容量默认为10，而ArrayList在JDK1.6及之前的版本也是10，JDK1.7之后的版本ArrayList初始化为长度为0的空数组，之后在添加第一个元素时，再创建长度为10的数组。
- Vector因为版本古老，支持Enumeration 迭代器。但是该迭代器不支持快速失败。而Iterator和ListIterator迭代器支持快速失败。如果在迭代器创建后的任意时间从结构上修改了向量（通过迭代器自身的 remove 或 add 方法之外的任何其他方式），则迭代器将抛出 ConcurrentModificationException。因此，面对并发的修改，迭代器很快就完全失败，而不是冒着在将来不确定的时间任意发生不确定行为的风险。

### (2)链表的特点

逻辑结构：线性结构

物理结构：不要求连续的存储空间

存储特点：数据必须封装到“结点”中，结点包含多个数据项，数据值只是其中的一个数据项，其他的数据项用来记录与之有关的结点的地址。

例如：以下列出几种常见的链式存储结构（当然远不止这些）

### (3)链表与动态数组的区别

动态数组底层的物理结构是数组，因此根据索引访问的效率非常高。但是非末尾位置的插入和删除效率不高，因为涉及到移动元素。另外添加操作时涉及到扩容问题，就会增加时空消耗。

链表底层的物理结构是链表，因此根据索引访问的效率不高，但是插入和删除不需要移动元素，只需要修改前后元素的指向关系即可，而且链表的添加不会涉及到扩容问题。

### 4 Set集合

Set接口是Collection的子接口，set接口没有提供额外的方法。但是比`Collection`接口更加严格了。

Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个 Set 集合中，则添加操作失败。

Set集合支持的遍历方式和Collection集合一样：foreach和Iterator。

Set的常用实现类有：HashSet、TreeSet、LinkedHashSet。

### 5 HashSet

HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。

`java.util.HashSet`底层的实现其实是一个`java.util.HashMap`支持，然后HashMap的底层物理实现是一个Hash表。（什么是哈希表，下一节在HashMap小节在细讲，这里先不展开）

HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取和查找性能。HashSet 集合判断两个元素相等的标准：两个对象通过 hashCode() 方法比较相等，并且两个对象的 equals() 方法返回值也相等。因此，存储到HashSet的元素要重写hashCode和equals方法。

### 6 LinkedHashSet

LinkedHashSet是HashSet的子类，它在HashSet的基础上，在结点中增加两个属性before和after维护了结点的前后添加顺序。`java.util.LinkedHashSet`，它是链表和哈希表组合的一个数据存储结构。LinkedHashSet插入性能略低于 HashSet，但在迭代访问 Set 里的全部元素时有很好的性能。

### 7 TreeSet

TreeSet里面维护了一个TreeMap，底层是基于红黑树实现的！

TreeSet特点：

- 不允许重复
- 实现排序：自然排序或定制排序

如何实现去重的？

```text
如果使用的是自然排序，则通过调用实现的compareTo方法
如果使用的是定制排序，则通过调用比较器的compare方法
```

如何排序？

```text
方式一：自然排序
让待添加的元素类型实现Comparable接口，并重写compareTo方法

方式二：定制排序
创建Set对象时，指定Comparator比较器接口，并实现compare方法
```

### 8 Collections工具类

参考操作数组的工具类：Arrays。

Collections 是一个操作 Set、List 和 Map 等集合的工具类。Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法：

- public static boolean addAll(Collection<? super T> c,T... elements)将所有指定元素添加到指定 collection 中。
- public static int binarySearch(List<? extends Comparable<? super T>> list,T key)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且必须是可比较大小的，即支持自然排序的。而且集合也事先必须是有序的，否则结果不确定。
- public static int binarySearch(List<? extends T> list,T key,Comparator<? super T> c)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且集合也事先必须是按照c比较器规则进行排序过的，否则结果不确定。
- public static > T max(Collection<? extends T> coll)在coll集合中找出最大的元素，集合中的对象必须是T或T的子类对象，而且支持自然排序
- public static T max(Collection<? extends T> coll,Comparator<? super T> comp)在coll集合中找出最大的元素，集合中的对象必须是T或T的子类对象，按照比较器comp找出最大者
- public static void reverse(List<?> list)反转指定列表List中元素的顺序。
- public static void shuffle(List<?> list) List 集合元素进行随机排序，类似洗牌
- public static > void sort(List list)根据元素的自然顺序对指定 List 集合元素按升序排序
- public static void sort(List list,Comparator<? super T> c)根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
- public static void swap(List<?> list,int i,int j)将指定 list 集合中的 i 处元素和 j 处元素进行交换
- public static int frequency(Collection<?> c,Object o)返回指定集合中指定元素的出现次数
- public static void copy(List<? super T> dest,List<? extends T> src)将src中的内容复制到dest中
- public static boolean replaceAll(List list，T oldVal，T newVal)：使用新值替换 List 对象的所有旧值
- Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题
- Collections类中提供了多个unmodifiableXxx()方法，该方法返回指定 Xxx的不可修改的视图。

### 9 Map集合

现实生活中，我们常会看到这样的一种集合：IP地址与主机名，身份证号与个人，系统用户名与系统用户对象等，这种一一对应的关系，就叫做映射。Java提供了专门的集合类用来存放这种对象关系的对象，即`java.util.Map<K,V>`接口。

我们通过查看`Map`接口描述，发现`Map<K,V>`接口下的集合与`Collection<E>`接口下的集合，它们存储数据的形式不同。

- `Collection`中的集合，元素是孤立存在的（理解为单身），向集合中存储元素采用一个个元素的方式存储。
- `Map`中的集合，元素是成对存在的(理解为夫妻)。每个元素由键与值两部分组成，通过键可以找对所对应的值。
- `Collection`中的集合称为单列集合，`Map`中的集合称为双列集合。
- 需要注意的是，`Map`中的集合不能包含重复的键，值可以重复；每个键只能对应一个值（这个值可以是单个值，也可以是个数组或集合值）。

### 10 Map常用方法

1、添加操作

- V put(K key,V value)
- void putAll(Map<? extends K,? extends V> m)

2、删除

- void clear()
- V remove(Object key)

3、元素查询的操作

- V get(Object key)
- boolean containsKey(Object key)
- boolean containsValue(Object value)
- boolean isEmpty()

4、元视图操作的方法：

- Set keySet()
- Collection values()
- Set> entrySet()

5、其他方法

- int size()

### 11 Map集合的遍历

Collection集合的遍历：（1）foreach（2）通过Iterator对象遍历

Map的遍历，不能支持foreach，因为Map接口没有继承java.lang.Iterable接口，也没有实现Iterator iterator()方法。只能用如下方式遍历：

（1）分开遍历：

- 单独遍历所有key
- 单独遍历所有value

（2）成对遍历：

- 遍历的是映射关系Map.Entry类型的对象，Map.Entry是Map接口的内部接口。每一种Map内部有自己的Map.Entry的实现类。在Map中存储数据，实际上是将Key---->value的数据存储在Map.Entry接口的实例中，再在Map集合中插入Map.Entry的实例化对象，如图示：

```java
package com.atguigu.map;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class TestMapIterate {
    public static void main(String[] args) {
        HashMap<String,String> map = new HashMap<>();
        map.put("许仙", "白娘子");
        map.put("董永", "七仙女");
        map.put("牛郎", "织女");
        map.put("许仙", "小青");

        System.out.println("所有的key:");
        Set<String> keySet = map.keySet();
        for (String key : keySet) {
            System.out.println(key);
        }

        System.out.println("所有的value：");
        Collection<String> values = map.values();
        for (String value : values) {
            System.out.println(value);
        }

        System.out.println("所有的映射关系");
        Set<Map.Entry<String,String>> entrySet = map.entrySet();
        for (Map.Entry<String,String> entry : entrySet) {
//          System.out.println(entry);
            System.out.println(entry.getKey()+"->"+entry.getValue());
        }
    }
}
```

### 12 Map的实现类们

Map接口的常用实现类：HashMap、TreeMap、LinkedHashMap和Properties。其中HashMap是 Map 接口使用频率最高的实现类。

### **1、HashMap和Hashtable**

HashMap和Hashtable都是哈希表。HashMap和Hashtable判断两个 key 相等的标准是：两个 key 的hashCode 值相等，并且 equals() 方法也返回 true。因此，为了成功地在哈希表中存储和获取对象，用作键的对象必须实现 hashCode 方法和 equals 方法。

- Hashtable是线程安全的，任何非 null 对象都可以用作键或值。
- HashMap是线程不安全的，并允许使用 null 值和 null 键。

### **2、LinkedHashMap**

LinkedHashMap 是 HashMap 的子类。此实现与 HashMap 的不同之处在于，后者维护着一个运行于所有条目的双重链接列表。此链接列表定义了迭代顺序，该迭代顺序通常就是将键插入到映射中的顺序（插入顺序）。

### **3、TreeMap**

基于红黑树（Red-Black tree）的 NavigableMap 实现。该映射根据其键的自然顺序进行排序，或者根据创建映射时提供的 Comparator 进行排序，具体取决于使用的构造方法。

## JavaSE_第13章 泛型

### 1 使用核心类库中的泛型类/接口

自从JDK1.5引入泛型的概念之后，对之前核心类库中的API做了很大的修改，例如：集合框架集中的相关接口和类、java.lang.Comparable接口、java.util.Comparator接口、Class类等等。

下面以Collection、ArrayList集合以及Iterator迭代器为例演示，泛型类与泛型接口的使用。

### 案例一：Collection集合相关类型

（1）创建一个Collection集合（暂时创建ArrayList集合对象），并指定泛型为

（2）添加5个[0,100)以内的整数到集合中，

（3）使用foreach遍历输出5个整数，

（4）使用集合的removeIf方法删除偶数，为Predicate接口指定泛型

（5）再使用Iterator迭代器输出剩下的元素，为Iterator接口指定泛型。

### 案例二：Comparable接口

（1）声明矩形类Rectangle，包含属性长和宽，属性私有化，提供有参构造、get/set方法、重写toString方法，提供求面积和周长的方法。

（2）矩形类Rectangle实现java.lang.Comparable接口，并指定泛型为，重写int compareTo(T t)方法，按照矩形面积比较大小，面积相等的，按照周长比较大小。

（3）在测试类中，创建Rectangle数组，并创建5个矩形对象

（4）调用Arrays的sort方法，给矩形数组排序，并显示排序前后的结果。

### 2 自定义泛型类与泛型接口

当我们在类或接口中定义某个成员时，该成员的相关类型是不确定的，而这个类型需要在使用这个类或接口时才可以确定，那么我们可以使用泛型。

- 当某个类/接口的非静态实例变量的类型不确定，需要在创建对象或子类继承时才能确定
- 当某个（些）类/接口的非静态方法的形参类型不确定，需要在创建对象或子类继承时才能确定

语法格式：

```java
【修饰符】 class 类名<类型变量列表> 【extends 父类】 【implements 父接口们】{

}
【修饰符】 interface 接口名<类型变量列表> 【extends 父接口们】{

}
```

### 3 使用泛型类与泛型接口小结

在使用这种参数化的类与接口时，我们需要指定泛型变量的实际类型参数：

（1）实际类型参数必须是引用数据类型，不能是基本数据类型

（2）子类继承泛型父类时，子接口继承泛型父接口、或实现类实现泛型父接口时，

- 指定类型变量对应的实际类型参数，此时子类或实现类不再是泛型类

```java
package com.atguigu.genericclass.define;

//ChineseStudent不再是泛型类
public class ChineseStudent extends Student<String>{

    public ChineseStudent() {
        super();
    }

    public ChineseStudent(String name, String score) {
        super(name, score);
    }

}
public class Rectangle implements Comparable<Rectangle>
```

- 指定类型变量（该类型变量可以和原来字母一样，也可以换一个字母），此时子类、子接口、实现类仍然是泛型类或泛型接口

```java
public interface Iterable<T>
public interface Collection<E>extends Iterable<E>
public interface List<E>extends Collection<E>
public class ArrayList<E>extends AbstractList<E>implements List<E>, RandomAccess, Cloneable, Serializable
```

（3）在创建泛型类的对象时指定类型变量对应的实际类型参数

```java
package com.atguigu.genericclass.define;

public class TestStudent {
    public static void main(String[] args) {
        //语文老师使用时：
        Student<String> stu1 = new Student<String>("张三", "良好");
        ChineseStudent chineseStudent = new ChineseStudent("张三", "良好");

        //数学老师使用时：
        //Student<double> stu2 = new Student<double>("张三", 90.5);//错误，必须是引用数据类型
        Student<Double> stu2 = new Student<Double>("张三", 90.5);

        //英语老师使用时：
        Student<Character> stu3 = new Student<Character>("张三", 'C');

        //错误的指定
        //Student<Object> stu = new Student<String>();//错误的
    }
}
```

> JDK1.7支持简写形式：Student stu1 = new Student<>("张三", "良好");
> 指定泛型实参时，必须左右两边一致，不存在多态现象

### 4 泛型方法的调用

在java.util.Arrays数组工具类中，有很多泛型方法，例如：

- public static List asList(T... a)：将实参对象依次添加到一个固定大小的List列表集合中。
- public static T[] copyOf(T[] original, int newLength)：复制任意对象数组，新数组长度为newLength。
- ...

```java
package com.atguigu.genericmethod;

import java.util.Arrays;
import java.util.List;

public class TestArrays {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("java", "world", "hello", "atguigu");
        System.out.println(list);

        String[] arr = {"java", "world", "hello"};
        String[] strings = Arrays.copyOf(arr, arr.length * 2);
        System.out.println(Arrays.toString(strings));
    }
}
```

泛型方法在调用时，由实参的类型确定泛型方法类型变量的具体类型。

### 5 自定义泛型方法

前面介绍了在定义类、接口时可以声明<类型变量>，在该类的方法和属性定义、接口的方法定义中，这些<类型变量>可被当成普通类型来用。但是，在另外一些情况下，

（1）如果我们定义类、接口时没有使用<类型变量>，但是某个方法形参类型不确定时，这个方法可以单独定义<类型变量>；

（2）另外我们之前说类和接口上的类型形参是不能用于静态方法中，那么当某个静态方法的形参类型不确定时，静态方法可以单独定义<类型变量>。

语法格式：

```java
【修饰符】 <类型变量列表> 返回值类型 方法名(【形参列表】)【throws 异常列表】{
    //...
}
```

- <类型变量列表>：可以是一个或多个类型变量，一般都是使用单个的大写字母表示。例如：、等。
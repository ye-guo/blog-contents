##  Java集合、Map、ListIterator、Stream流

#### 字符串

![](https://files.catbox.moe/vsyw0m.png)

### 一、Collection

#### 1.boolean add(E e)e

​	把给定对象添加到当前集合中

#### 2.void clear()

​	清空集合中所有元素

#### 3.boolean remove(E e)

​	把给定的对象在当前集合中删除

#### 4.boolean contains(Object obj)

​	判断当前集合中是否包含给定的对象

#### 5.boolean isEmpty()

​	判断当前集合是否为空

#### 6.int size()

​	返回集合中元素的个数

### 二、List

#### 1.void add(int index,E element)

​	在此集合中的指定位置插入指定的元素

#### 2.E remove(int index)

​	删除指定索引处的元素，返回被删除的元素

#### 3.E set(int index,E element)

​	修改指定索引处的元素，返回被修改的元素

#### 4.E get(int index) 

​	返回指定索引处的元素

### 三、TreeSet

#### 1.add(E e)

#### 2.first()

​	返回集合中第一个元素

#### 3.last()

​	返回集合中最后一个元素

#### 4.higher(E e)

​	返回集合中严格大于给定元素的最小元素

#### 5.lower(E e)

​	返回集合中严格小于给定元素的最大元素

#### 6.pollFirst()

​	移除并返回集合中的第一个元素

#### 7.pollLast()

​	移除并返回集合中的最后一个元素

### 三、ListIterator

​	在原迭代器基础上添加了可以添加元素方法add()
### 四、Map
​	键唯一，值不一定

#### 1.v put(k key, v value)

​	添加元素，返回被覆盖的元素

#### 2.v remove(Object key)

​	根据键删除键值对元素

#### 3.void clear()

​	移除所有的键值对元素

#### 4.boolean containsKey(Object key)

​	判断集合是否包含指定的键

#### 5.boolean containsValue(Object value)

​	判断集合是否包含指定的值

#### 6.boolean isEmpty()

​	判断集合是否为空

#### 7.int size()

​	集合的长度，也就是集合中键值对的个数



### 五、集合继承结构图

![](https://files.catbox.moe/fuu768.png)

### 七、Map继承结构图

![](https://files.catbox.moe/qvlwvr.png)

### 八、Stream流

#### 1.单列集合

​	Collection中默认方法 default Stream<E> stream() //创建流

#### 2.双列集合

​	无，先将键值对转为集合使用Collection的stream()方法  //创建流

#### 3.数组

​	使用Arrays工具类的静态方法stream()  //创建流

#### 4. 零散数据	

​	使用Stream接口中的静态方法stream()  //创建流

#### 5.Stream流中间方法

​	1.Stream<T>filter(Predicate<? super T> predicate) 				 过滤

​	2.Stream<T> limit(long maxSize)   				获取前几个元素

​	3.Stream<T> skip(long n)    							跳过前几个元素

​	4.Stream<T> distinct()      								元素去重，依赖(hashCode和equals方法)

​	5.static<T> Stream<T> concat(Stream a, Stream b)   合并a、b两个流为一个流

​	6.Stream<R> map(Function<T,R> mapper)   			    转换流中的数据类型

注意：中间方法，返回新的Stream流，原来的Stream流只能使用一次，建议使用链式编程
修改Stream流中的数据，不会影响原来集合或者数组中的数据

#### 6.Stream流终结方法

​	1.void forEach(consumer action)    遍历

​	2.long count()                                      统计

​	3.toArray() 											收集流中数据放到数组中

​	4.collect(Collector collector) 			收集流中数据，放到集合中

### 九、方法引用::(引用符)

#### 1.方法引用条件 

​	1.引用处必须是函数式接口

​	2.被引用的方法必须已经存在

​	3.被引用的方法的形参和返回值需要跟抽象方法保持一致

​	4.被引用方法的功能要满足当前需求

#### 2.方法引用分类

##### 1.引用静态方法

​	格式：类名::静态方法

​	范例：Integer::parseInt

#### 2.引用成员方法

​	格式：对象::成员方法

​	其他类：其他类对象::方法名

​	本类：	this::方法名      // 静态方法中没有this,需要new一个本类的对象调用

​	父类：	super:: 方法名	//静态方法中没有super ,需要new一个本类的对象调用

#### 3.引用构造方法

​	格式：类名::new

​	范例：Student::new

#### 4.其他调用方式

​		1.使用类名引用成员方法

​			格式：类名::方法名

​			范例：String::toUpperCase

​			规则：
​				**1.需要有函数式接口
​				2.被引用的方法必须已经存在
​				3.被引用方法的形参，需要跟抽象方法的第二个形参到最后一个形参保持					一致,返回值需要保持一致。
​				4,被引用方法的功能需要满足当前的需求**   			 

抽象方法形参的详解：
第一个参数：**表示被引用方法的调用者，决定了可以引用哪些类中的方法
					    在Stream流当中，第一个参数一般都表示流里面的每一个数据。
						假设流里面的数据是字符串，那么使用这种方式进行方法引用，只能						引用String这个类中的方法**
第二个参数到最后一个参数：**跟被引用方法的形参保持一致，如果没有第二个参数，说明被引用的方法需要是无参的成员方法**

总结：**如果抽象方法的第一个参数是A类型的只能引用A类中的方法，被引用方法的形参，需要跟抽象方法的第二个形参到最后一个形参保持一致，返回值需要保持一致。**

​	2.引用数组的构造方法

​		格式：数据类型[]::new

​		范例：int[]::new

​		创建一个指定类型的数组

​		细节：**数组的类型，需要跟流中数据的类型保持一致。**

### 十、异常

#### 1.结构图

![](https://files.catbox.moe/3g15cu.png)

运行时异常和编译时异常的区别？
	编译时异常：除了RuntimeExcpetion和他的子类，其他都是编译时异常
							编译阶段需要进行处理，作用在于提醒程序员
	运行时异常：RuntimeException本身和所有子类，都是运行时异常。编译阶段不报错，							是程序运行时出错的,一般是由于参数传递错误带来的问题

#### 2. Throwable的成员方法

​	1.public String getMessage()   返回此throwable的详细消息字符串

​	2.public String toString()   返回此可抛出的简短描述

​	3.public void printStackTrace()  把异常的错误信息输出在控制台

### 十一、File

#### 1.三个构造方法

* public File(String pathname)  			根据文件路径创建文件对象
* public File(String parent,String child)根据父路径名字符串和子路径名字符串创建文件对象
* public File(File parent,String chilld)   根据父路径文件对象和子路径名字符串创建文件对象

#### 2.常见成员方法

![](https://files.catbox.moe/5ssqls.png)

![](https://files.catbox.moe/5g022t.png)

delete方法细节

* 如果删除的是文件，则直接删除，不走回收站。
* 如果删除的是空文件夹则直接删除，不走回收站
* 如果删除的是有内容的文件夹，则删除失败

![](https://files.catbox.moe/e9a01e.png)

* 当调用者File表示的路径不存在时，返回null
* 当调用者File表示的路径是文件时，返回null
* 当调用者File表示的路径是一个空文件夹时，返回一个长度为0的数组
* 当调用者File表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
* 当调用者Fil表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件
* 当调用者File表示的路径是需要权限才能访问的文件夹时，返回nu

### 十二、IO流

![](https://files.catbox.moe/jr3ztd.png)

#### 一、什么是I0流？

* 存储和读取数据的解决方案
  	I  : input
  	O: output
  	流：像水流一样传输数据

2.10流的作用？

* 用于读写数据（本地文件，网络）》

3.O流按照流向可以分类哪两种流？
	输出流：程序 --->文件
	输入流：文件---->程序
4.IO流按照操作文件的类型可以分类哪两种流？
	字节流：可以操作所有类型的文件
	字符流：只能操作纯文本文件
5.什么是纯文本文件？
	用windows.系统自带的记事本打开并且能读懂的文件
	txt文件，md文件，ml文件，lrc文件等

#### 二、字节流

![](https://files.catbox.moe/ytxw7s.png)

![](https://files.catbox.moe/mnu93u.png)

![](https://files.catbox.moe/k7k3du.png)

![](https://files.catbox.moe/mnsr4s.png)

#### 三、编码规则

![](https://files.catbox.moe/xtu5fj.png)

![](https://files.catbox.moe/hcwv52.png )

**默认为UTF-8**

![](https://files.catbox.moe/0wqor9.png)

#### 四、字符流

![](https://files.catbox.moe/trphd1.png)

![](https://s3.bmp.ovh/imgs/2023/09/02/4d9f265d94025145.jpg)

![](https://s3.bmp.ovh/imgs/2023/09/02/2a4e34887320b03e.jpg)

字符流原理解析
①创建字符输入流对象
	底层：关联文件，并创建缓冲区（长度为8192的字节数组)
②读取数据
	底层：1.判断缓冲区中是否有数据可以读取
				2.缓冲区没有数据：就从文件中获取数据，装到缓冲区中，每次尽可能装满					缓冲区如果文件中也没有数据了，返回-1
				3.缓冲区有数据：就从缓冲区中读取。
空参的read方法：一次读取一个字节，遇到中文一次读多个字节，把字节解码并转成十进制返回
有参的read方法：把读取字节，解码，强转三步合并了，强转之后的字符放到数组中.

#### 五、应用场景

* 字节流：拷贝任意类型的文件
* 字符流：读取纯文本文件中数据，往纯文本文件中写出数据

### Ⅱ高级流

#### 六、缓冲流

![](https://files.catbox.moe/rf7kmd.png)

#### 七、转换流

1.转换流的名字是什么？

* 字符转换输入流：InputStreamReader
* 字符转换输出流：OutputStreamWriter

2.转换流的作用是什么？

* 指定字符集读写数据（UDK11之后已淘汰)
* 字节流想要使用字符流中的方法了

#### 八、序列化流

![](https://files.catbox.moe/kzzcjo.png)

![boyohc.png](https://files.catbox.moe/boyohc.png)

![](https://files.catbox.moe/f9gjgh.png)

#### 九、打印流

![76pkuu.png](https://files.catbox.moe/76pkuu.png)

![xth71q.png](https://files.catbox.moe/xth71q.png)

![xvzuwo.png](https://files.catbox.moe/xvzuwo.png)

![6lyzg7.png](https://files.catbox.moe/6lyzg7.png)

1.打印流有几种？各有什么特点？

* 有字节打印流和字符打印流两种
* 打印流不操作数据源，只能操作目的地
* 字节打印流：默认自动刷新，特有的println自动换行
* 字符打印流：自动刷新需要开启，特有的println自动换行

#### 十、解压缩流

#### 十一、多线程

![4d6gft.png](https://files.catbox.moe/4d6gft.png)

##### 一、生命周期

![z2ww8b.png](https://files.catbox.moe/z2ww8b.png)

![cjw0sw.png](https://files.catbox.moe/cjw0sw.png)

![x9ofvr.png](https://files.catbox.moe/x9ofvr.png)

#### 十二、网络编程

![](https://files.catbox.moe/692xc5.png)

#### 十三、反射

##### 1.获取class对象的三种方式

![gys1m5.png](https://files.catbox.moe/gys1m5.png)

##### 2.利用反射获取构造方法

![4usg73.png](https://files.catbox.moe/4usg73.png)

##### 3.利用反射获取成员变量

![lkpspg.png](https://files.catbox.moe/lkpspg.png)

##### 4.利用反射获取成员方法

![f3y5en.png](https://files.catbox.moe/f3y5en.png)

##### 5.总结

![wcl7gq.png](https://files.catbox.moe/wcl7gq.png)

#### 十四、动态代理

> 大多数内容摘选来自Core Java, Thinking in Java。部分来自网络

`heap堆` `stack堆栈` `queue队列` `reference引用`

# 控制程式流程
1. Java标准程式库中的大多数`classes`都复写了`equals()`，所有它们都会比较对象（而非其`references`）的内容是否相等。
2. 在比`int`更小的基本类型（亦即`char、byte、short`）上进行任何数学运算或位元运算时，运算之前其值会先被晋升为`int`，最后所得结果也会是`int`类型。
3. 在迭代述句的主体内，`break`会跳出循环，不在执行剩余部分。`continue`会停止当次迭代，回到循环起始处，开始下一个迭代过程。
4. `labeld continue`会跳跃至`label`所在处，然后恰在`label`之后重新进入循环。`labeld break`会跳跃`label`所描述的循环。在Java里头使用`labels`，唯一的理由是：在巢状回圈中想要令`break`或`continue`越过一个以上的巢状层级（`nested level`)
5. `Math.random()`会产生介于0和1之间的值（包括0），`[0，1)`

# 面向对象

&emsp;&emsp;面向对象的程序是由对象组成的，每个对象包含对用户公开的特定功能部分和隐藏的实现部分。只要对象能够满足要求，就不必关心其功能的具体实现过程。在OOP中，不必关心对象的具体实现，只要能够满足用户的需求即可。

&emsp;&emsp;传统的结构化程序设计通过设计一系列的过程（即算法）来求解问题。一旦确定了这些过程，就要开始考虑存储数据的方式。而OOP却调换了这个次序，将数据放在第一位，然后再考虑操作数据的算法。

> &emsp;&emsp;实现一个简单的Web 浏览器可能需要大约2000个过程，这些过程可能需要对一组全局数据进行操作。采用面向对象的设计风格，可能只需要大约100个类，每个类平均包含20个方法。后者更易于程序员掌握，也容易找到bug。假设给定对象的数据出错了，在访问过这个数据项的20个方法中查找错误要比在2000个过程中查找容易得多。

&emsp;&emsp;**封装**（`encapsulation`,有时称为数据隐藏）是与对象有关的一个重要概念。从形式上看，封装不过是将数据和行为组合在一个包中，并对对象的使用者隐藏了数据的实现方式。对象中的数据称为实例域（`instance field`), 操纵数据的过程称为方法（`method`)。对于每个对象都有一组特定的实例域值。这些值的集合就是这个对象的当前状态（`state`)。无论何时，只要向对象发送一个消息，它的状态就有可能发生改变。

&emsp;&emsp;实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域。程序仅通过对象的方法与对象数据进行交互。

## 对象的三个主要特性

+ 行为`behavior`——可以对对象施加哪些操作或方法
+ 状态`state`——当施加那些方法时，对象如何响应
+ 标识`identity`——如何辨别具有相同行为和状态的不同对象

&emsp;&emsp;同一个类的所有对象实例，由于支持相同的行为而具有家族式的相似性。对象的行为是用可调用的方法定义的。

&emsp;&emsp;每个对象都保存着描述当前特征的信息。这就是对象的状态。对象的状态可能会随着时间而发生改变，但这种改变不会是自发的。对象状态的改变必须通过调用方法实现（如果不经过方法调用就可以改变对象状态，只能说明封装性遭到了破坏）。

&emsp;&emsp;对象的状态并不能完全描述一个对象。每个对象都有一个唯一的身份(`identity`)。

## 类之间的关系

+ 依赖`user-a`
+ 聚合`has-a`
+ 继承`is-a`

&emsp;&emsp;一个类的方法操纵另一个类的对象，我们就说一个类依赖于另一个类。应该尽可能地将相互依赖的类减至最少。如果类A不知道B的存在，它就不会关心B的任何改变（这意味着B的改变不会导致A产生任何bug）。用软件工程的术语来说，就是让类之间的耦合度最小。

&emsp;&emsp;聚合关系意味着类A的对象包含类B的对象。

## 封装的优点

将一个实例域设置为`private`，并提供`public`的**域访问器**和**域更改器**虽然复杂些，但是却有着下列明显的好处：

1. 可以改变内部实现，除了该类的方法之外，不会影响其他代码。
	```java
	public String getName() {
		return firstName + " " + lastName;
	}
	```
2. 更改器方法可以执行错误检查。

> 注意不要编写返回引用可变对象的访问器方法。如果需要，应该首先对它进行克隆(`clone`)。
```java
class Employee {
	private Date hireDay;
	public Date getHireDay() {
		return (Date) hireDay.clone();
	}
}
```

## 方法参数

&emsp;&emsp;Java程序设计语言总是采用**按值调用**。也就是说，方法得到的是所有参数值的一个拷贝，特别是，方法不能修改传递给它的任何参数变量的内容。

&emsp;&emsp;**注意**：Java程序设计语言对对象采用的不是引用调用，实际上，对象引用是按值传递的。

```java
public static void swap(Employee x, Employee y) {
	Employee temp = x;
	x = y;
	y = temp;
}
Employee a = new Employhee("Alice");
Employee b = new Employhee("Bob");
swap(a, b);
// 方法并没有改变存储在变量a和b中的对象引用
```
## 类设计技巧

1. 一定要保证数据私有
2. 一定要对数据初始化
3. 不要在类中使用过多的基本类型

	用其他的类代替多个相关的基本类型的使用。这样会使类更易于理解且易于修改。例如，用一个成为`Address`的新类替换一个`Customer`类中以下的实例域：
	```java
	private String street;
	private String city;
	private String state;
	private int zip;
	```

4. 不是所有的域都需要独立的域访问器和域更改器

	有一些实例域不应该被修改或访问
5. 将职责过多的类进行分解
6. 类名和方法名要能够体现它们的职责

	类命名的良好习惯是采用一个名词（Order）、前面有形容词修饰的名词（RushOrder）或动名词（有“-ing”后缀）修饰名词（例如，BillingAddress）。
7. 优先使用不可变的类

	更改对象的问题在于，多个线程试图同时更新一个对象，就会发生并发更改。其结果是不可预料的。如果类是不可变的，就可以安全地在多个线程之间共享其对象。

# 继承

&emsp;&emsp;一个对象变量可以指示多种实际类型的现象被称为多态（`polymorphism`）。在运行时能够自动地选择调用哪个方法的现象称为动态绑定（`dynamic binding`）。

&emsp;&emsp;如果是`private`方法、`static`方法、`final`方法或者构造器，那么编译器将可以准确地知道应该调用哪个方法， 我们将这种调用方式称为静态绑定(`static binding`)。与此对应的是，调用的方法依赖于隐式参数的实际类型， 并且在运行时实现动态绑定。

&emsp;&emsp;每次调用方法都要进行搜索，时间开销相当大。因此，虚拟机预先为每个类创建了一个方法表(`method table`), 其中列出了所有方法的签名和实际调用的方法。这样一来，在真正调用方法的时候，虚拟机仅查找这个表就行了。

&emsp;&emsp;`final`类不能被继承，`final`类的方法自动成为`final`，但不包括域。

&emsp;&emsp;`final`方法不能被覆盖（即重写，但是可以重载）。`private`方法自动成为`final`。子类访问不了父类的`private`方法，但是可以定义一个跟它一样的方法，这种写法类似重写，但实际不是，因为访问不了，何来的重写。

&emsp;&emsp;`final`域在构造对象后，就不允许被改写。

## 控制可见性的4个访问修饰符

1. 仅对本类可见——private
2. 对所有类可见——public
3. 对本包和所有子类可见——protected
4. 对本包可见——默认

# 包
&emsp;&emsp;`import`语句的唯一的好处是简捷。可以使用简短的名字而不是完整的包名来引用。

&emsp;&emsp;如果使用`import`语句引入整个包中的类`import java.util.*;`，那么可能会增加编译时间，但不会影响程序运行的性能，因为当程序执行时，只能将真正使用的类的字节码文件加载到内存。

&emsp;&emsp;`import`语句不仅可以导入类，还增加了导入静态方法和静态域的功能。

# 初始化与清理
## **初始化顺序**
1. 创建对象时，在堆上分配储存空间，将字段初始化（基本类型设置为默认值，引用类型为空）
2. 执行所有出现于字段定义处的初始化动作
3. 执行构造器
## **静态初始化**
- 静态对象先于“非静态”对象初始化
- 只有在第一个该类对象被创建（或者第一次访问静态数据，常量不算）的时候，才会被初始化
- 静态块与其他静态初始化动作一样

# 复用类
+ **final变量**：所有`final`变量都要初始化，不是在定义处，就是在构造器上
+ **final参数**：无法在方法中让参数指向别的引用
+ **final方法**：子类无法复写
+ **final类**：不能用于继承。所有方法都是`final`
+ `private`声明的方法都是`final`的

# 多态
1. Metchod-call（函式呼叫）连结方式：先期连结，后期连结（执行期连结、动态连结）
2. Java中除了static方法和final方法，其他方法都是后期连结（后期绑定）
3. **复制对象调用构造器的顺序：**
	1. 调用基类构造器（base class）
	2. 按声明顺序初始化成员
	3. 调用到导出类构造器的主体（derived class）

# 内部类
1. **成员式内部类：**
	1. 有static修饰符则为类级（静态内部类），否则为对象级。类级可以通过外部类直接访问，对象级需要先生成外部的对象后才能访问：`outObjectName.new`
	2. 内部类访问外部类对象：`outClassName.this`
	3. 非静态内部类中不能声明任何static成员
	4. 一般把内部类声明成private的，这样除了外部类以外没人能访问。完全隐藏实现的细节。
2. **局部内部类（包括匿名内部类）：**
	1. 定义在方法和作用域中的类，只在代码块中可见。优势是对外界隐藏
	2. 不能用public、private、protected修饰，只能使用缺省的
	3. 局部内部类只能访问final变量
	4. 匿名类可以创建，接口，抽象类，与普通类的对象。

> 最吸引人的原因，每个内部类都能独立继承一个接口，而无论外部类是否已经继承了某个接口。inner class是多重继承问题的完整解决方案。 

# 持有对象
## 数组

&emsp;&emsp;`array`是Java用来储存及随机存取一连串对象的各种作法中，最有效率的一种。想储存一大群对象，第一选择应该是`array`，如果储存一群基本类别的数值，也只能选择`array`。
&emsp;&emsp;“不规则”数组：数组的每一行有不同的长度。
```java
// 定义和初始化
int[] a1;
int a1[];
int[] a1 = {1, 2, 3, 4, 5, 6};
int[] a1 = new int[6];
```

## 工具类Arrays

+ `static String toString(type[] a)` 5.0
	返回包含a中数据元素的字符串，这些数据元素被放在括号内，并用逗号分隔
+ `static type copyOf(type[] a, int length)` 6
+ `static type copyOfRange(type[] a, int start, int end)` 6
	返回与a类型相同的一个数组，其长度为`length`或者`end`(不包含)-`start`(包含)，数组元素为a的值
+ `static void sort(type[] a)`
	采用优化的快速排序算法对数组进行排序
	```java
	// 引用类型两种比较方案：
	// Comparable interface(可比较的)
	class Person implements Comparable {// 将要比较的对象实现Comparable接口
	    String name;
	    int age;
	    public int compareTo(Person another) {// 实现compareTo方法
	        return age - another.age;
	    }
	}
	Person[] a = new Person[10];
	Arrays.sort(a);
	// Comparator interface(比较器)
	class PersonComparator implements Comparator {
	    public int compare(Person one, Person another) {
	        return one.age - another.age;
	    }
	}
	Arrays.sort(a, new PersonComparator());
	```
+ `static int binarySearch(type[] a, type v)`
+ `sattic int binarySearch(type[] a, int start, int end, type v)` 6
	采用二分搜索算法查找值v。如果查找成功，返回下标值；否则返回一个负数值r。`-r-1`为v应插入的位置
+ `static void fill(type[] a, type v)`
	将数组的所有数据元素值设置为v
+ `static boolean equals(type[] a, type[] b)`
	如果两个数据大小相同，并且下表相同的元素都对应相等，返回true
+ `asList()`
	接收一个数组或一个用逗号分隔的元素列表，转换为List对象
	```java
	// Collection的构造方法可以接收一个Collection
	Collection<Integer> collection = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
	// 固定大小的List，不可进行修改操作
	Collection<Integer> collection = Arrays.asList(1, 2, 3, 4, 5); 
	```

## 容器Containers
## `Collection`（独立元素的序列）
`List`，以特定次序储存一组元素；`set`：元素不得重复（必须定义`equals()`以判断唯一性）

+ `ArrayList`
    允许快速随机存取。元素的安插或移除发生于List中央位置时效率差，应该只拿ListIterator来进行向后或向前走访动作
+ `LinkedList`
    提供最佳循序存取，以及成本低廉的List中央位置元素安插与移除。随机存取动作相对缓慢。具备`addFirst() addLast() getFirst() getLast() removeFirst() removeLast()`
+ `HashSet`
    把搜寻时间看得很重要的`Sets`。所有元素都必须定义`hashCode()`
+ `TreeSet`
     底层结构为`tree`的一种有序的Set。
## `Map`（键值对）
`keySet()`会将`Map`内的`Keys`生成一个`Set`。`values()`的行为类似

+ `HashMap`
    **put的流程：**
    当我们想一个`HashMap`中添加一对`key-value`时，系统首先会计算`key`的`hash`值，然后根据`hash`值确认在`table`中存储的位置。
    若该位置没有元素，则直接插入。否则迭代该处元素链表并依此比较其key的hash值。如果两个hash值相等且key值相等`(e.hash == hash && ((k = e.key) == key || key.equals(k)))`，则用新的Entry的value覆盖原来节点的value。如果两个hash值相等但key值不等 ，则将该节点插入该链表的链头。

+ `TreeMap`

## **工具类Collections**
+ `addAll()`: 接收一个Collection对象，以及一个数组或一个用逗号分隔的列表，将元素添加到Collection中
```java
Integer[] moreInts = {6, 7, 8, 9, 10};
Collections.addAll(collection, moreInts);
Collections.addAll(collection, 11, 12, 13, 14, 15);
```
+ `reverseOrder()`: 返回一个指向Comparator的引用，可将正常的排列顺序颠倒
+ `fill()`：将同一个对象引用复制到容器每个位置上，只对List有效
## **迭代器Iterator**
为何使用：可被用于不同类型的容器上，不需要每次重新撰写一份
```java
Iterator e = containersObject.iterator();// 要求容器返回一个Iterator
while(e.hasNext())// 检查序列中是否还有其他元素
    e.next();// 取得序列中的下一个元素
```

# String

**不可变字符串**的优点：编译器可以让字符串共享。

> &emsp;&emsp;如果虚拟机始终将相同的字符串共享，就可以使用`==`运算符检测是否相等。但实际上只有字符串常量是共享的，而`+`或`substring`等操作产生的结果并不是共享的。因此，千万不要使用`==`运算符测试字符串的相等性。
—— Core Java

&emsp;&emsp;Java字符串由`char`值序列组成。`char`数据类型是一个采用`UTF-16`编码表示`Unicode`码点的代码单元。大多数的常用`Unicode`字符使用一个代码单元就可以表示，而辅助字符需要一对代码单元表示（即一个字符可能是2或4个字节）

	:::java
	String str = "abc";
	// 等效于
	char data[] = {'a', 'b', 'c'};
	String str = new String(data);

&emsp;&emsp;`length()`返回采用`UTF-16`编码表示的给定字符串所需要的代码单元的数量。想要得到实际的长度，即码点数量：`codePointCount(0, greeting.length());`

```java
// 遍历一个字符串，并且依次查看每个码点：
for (int i = 0; i < sentence.length();) {
	int cp = sentence.codePointAt(i);
	if (Character.isSupplementaryCodePoint(cp)) i += 2;
	else i++;
}

// 流方法
int[] codePoints = str.codePoints().toArray();
```

## String API

+ `char charAt(int index)`
	返回给定位置的代码单元。除非对底层的代码单元感兴趣，否则不需要调用这个方法
+ `int codePointAt(int index)` 5.0
	返回从给定位置开始的码点
+ `int offsetByCodePoints(int startlndex, int cpCount)` 5.0
	返回从startlndex 代码点开始， 位移cpCount 后的码点索引。
+ `int compareTo(String other)`
	按照字典顺序，如果字符串位于other 之前，返回一个负数；之后，返回一个正数；相等，返回0。
+ `IntStream codePoints()` 8
	将这个字符串的码点作为一个流返回。调用toArray 将它们放在一个数组中。
+ `new String(int[] codePoints, int offset, int count)` 5.0
	用数组中从offset 开始的count 个码点构造一个字符串。
+ `boolean equals(0bject other)`
	如果字符串与other 相等， 返回true。
+ `boolean equalsIgnoreCase(String other)`
	如果字符串与other 相等（忽略大小写)，返回true。
+ `boolean startsWith(String prefix)`
+ `boolean endsWith(String suffix)`
	如果字符串以suffix 开头或结尾， 则返回true。
+ `int indexOf(String str)`
+ `int indexOf(String str, int fromlndex)`
+ `int indexOf(int cp)`
+ `int indexOf(int cp, int fromlndex)`
	返回与字符串str 或代码点cp 匹配的第一个子串的开始位置。这个位置从索引0 或fromlndex 开始计算。如果在原始串中不存在str， 返回-1。
+ `int lastIndexOf(String str)`
+ `Int lastIndexOf(String str, int fromlndex)`
+ `int lastindexOf(int cp)`
+ `int lastindexOf(int cp, int fromlndex)`
	返回与字符串str 或代码点cp 匹配的最后一个子串的开始位置。这个位置从原始串尾端或fromlndex 开始计算。
+ `int length( )`
	返回字符串的长度。

## StringBuilder

+ `StringBuilder()`
	构造一个空的字符串构建器。
+ `int length()`
	返回构建器或缓冲器中的代码单元数量。
+ `StringBuilder append(String str)`
	追加一个字符串并返回this。
+ `StringBuilder append(char c)`
	追加一个代码单元并返回this。
+ `StringBuilder appendCodePoint(int cp)`
	追加一个代码点，并将其转换为一个或两个代码单元并返回this。
+ `void setCharAt(int i,char c)`
	将第i 个代码单元设置为c。
+ `StringBuilder insert(int offset,String str)`
+ `StringBuilder insert(int offset,Char c)`
	在offset 位置插入一个字符串或代码单元并返回this。
+ `StringBuilder delete(int startIndex,int endIndex)`
	删除偏移量从startIndex 到endIndex-1 的代码单元并返回this。
+ `String toString()`
	返回一个与构建器或缓冲器内容相同的字符串

# 类型信息
+ `Class.forName("Gum");`，这个方法是`Class`类的一个`static`成员。`Class`对象就和其他对象一样，我们可以获取并操作它的引用。`forName()`是取得`Class`对象引用的一种方法。它是用一个包含目标类的文本名的`String`作输入参数，返回一个`Class`对象的引用,上面的代码忽略了返回值。对`forName()`的调用是为了它产生的“副作用”:如果`Gum`还没有被加载就加载它。在加载的过程中，`Gum`的`static`字句被执行。
+ 不需要为了获得`Class`引用而持有该类型的对象。但是如果已经拥有了一个感兴趣的类型的对象，那就可以用过调用`getClass()`方法来获取`Class`引用。
+ `forName()`的字符串必须使用全限定名（包含包名）。
`getName()`：产生全限定的类名
`getSimpleName()`：产生不含包名的类名
`isInterface()`：代表`Class`对象是否表示某个接口
`getSuperclass()`：查询基类
`newInstance()`：虚拟构造器，会得到`Object`引用，但引用指向确切的对象
+ **类字面常量**是生成`Class`对象引用的另一种方法：`FancyToy.class;`，类字面常量也可用于**接口**、**数组**、**基本数据类型**。对于基本数据类型的**包装器类**，还有一个标准字段`TYPE`，指向对应的基本数据类型的`Class`对象`Integer.TYPE`。
不同于`forName()`，`.class`创建`Class`对象引用时不会自动初始化该`Class`对象。
+ 为使用类而做的准备工作：
    1. **加载**，由类加载器执行。查找字节码。并从中创建Class对象。
    2. **链接**，验证类中的字节码，为静态域分配存储空间，并且如果必需的话，将解析这个类创建的对其他类的所有引用。
    3. **初始化**，如果该类具有超类，则对其初始化，执行静态初始化器和静态初始化块。初始化被延迟到了对**静态方法**或者**非常数静态域**进行首次引用时才执行。
+ 如果一个`class`不声明为`public`，那么在这个`class`的包外是不能访问它的`private`变量和方法的。但是通过**反射**却能访问甚至修改，除了`final`的变量，修改了没效果。
```java
Method g = a.getClass().getDeclaredMethod(methodName);// preivate methodName只需要通过javap就能取得
g.setAccessible(true);// 指示反射的对象在使用时应该取消 Java 语言访问检查
g.invoke(a);
```

# io
+ `ByteArrayInputSteam`、`StringBufferInputStream`、`FileInputStream`是三种基本的节点流，分别从**Byte数组**、**StringBuffer**、**本地文件**中读取数据。
+ `ByteArrayOutputStream`是用来缓冲数据的，向他的内部缓冲区写入数据，缓冲区自动增长，由于这个原因，常用于存储数据以用于一次写入。
+ `DataOutputStream`储存`String`时，字符串的长度储存在`UTF-8`字符串的前两个字节中。为了保证所有的读方法都能够正常工作，必须知道流中数据所在的确切位置（有可能将保存的`double`数据作为一个简单的字节序列或其他类型读入）。因此必须：要么为文件中的数据采用固定格式；要么将额外信息保存到文件中，以便能够解析。对象序列化和XML是更容易的存储和读取复杂数据结构的方式。
```java
out.writeInt(1);
out.writeUTF("That was pi");
out.writeInt(2);
out.writeUTF("Square root of 2");

0000 0001 000b 5468 6174 2077 6173 2070
6900 0000 0200 1053 7175 6172 6520 726f
6f74 206f 6620 32

// 第5 6个字节000b 第22 23个字节0010代表字符串长度
```
+ `Stream`用于二进制文件（非文本） `Writer/Reader`用于文本文件（虽然也是二进制，不过是按照一定的字符编码规则，不像前者）
+ **`PrintStream`和`PrintWriter`的区别**：
    + `PrintStream`是字节流，有处理`byte`的方法，`write(int b)`和`  write(byte[] buf, int off, int len)` `PrintWriter`是字符流，有处理`char`的方法，`write(int c)`和`write(char[] cbuf, int off, int len)`
    + `PrintStream`和`PrintWriter`的`autoflushing`机制不同，前者在**输出byte数组**、**调用println方法**、**输出换行符**或者**byte值10（即\n）**时自动调用`flush`方法，后者仅在**调用println方法**时发生`autoflushing`。
# 并发
**为什么使用并发**
1. **更快的执行**
    同一时间执行几个任务，不会因为某一任务计算量大而使整个程序等待它结束才能运行别的任务（阻塞）。两种实现方式：多CPU并行、单CPU时间分片。
2. **改进代码的设计**

# 相关知识
## 字符集、字符编码
字符集只是一个规则集合的名字，对应到真实生活中，字符集就是对某种语言的称呼。例如：英语，汉语，日语。
对于一个字符集来说要正确编码转码一个字符需要三个关键元素：字库表（`character repertoire`）、编码字符集（`coded character set`）、字符编码（`character encoding form`）。
可以这样理解：`Unicode`是字符集，`UTF-32`/`UTF-16`/`UTF-8`是三种字符编码方案。
**字库表**是一个相当于所有**可读**或者**可显示字符**的**数据库**，字库表决定了整个字符集能够展现表示的所有字符的范围。
**编码字符集**（如：`Unicode`），即用一个编码值`code point`来表示一个字符在字库中的位置。
**字符编码**（如：`UTF-8`），将编码字符集和实际存储数值之间的转换关系。
> 例：“严”字的Unicode序号为：`0x4E25`(`1001110 00100101`)
> UTF-8的物理储存数值：`0xE4B8A5`(`11100100 10111000 10100101`)

+ **码点 code point：**`Unicode`中为字符分配的编号。如：`A`对应`U+0041`
+ **码元 code unit：**是针对编码方法而言，字符的最小存储单元。`UTF-8`为一个字节、`UTF-16`为两个字节
+ `UTF-8`最大特点是：它是一种变长的编码方式。使用1~4个字节表示一个符号，根据不同的符号而变化字节长度

## UTF-8编码规则
+ 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的。
+ 对于n字节的符号(n>1)，第一个字节的前n位设为1，第n+1位设为0，后面字节的前两位一律设为10.剩下的没有提及的二进制位，全部为这个符号的unicode码。

    |Unicode符号范围（十六进制）    |UTF-8编码方式（二进制）               |
    |-------------------------------|:-------------------------------------|
    | `0000 0000`  →  `0000 007F`  | `0xxxxxxx`                           |
    | `0000 0080`  →  `0000 07FF`  | `110xxxxx 10xxxxxx`                  |
    | `0000 0800`  →  `0000 FFFF`  | `1110xxxx 10xxxxxx 10xxxxxx`         |
    | `0001 0000`  →  `0010 FFFF`  | `11110xxx 10xxxxxx 10xxxxxx 10xxxxxx`|

    > Unicode编码范围最大为0x10FFFF，也就是：1FFFFF - F0000

> 参考：[字符编码](https://github.com/acmerfight/insight_python/blob/master/Unicode_and_Character_Sets.md)
## UTF-16编码规则
**辅助平面编码：**
基本多语言平面内，从`U+D800`到`U+DFFF`之间的码位区段是永久保留不映射到`Unicode`字符。`UTF-16`就利用保留下来的`0xD800`-`0xDFFF`区段的码位来对辅助平面的字符的码位进行编码。

**示例：例如`U+10437`编码（𐐷）:**

+ `0x10437`减去`0x10000`,结果为`0x00437`,二进制为`0000 0000 0100 0011 0111`
+ 分区它的上10位值和下10位值（使用二进制）:`0000000001 and 0000110111`
+ 添加`0xD800`到上值，以形成高位：`0xD800` + `0x0001` = `0xD801`
+ 添加`0xDC00`到下值，以形成低位：`0xDC00` + `0x0037` = `0xDC37`
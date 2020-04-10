##### JAVA核心：

##### Java分区：

Java运行时分为堆区、栈区、方法区：

1. 堆存放Java全部对象，每个对象包含与之对应的class信息；可被线程共享，不会存放别的对象引用

2. 栈：保存对象的引用；保存基本变量类型

3. 方法区（静态区）：保存程序中永远唯一的元素，包括静态方法、静态变量；被所有线程共享

   栈的存取速度极快，数据能共享，但是大小需提前确定，缺乏灵活性

   

##### 继承与多态

1. 子类可以使用super（）来调用父类的方法，包括成员方法和构造方法；

   子类也可以重写（覆盖）父类的方法，即是用父类的方法名，重新写成员方法的实现内容，更改成员方法的存储权限，或是修改成员方法的返回值类型；

   ​	在继承中有一种特殊的重写方式，子类与父类的成员方法返回值、方法名、参数类型与个数完全相同，唯一不同的是方法的实现内容，这种特殊重写方式被称为**重构**

   子类的构造方法必调用父类的构造方法；无论显式还是隐式调用。就是说实例化子类对象时首先要实例化父类对象。

   无参构造方法可以被隐式调用，有参构造方法只能使用super（）来显示调用 

   

2. 重写父类方法时，修改方法的修饰全线只能从小范围到大范围改变。子类权限必须大于等于父类权限

   

3. ###### 重写父类方法中，当修改的是父类方法的返回值时，这个返回值应该是父类中同一方法返回值类型的子类

   

4. object类

   Java中所有类都是继承自object类（艹，难怪之前看见的Java程序都要import java.lang.Object）Java中所有的类都是object的子类

   object类中有几个重要的方法：

   	1. getClass（）返回当前所在类的实例，一般常用来获取getClass（）获取名称：getClass（）.getname()
    	2. toString（）
    	3. equals（）方法：比较两个对象的内容（记得与==的区别啊，==比较的是对象引用）

   

5. 对象类型转换

   1. 向上转型：从具体到抽象的转换“鸽子是鸟”，就是说子类对象向上转成父类对象，即将子类对象赋值给父类对象类型：父类 obj = new 子类（），这时，我们创建这个父类对象其实就是子类的一种特殊父类对象，范围限制在子类对象范围（真几把复杂）

   2. 向下转型：即从抽象到具体，就是说“鸟是鸽子”，这样的转换当然会存在问题，有时需要进行强制转换，如  

      ```
      父类 object = new 子类（）；
      子类 obj = （子类）object
      ```

      **其实，这个父类本身就是子类对象的实例，只是他的对象本身是父类来定义，因此他能显式转换为子类**（这个很重要，在向下转型前一定要判断父类是否是子类对象的实例）

      判断使用instanceof来进行

      ```
      boolean bb = (a instanceof b);
      ```

      

      向下转型时必须使用显示类型转换。

   3. 其实吧，有一个小技巧可以快速判断是向上还是向下转型，以  类1   a =  类2 b为例，如果b是a 能说得通就是向上转型，否则就是向下，这时就需要先判断再强制转型

   

6. 方法重载

   一个类中允许出现同名方法，前提是这些同名方法的参数个数或者类型不同

   编译器是利用方法名、方法各参数类型和参数个数及参数顺序来确定类的方法是否唯一

   当谈到参数个数时，我们想到定义不定长参数方法

   ```
   返回值 方法名（参数数据类型...参数名）
   int funn(int ... a)
   这种形式可以看作为int[]a
   ```

   这里面需要使用for来循环遍历哦，记住

   

7. 插入一点：我们创建数组时，常规都是整型数字来创建数组如int []a = new int[];

   但不能仅仅限制于此，我们有时候还会出现我们自己创建的类来创建数组，格式一致：类  []  数组名 = new 类[n]

   

8. 多态

   就是之前说的在继承类中都需要调用同一个方法，使用子类参数，当程序量很大时，我们不能为每一个子类都写这个方法，因此我们可以在父类中写这个方法，当然父类中的参数一定是父类，但是无妨，我们可以使用之前说的向上转型在子类中调用父类方法

   方法中不同的子类类型我们可以创建一个数组来遍历筛选出目标数据类型进行子类方法写入

   

9. 抽象类

   之前的多态之后在实践中有时并不需要我们的父类进行初始化对象，我们需要的知识子类对象，这啊时候我们就用abstract关键字来对父类进行修饰，把父类描述为抽象类。这就是抽象类的来源，被abstract修饰的成为抽象类，这种类不能进行实例化，但是他的子类却可以。

   同时方法也可以使用abstract来修饰，成为抽象方法，抽象方法没有方法体，这个方法没有任何意义，除非他被重写，含有抽象方法的抽象类必须要被继承。一般来说，抽象类除了被继承之外无任何意义

   **也就是说如果声明一个抽象方法，就必须将这个类定义为抽象类。不能在非抽象类中获取抽象方法。**

   抽象类被继承后必须实现它里面的所有抽象方法。

   

10. 前面的多态机制下，我们可以将父类写成抽象类来表示，这是根据要求每个子类就不得不重写其中的抽象方法，但有时候有的子类并不需要，仅仅是部分子类需要，这时候我们就引入了接口概念。使得所有子类并不需要全部重写抽象方法，当需要某个方法时，直接继承目标方法的承载接口即可。

    接口中的方法必须被定义为public和abstract

    接口中的任何字段都自动是static和final

    Java中类不允许多继承，但是接口允许多继承存在



##### 类的高级特性：

1. 包：

   程序繁杂到一定程度时会出现类名管理困难，因为Java不能允许同名类出现，因此包的概念应运而出。

   Java中每个类都隶属于一个包

   import导入包时可以使用包.*来使用此包下的所有类

   

2. 使用javac程序命令；使用path环境变量中设置命令；使用set path  = C:\\JAVA\jdk.1.6.0_03\bin;%path%命令在DOS中设置path环境变量

   

3. import static 静态成员

   

4. final

   用来变量声明变量，此变量不可以再次被改变，基本上fianl用来定义变量为常量，如fianl  double PI = 3.14

   fianl在声明时必须对变量进行赋值操作，

   final不仅可以修饰变量，还可以修饰对象引用。由于数组可以被看做是一个对象来引用，所以final可以用来修饰数组。

   final声明的对象引用只能指向唯一一个对象，不可以将它再指向其他对象，但是一个对象本身的值可以被改变，因此若要是一个常量真正不可被更改，我们需要声明为static final

   Java中全局变量一般用public static final来修饰

   

5. final修饰的变量不可被更改，但是程序再次运行时可以被重新赋值，当被static修饰时，地址被创建，再次执行时仍然不改变，指向同一块地址内存

   

6. 有哪些修饰词呢？

   权限修饰词：private、protect、public

   抽象：abstract

   注：以上修饰词顺序可变

   数据类型：int double....

   

7. final方法

   final方法不可被重写

   private也可以看做是隐式地final方法类型

   

8. final类不能被继承

   

11. 内部类：成员内部类、局部内部类、匿名类

    

12. 内部类的对象实例化必须在外部类或者在外部类的费静态方法中实现

    

13. 一般情况下我们不能在一个类中多次实现接口的的同一个方法，但是有一个技巧，我们可以在内部类中实现同一个方法，这就完成了一个类中不同的响应事件

    

14. 非内部类不能被声明为peivate\peotected

    

15. 内外类的成员变量名称冲突时，使用this

16. 内存中所有对象都被放置在堆中，方法与方法的形参或局部变量放置在栈中

    

17. 如果需要在方法体中使用局部变量，该局部变量需要被设置为final类型，就是说，在方法体内的内部类只能访问到该方法中final类型的局部变量。因为方法中的局部变量相当于一个常量，他的生命周期超过方法运行的生命周期，所以被声明为final

    

18. 匿名内部类

    在return语句中插入一个定义内部类的代码，这个类没有名称，因此将该内部类定义为匿名内部类

    ```
    return new 类名(){
    ...
    }；
    ```

    匿名内部类定义结束后，需要加上分号标识。这个分号不代表内部类结束的表示，而是代表创建类引用表达式的标识。

    **使用匿名内部类必须要集成一个父类或实现一个接口**

    例子：

    原始：只为调用单独创建一个child类

    ```
    abstract class Person {
        public abstract void eat();
    }
    class Child extends Person {
        public void eat() {
            System.out.println("eat something");
        }
    }
    public class Demo {
        public static void main(String[] args) {
            Person p = new Child();
            p.eat();
        }
    }
    ```

    继承父类的匿名内部类：

    ```
    abstract class Person {
        public abstract void eat();
    }
    public class Demo {
        public static void 		 main(String[] args) {
            Person p = new Person() {
                public void eat() {
        System.out.println("eat something");
                }
            };
            p.eat();
        }
    }
    ```

    继承接口的匿名内部类

    ```
    interface Person {
        public void eat();
    }
    public class Demo {
        public static void main(String[] args) {
            Person p = new Person() {
                public void eat() {
        System.out.println("eat something");
                }
            };
            p.eat();
        }
    }
    ```

    匿名内部类不能定义任何静态成员、方法。

    匿名内部类中的方法不能是抽象的；

    匿名内部类必须实现接口或抽象父类的所有抽象方法。

    匿名内部类访问的外部类成员变量或成员方法必须用static修饰；

    

19. 非静态内部类中不可以声明静态成员，在静态内部类中可以声明。**静态内部类不可以使用外部类的非静态成员**

    普通内部类在外部保存一个引用，指向创建他的外部类对象，但是若内部类被定义为static，会存在更多的限制：1.如果创建静态内部类的对象，不需要其外部类的对象；2.不能从静态内部类的对象中访问非静态外部类的独享



##### 异常处理

1. Java程序中包括了对异常的处理，异常处理机制。当方法发生错误时，方法会创建一个对象，这个对象就是异常对象，内部机制会把非正常处理代码与主逻辑分离，在编写代码主流程的同事在其他地方处理异常

   

2. 格式：

   ```
   try{
   可能异常的代码块
   }
   catch（ExceptionType1 e){
   对异常1的处理
   }
   catch（ExceptionType2 e){
   对异常2的处理
   }
   ...
   finally{
   无论try如何退出，都会执行finally程序块
   }
   ```

   finally都会被执行，除了以下四种情况：

   1. finally语句内发生异常
   2. 前面加入了System.exit();
   3. 程序所在线程死亡
   4. 关闭CPU

   

3. Java常见异常：详情见百度

   

4. 自定义异常：

   1. 创建自定义异常类

   2. 在方法中通过throw关键字抛出异常对象

   3. 使用try-catch,或者通过throws关键字指明异常（其实就是调用抛出异常的方法后面加上throws + 自定义的异常名，也就是说要在这个方法内抛出我定义的异常）

      ```
      static fun(int asd)throws myException{
      	throw new myException;
      }这是抛出了自定义的异常
      
      static fun(int asd)throws negativeArraySizeException{
      	
      }这是抛出了系统定的异常情况
      ```

      使用throws 关键字讲一场抛给上一级，如果不想处理，还可以继续向上抛出

   4. 在出现异常方法的调用者中捕获并处理异常

5. throw抛出异常

   程序执行到Throw语句时不在继续执行，立即终止。

   ```
   fun1(){
   	throw new MyException;
   }
   main函数（）{
   	try{
   	}catch(MyException e){
   	}	
   }
   ```

6. RuntimeException异常类



##### Swing程序设计

抽象窗体工具箱AWT，awt的升级版就是Swing，Swing保证了跨平台的外观风格统一缺陷

1. 轻量级组件：不依赖于操作系统的组件

   重量级组件：依赖于本地平台的组件

   

2. Swing包的层次结构和继承关系

   

3. Swing组件

   

4. java中的方法划横线的话，意味着这个方法被新版JDK弃用

   

5. 例如使用JButton按钮相关功能，我们需要查询JavaAPI来实现相应功能

   

6. 同一个类中不能重写两次方法

   

7. 像button的事件监听响应方法：

   1. 一个可以直接编写匿名内部类

      ```
      jButton.addActionListener(new ActionListener() {
                  @Override
                  public void actionPerformed(ActionEvent e) {
                      
                  }
              });
      ```

   2. 外部类继承响应事件接口，在外部类中重写响应事件方法，然后在参数中直接加上this

      ```
      public class ABC implements ActionListener{
      	public statoc void main(String[]asd){
      		Jbutton jButton = new JButton;
      	}
      jButton.addActionListener(this)	
      	@Override
          public void actionPerformed(ActionEvent e) {
             jLabel.setText("asda");
          }
      }
      ```








##### java集合

1. Collection集合

   set --HashSet,TreeSet

   list--ArrayList,LinkedList

   

   Map类

   HashMap

   TreeMap

   

2. list集合通常为List类型

   

3. set集合的构造需要一个约束条件，传入的collection对象不能有重复值

   

4. hashmap类似于数组，他的搜索的时间复杂度为O(1)，因为他借助了数组的特性，通过把索引下标与地址建立一个哈希函数，直接得到了下标，方便快捷

   数组加上链表组成，数组是主体，链表是为了解决哈希冲突而存在的。

   实质是存储key-value键值对

   

5. Set集合下我们出现两种集合类：HashSet和treeSet类。这两个类可以用来创建新的集合对象，这些类继承了Set下的方法和成员变量，创建实例

   ```
   TreeSet<> aaa = new TreeSet<>();
   ```

   可以用来创建对象。

   其中尖括号中可以加入我们自定义的类，来使得我们新创建的对象拥有希望的属性值。

   其中TreeSet的 部分方法：

    1. first（）

    2. last()

    3. comparator()

    4. headSet()

    5. subSet()

    6. tailSet()

       

6. 

   MAP集合

   map名称来源于地图，地图上是一个坐标（x，y）对应一个图点一对一关系，map也是如此，一个key对应一个value。

   key值决定了存储对象在映射中的存储位置，但不是由key对象本身决定，是通过一种“散列技术”进行处理，产生了一个散列码的整数值。

   方法：

   1. put(key, value)添加映射关系
   2. contain(key)布尔
   3. get(key)返回值
   4. keySet()返回key对象集合
   5. values()返回值对象集合

   

7. **凡是集合范围内的遍历，都是通过迭代器（Iterator)来实现，Collection接口中的Iterator（）方法可以返回迭代器**

   我们需要通过collection下的对象来构建迭代器，但是map对象不是继承collection，所以遍历map对象的时候，我们需要先创建集合对象，把map对象传进集合对象，再通过集合来创建迭代器进行遍历map，例

   ```
   Set set = map.keySet();
   Iterator it =set.iterator();
   ```

   

8. 我们使用哈希表hashmap的时候，有时候没必要知道他内部实现，只要知道他是map的一种，我们只需要学习map类，学习map方法，哈希map就是map的一种，我们只要知道hashmap的查找功能极其强大，比treemap效率高的多啊。

   因此，我们一般使用hashmap来使用map类，但是如果需要我们的map集合中对象存在一定顺序，我们需要使用treemap类来实现map集合

   

9. hashmap实现map接口，允许空值，必须保证键值对的唯一性，方便快速查找，不能保证顺序恒久不变

10. treemap不仅实现map接口，还实现SortMap接口，因此treemap中的映射关系是按照一定顺序排列的，他不允许空值存在





#### I/O输入输出

1. 我们程序的数据都是暂时，结束后会丢失，基于此，Java创建了I/O技术，永久保存数据，将数据保存在文本文件、二进制文件、ZIP文件中

   

2. 流是一种有序的数据序列，根据操作的类型，分为输入流和输出流，流提供了一条通道程序

   

3. Java中许多泪实现输入/输出，这些类放置在Java.io中，

   1. 所有输入流都是抽象类InputStream（字节输入流）或者抽象类Reader（字符输入流）的子类；
   2. 所有输出流都是抽象类OutputStream（字节输出流）或者Writer（字符输出流）的子类

   

4. IO类的各个深层次结构图

   

5. InputStream字节输入流的相关方法

   1. read（）
   2. read（byte【】b）
   3. Mark（int readlimit）
   4. reset（）
   5. skip（long）
   6. close()

   Java中字符是Unicode编码，双字节，因此，InputStream不适合处理字符，需要使用reader类来进行字符输入流

   

6. OutputStream输出流

   他里面的方法都是返回void

   方法：

   1. write（int b）
   2. write（byte【】 b）
   3. flush（）清空缓存区
   4. close（）关闭输出流

   

7. file类

   他代表了Java.io包中唯一代表磁盘文件的对象。

   创建文件对象：

   1. File file = new File(d:/test.txt)

   2. new File(D:/doc, test.txt)父路径字符串，子路径字符串

   3. new File(File f, String child)父路径对象，子路径字符串

      

8. 对于windoows平台，包含盘符的路径名前缀由驱动器号加一个“：”组成。

   如果路径名是绝对路径名，后面还可能跟“\\\\\"

   例如D盘下创建a.txt

   文件路径名应写成

   **“d:a.txt"或**

   **”d://a.txt"**

   **“D:a.txt"或**

   **”D://a.txt"**

   

9. FileInputStream ,  FileOutStream用来操作磁盘文件

   

10. Java流中读取与写入文件，都不能直接使用reader和writer下的方法实现，需要借助具体的子类的方法实现，如bufferreader、DataOutputStream等

    

11. zip

    ziprntry、zipinputStream、zipoutputStream

    zip创建了一个压缩文件，需要借助方法来不断把 目标文件夹来进行压缩并保存在新建的文件中

    

12. file 指示的是文件的绝对路径

    File.list()返回的是目标文件夹下的所有文件和目录的文件名，返回String变量

    FIle.listFile(返回的是目标文件夹下的所有文件夹的绝对路径，返回file变量
    
    创建单级文件夹：mkdir（）
    
    创建多级文件夹：mkdirs（）
    
    file.isfile():判断是否是一个标准文件（若果改文件不是一个目录，并且满足其他与系统有关的标准，那么改文件是标准文件）
    
    file.exist():判断抽象路径名表示的文件或者目录是否存在
    
    
    
13. 
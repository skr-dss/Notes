---
typora-root-url: ..\img
typora-copy-images-to: upload
---

# Java

## java内存分配

1）栈（stack）：存放的都是方法中的局部变量，方法运行一定要在栈当中
		一旦超出作用域，立刻从栈内存中消失

2）堆（heap）：凡是new出来的东西都在堆中（数组也是对象，也放在堆里）
		堆内存中存放的东西（如成员方法，存的是方法区中方法的地址）都有一个地址值：16进制
		堆内存里面的数据（如成员变量）都有默认值，规则：
			整数	0
			浮点数	0.0
			字符	‘\u0000’
			布尔	false
			引用类型	null

3）方法区（method area）：存储.class相关信息，包含方法信息
4）本地方法栈（native method stack）：与操作系统相关
5）寄存器（pc register）：与CPU相关

## 示例过程

![1](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092100.png)

对象作为参数传递，实际上传递的是对象的地址

![2](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092145.png)

对象作为返回值，返回的还是地址值

![3](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092204.png)

## 注意事项

1.局部变量没有默认值，成员变量有
	局部的变量在栈，成员变量在堆
	局部变量随方法进栈被创建，方法出栈而消失。成员变量随对象创建而产生，随对象被垃圾回收而消失



2.对于boolean, Getter方法要写成isXxx形式

```java
	public class student{
		private String name;
		private boolean male;
		
		public void setName(String str){
			name = str;
		}
		public String  getName(){
			return name;
		}
		public boolean isMale{
			return male;
		}
}
```

3.通过谁调用的方法，谁就是this
4.构造方法名必须和所在类名完全一样，且不能写返回值类型，连void都不要写
5.标准类构造：
	（1）所有成员变量使用private修饰
	（2）每一个成员变量都有Getter/Setter方法
	（3）编写一个无参数构造方法和一个全参数构造方法
6.只有java.lang包下的内容不需要导包，其他的包都需要

# Scanner

```java
import java.util.Scanner;
….
	//Sysetm.in代表从键盘输入
	Scanner scn = new Scanner(System.in);
	//获取键盘的输入的一个数字
	int num = scn.nextInt();
	//获取键盘输入的字符串
	Sring str = scn.next();
```

# Random

```java
import java.util.Random;
….
	Random ra = new Random();
	int num = ra.nextInt();
	//范围左闭右开[0,3)
	int range =ra.nextInt(3);
```

# ArrayList

```java
/*
ArrayList集合长度可以随意变化，但是数组长度是固定的
虽然如此，但是ArrayList底层仍是数组
<>中为泛型，指装在集合中所有元素的类型，只能是引用类型，不能是基本类型
对ArrayList集合来说，直接打印得到的不是地址值，而是内容，如果为空，则打印出来的是[]
*/
ArrayList <String> list = new ArrayList<>();
list.add("hello world"); 
//add方法对ArrayList来说是一定会成功的，因此add()方法返回值一定是true，但其他集合的add()方法不一定成功
boolean success = list.add("hello");
//get方法
String str = list.get(1);
//remove方法
String whoremove = list.remove(1);
//大小
list.size();
```

如果想要存放基本数据类型，需要用到包装类（引用类型）

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| byte         | Byte      |
| int          | Integer   |
| short        | Short     |
| long         | Long      |
| float        | Float     |
| double       | Double    |
| char         | Character |
| boolean      | Boolean   |

# String

```java
/*
string类效果是char[ ]字符数组,但底层原理是byte[ ]字节数组
直接双引号赋值的String就在字符串常量池中
基本类型==进行数值比较
引用类型==进行地址值比较
*/
char[ ] ch = {'a','b','c'};
String str1 = new String(ch);

byte[ ] by = {97,98,99};
String str2 = new String(by);

System.out.println(str1 + "\n" + str2);
```

![4](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092221.png)

String 类型字符串比较,只有内容完全相同才会返回true：
`str1.equals( str2 );`

推荐：`"abc".equals( str1 );`
不推荐：`str1.equals( "abc" );`
原因：当`str1 = null`时，后者会报空指针错误，而前者不会

忽略大小写比较：
`str1.equalsIgnoreCase( str2 );`

```java
//其他方法：
str.length;		//字符串长度
str3 = str1.concat( str2 );		//返回新的连接字符串(原字符串不变)
str.charAt( 3 );		//字符串第3个位置字符
str1.indexOf( str2 );		//str2 在 str1 中首次出现的索引位置，没有返回-1
str1.substring( 3 );	//截取从3到末尾的子串
str1.substring( 3,5 );		//截取[ 3,5 )的子串
char[ ] str_char = str.toCharArray();	//转换为字符数组类型
byte[] str_bytes =str.getBytes();	//转换为字节数组类型

String str = "hello hello world world";		
String str2 = str.replace("hello", "fuck");		//替换字符串

String str = "aaa,bbb,ccc";	
String[] str2 = str.split(",") ;////按参数分割字符串，返回字符串数组，参数实际上是正则表达式，对于“.”分割的字符，参数为“\\.”

```

# Static

static修饰成员变量，那么这个成员变量就不属于对象，而属于类

static修饰成员方法，那么这个成员方法也不属于对象，而属于类

被static修饰的，不论是成员变量还是成员方法，都推荐使用类名调用，这样区分更清楚

注意：1.静态不能访问非静态，原因是内存中先有静态，后有非静态，先人不知道后人，后人知道先人

​			2.静态方法中不能使用this,原因是this代表当前对象，通过谁调用的方法，谁就是当前对象。

![5](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092236.png)

静态代码块

特点：当第一次用到本类时，静态代码块执行唯一的一次

​			静态内容总是优先于非静态内容，因此静态代码块比构造方法先执行

典型用途：

​			用来一次性对静态成员变量赋值

```java
public class 类名{
	static {
		...
	}
}
```

# Arryays

```java
import java.util 
...
int[ ] arr ={1,2,3}
String str = Arrays.toString( arr );	//数组转字符串
Arrays.sort( arr );		//默认升序排列(必须是数组才能使用4)

```

# Math

```java
import java.lang
   ...
	public static double abs(double num);//绝对值
	public static double ceil(double num);//向上取整
	public static double floor(double num);//向下取整
	public static long round(double num);//四舍五入
	//Math.PI代表近似的圆周率常量（double）
```

# 继承

java是单继承的，一个类的直接父类只有一个

成员方法看右侧，new的是谁就是谁的成员方法

在父子类的继承关系中，如果成员变量重名，则创建子类对象时，访问有两种方式：

​	直接通过子类对象访问，等号左边是谁就优先用谁，没有就向上找

​	间接通过成员方法访问，方法属于谁就优先用谁，没有就向上找

```java
public class Fu {
    int num = 30;
    private String name = "马云";
    void show() {
        System.out.println(num);
    }
}

public class Zi extends Fu{
    int num = 20;
}

public class Main {

    public static void main(String[] args) {
        Zi test = new Zi();
        test.show();//输出为Fu的num,30
    }
}
```

```java
public class Zi extends Fu{
    int num = 10;
    public void method(){
        int num = 20;
        sout(num);	//20,局部变量
        sout(this.num);	//10,本类的成员变量
        sout(super.num);	//30，父类的成员变量
    }
}
```

**方法的重写 **

​	重写（override）:方法名称一样，参数列表也一样

​	重载（overload）:方法名称一样，参数列表不一样

​	大部分重写都保持和父类权限和返回值相同

注意：对已经投入使用的类，一般不要修改，推荐定义一个新的类来重复利用共性内容并添加新内容

**继承关系中父子类构造方法的访问特点：**

​	1.子类构造方法中默认隐含`super()`调用，所以一定是先调用父类构造，后执行子类构造

​	2.子类构造可以通过super关键字来调用父类重载构造

​	3.super的父类构造调用，必须是子类构造方法的第一个语句，，一个子类不能多次调用super构造

​	总结：子类必须调用父类构造方法，不写则赠送super(),写了则使用写的，super只能有一个，且必须是第一个

**super关键字用法**

​	1.子类成员方法中访问父类成员变量

​	2.子类成员方法中访问父类成员方法

​	3.子类构造方法中访问父类构造方法

**内存图**

注意：子类method中有一个super.method(); 图中没有写出来

![6](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092256.png)

# 抽象方法

抽象方法所在的类必须是抽象类

```java
public  abstract  class  Animal{
	public  abstract  void  eat();
}
```

注意：

​	1.不能直接`new`抽象类对象

​	2.必须用一个子类来继承抽象父类

​	3.子类必须覆盖重写抽象父类所有抽象方法

​	4.创建子类对象进行使用

​	5.抽象类不一定有抽象方法，有抽象方法的一定是抽象类，因为有些情况不想让调用者创建该类对象

​	6.抽象类可以有构造方法，是供子类创建对象，初始化父类成员使用的

# 接口

接口的实现类一般使用Impl结尾作为类名（implement）；

接口中所有方法都是抽象方法，不写abstract也是抽象方法，因为接口会默认自动加上abstract；

接口就是多个类的公共规范

接口是一种引用数据类型，最重要的就是其中的抽象方法

接口编译生成的字节码文件仍是:	.java --> .class

接口中可包含的内容：

​	java7：常量，抽象方法

​	java8：默认方法，静态方法

​	java 9：私有方法

使用步骤：

​	1.接口不能直接使用，必须有一个实现类来实现该接口

​	`public class 实现类名称 implements 接口名称{	}`

​	2.实现类必须覆盖重写接口中所有的抽象方法

​	3.创建实现类对象进行使用

注意：如果实现类没有重写接口中所有抽象方法，那么这个实现类自己必须是抽象类

**默认方法：**
	`public default 方法名（参数列表）{
		方法体
	}`
作用：实现接口的类的对象可以直接调用接口的默认方法（向上找原则）,解决接口升级问题

举例：当一个接口被很多类实现了以后，需要添加新的方法对接口升级，但是直接添加的话会使所有实现这个接口的类报错，因为它们都没有override这个新方法。因此需要默认方法来解决。

**静态方法：**
	public static 方法名（参数列表）{
		方法体
	}
注意：不能通过接口实现类调用静态方法，要通过接口名直接调用；

**私有方法：**
为了解决方法之间重复代码，可以额外写一个方法包含重复代码，供其他方法直接调用。但是这样new出来的对象也能直接使用这个额外方法，因此需要私有方法保证不被外部访问到

​	普通私有方法：
​		private 返回值类型 方法名（参数列表）{
​			方法体
​		}
​	作用：解决多个默认方法之间代码重复的问题

​	静态私有方法：
​		private static 返回值类型 方法名（参数列表）{
​			方法体
​		}
​	作用：解决多个静态方法之间代码重复的问题

**成员变量**

​	必须使用`public static final`修饰，从效果看，其实就是接口的常量

​	注意：

​	1.一旦使用final修饰，说明不可变

​	2.接口中的常量可以省略`public static final`

​	3.接口中的常量必须赋值

​	4.接口中的常量名称，推荐使用完全大写，用下划线分隔

**使用接口时要注意**

 * 接口没有静态代码块或构造方法

 * 一个类只能继承一个父类，但是可以同时实现多个接口

 * 如果实现类实现的多个接口中有重复的**抽象方法**，只需重写一次

 * 如果实现类没有重写所有抽象方法，那么实现类必须是抽象类

 * 如果实现类实现的多个接口中有重复的**默认方法**，那么实现类一定要对冲突的默认方法进行覆盖重写，并且带着**default**关键字

 * 实现类的直接父类方法和接口中的默认方法冲突，优先使用父类的方法

   

# 多态

代码中体现多态性，就是一句话，父类引用指向子类对象 

格式：

​		父类名称 对象名 = new 子类名称（）；

​		接口名称 对象名 = new 实现类名称（）；

在多态中成员方法的访问规则：

​	new的是谁，就优先用谁，没有则向上找

​	口诀：编译看左边，运行看右边

成员变量：编译看左边，运行还看左边

成员方法：编译看左边，运行看右边

```java
public class Fu {
    int num = 30;
    private String name = "马云";
    void show() {
        System.out.println(num);
    }
}

public class Zi extends Fu{
    int num = 20;
    public void show(){
        System.out.println(num);
    }
}

public class Main {

    public static void main(String[] args) {
        Fu test = new Zi();
        sout(test.num);//编译看左边，运行还看左边。输出为Fu的num,30
        test.show();//编译看左边，运行看右边。调用的是Zi的show()方法
        
    }
}
```

**多态的好处**

```java
//不用多态
Teacher one = new Teacher();
one.work();//讲课
Assistant two = new Assistant();
two.work();//辅导

//使用多态
Employee one = new Teacher();
one.work();//讲课
Employee two = new Teacher();
two.work();//讲课
```

传参时不需要判断是哪种类型了，直接使用多态调用方法就行了

```java
public void method(Employee e){
    e.work();
}
```



**多态的弊端**

​	无法使用子类特有的内容

​	解决方法：向下转型

# 向上转型与向下转型

* 对象的向上转型，其实就是多态

  父类名称 对象名 = new 子类名称（）；

  向上转型一定是安全的

* 对象的向下转型，其实是一个还原动作

  子类名称 对象名 = （子类名称） 父类对象；

  注意：必须保证对象本来创建的就是猫，才能向下转型为猫

* 如何知道一个父类的引用对象本来是什么子类？

  对象 instanceof 类名称

  会得到一个boolean值结果

# final修饰符

* final修饰类：此类不能被子类继承（太监类）
* final修饰方法：此方法不能被覆盖重写
* 对于类和方法来说，final和abstract不能同时使用，因为final不能重写，而abstract必须重写，矛盾

* final修饰局部变量：

  ​	基本类型：只能唯一一次赋值，数据不可变

  ​	引用类型：地址值不可变，地址存放的内容可变

* final修饰成员变量：

  ​	成员变量有默认值，用了final修饰后不再拥有默认值，必须手动赋值

  ​	要么直接赋值，要么构造方法赋值

* 必须保证类当中所有重载的构造方法都最终会对final的成员变量赋值

# 四种权限修饰符

|              | public | protected | (default) | private |
| ------------ | ------ | --------- | --------- | ------- |
| 同一个类     | YES    | YES       | YES       | YES     |
| 同一个包     | YES    | YES       | YES       | NO      |
| 不同包子类   | YES    | YES       | NO        | NO      |
| 不同包非子类 | YES    | NO        | NO        | NO      |

注意：default不是关键字“default”,而是根本不写

# 内部类

* 成员内部类

  ```java
  public class Outter {
      int num = 10;
      public class Inner{
          int num = 20;
          public void methodInner(){
              int num = 30;
              System.out.println(num);//30
              System.out.println(this.num);//20
              System.out.println(Outter.this.num);//10
          }
      }
  }
  ```

* 局部内部类

  ​	方法中定义的内部类，出了此方法都不能用

  ​	如果希望访问所在方法的局部变量，那么这个局部变量必须是**有效final的**（从java8开始可以省略）

  ​	原因：

  ​		1.new出来的对象在堆内存中

  ​		2.局部变量跟着方法走，在栈内存中

  ​		3.方法运行结束后，立刻出栈，局部变量立刻消失

  ​		4.但是new出来的对象会在堆当中持续存在，知道垃圾回收消失

  ​		注：当方法执行结束后，局部变量从栈中消失，方法中new的对象使用的是局部变量的copy，在常量池中，但要保证局部变量不能变来变去

  

  * 匿名内部类
    	如果接口的实现类只需要实现唯一的一次，那么可以省略该接口的实现类的定义，而使用匿名内部类
    	格式：
    		接口名称 对象名 = new 接口名称（）{
    			覆盖重写所有抽象方法
    		}；
    	形态1（匿名内部类）：
    		`MyInterface objA = new MyInterface() {method1(){} };
    		objA.method1;`
    	形态2（既是匿名内部类，也是匿名对象）：
    		`new MyInterface(){ method1(){} }.method1();`
    匿名内部类省略了类/子类名称，匿名对象省略了对象名称，二者不是一回事！！！

  ​	

* 修饰符：

  ​	外部类：public / default

  ​	成员内部类：public / protected / default / private

  ​	局部内部类：什么都不能写

# 接口作为成员变量

```java
class A{
	private MyInterface obj;
	public void setMyInterface(MyInterface in){//对象调用此方法时传入的是接口的实现类
		obj = in;
	}
}
```



# object类中的方法

object类是所有类的根类，因此所有类都能使用object类的方法

* toString方法

  `String str="hellworld";
  System.out.println(str);`
  输出语句默认调用的就是toString方法

  默认输出的是地址值，没有意义，最好重写输出属性值

  看一个类是否重写了toString方法，直接打印这个类对应的对象名字即可

* equals方法

  默认比较的是地址值，没有意义，最好重写比较属性值
  `Person p1 = new Person("张三")；
  Person p2 = new Person("李四")；
  boolean res = p1.equals(p2);`
  注：引用类型比较的是地址值
  怎样重写equals()方法？
  **alt+insert**
  重写equals方法的要求：
  1、自反性：对于任何非空引用x，x.equals(x)应该返回true。
  2、对称性：对于任何引用x和y，如果x.equals(y)返回true，那么y.equals(x)也应该返回true。
  3、传递性：对于任何引用x、y和z，如果x.equals(y)返回true，y.equals(z)返回true，那么x.equals(z)也应该返回true。
  4、一致性：如果x和y引用的对象没有发生变化，那么反复调用x.equals(y)应该返回同样的结果。
  5、非空性：对于任意非空引用x，x.equals(null)应该返回false

  **Object.equals(p1,p2)   //可以防止 p1.equals(p2) 可能的空指针异常**

# Date类

* Date 类表示特定瞬间，精确到毫秒
  如：2021-7-7 15:28:33:333
  时间原点：1970年1月1日 00:00:00（英国格林威治）
  中国属于东八区，需要加8个小时
  时间原点：1970年1月1日 08:00:00（中国）

```java
System.out.println(System.currentTimeMillis());//时间原点到当前时间的毫秒间隔1625124936537
System.out.println(new Date().getTime());//等同于System.currentTimeMillis()
System.out.println(new Date());//返回当前日期Thu Jul 01 15:35:36 CST2021
/*
Date带参构造方法：
Date(longdate):传递毫秒值，将毫秒值转化为Date日期
*/
System.out.println(new Date(1625124936537L));//Thu Jul 01 15:35:36 CST2021
```

* DateFormat类
  DateFormat是一个抽象类
  作用：格式化 日期->文本 
  	  解析    文本->日期
  	  标准化
  使用时需要使用其实现子类：SimpleDateFormat

  1.format()方法：日期->文本

  ```java
  /*
  y年 M月 d日 H时 m分 s秒
  步骤：
  	1.创建SimpleDateFormat对象
  	2.调用方法
  */
  Date date =newDate();
  SimpleDateFormat sd f= newSimpleDateFormat("yyyy-MM-ddHH:mm:ss");
  String s = sdf.format(date);
  System.out.println(s);//2021-07-0116:30:36
  System.out.println(date);//ThuJul0116:30:36CST2021
  ```

  2.parse()方法：文本->日期

  ```java
  /*
  使用步骤：
  	1.创建SimpleDateFormat对象，构造方法中传递指定的模式
  	2.调用SimpleDateFormat对象中的parse(),把符合构造方法中模式的字符串，解析为Date日期
  注意：
  	public Date parse(String source) throws ParseExcetion
  	parse方法声明了一个异常叫ParseException解析异常
  	如果字符串和构造方法中的模式不一样，那么程序就会抛出此异常
  	调用一个抛出了异常的方法，就必须处理这个异常，要么throws继续上给你们抛出这个异常，要么try...catch自己		处理这个异常
  	处理方法：alt+回车
  */
  
  public class Date1 {
      public static void main(String[] args) throws ParseException {
          Scanner sc = new Scanner(System.in);
          System.out.println("请输入出生日期,格式为yyyy-MM-dd");
          String str = sc.next();
          SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
          Date bir = format.parse(str);
          long time = bir.getTime();
          long now = new Date().getTime();
          long days = (now - time)/1000/60/60/24;
          System.out.println("出生天数为："+days);
      }
  }
  ```

# Calender类

* 抽象类
  `java.util.Calendar`
* 不用子类创建对象，使用`getInstance( )`方法，返回calender类的子类对象
  `Canlender now = Calender.gerInstance( ); `  //多态

```java
Calendar now = Calendar.getInstance();
System.out.println(now);

//get()方法返回给定日历字段的值
int year=now.get(Calendar.YEAR);
int month=now.get(Calendar.MONTH);
int day=now.get(Calendar.DATE);
int days=now.get(Calendar.DAY_OF_MONTH);//和Calendar.DATE效果相同
System.out.println(year+"\t"+month+"\t"+days);

//set()单独设置将给定的日历字段设置为给定值
now.set(Calendar.YEAR,1995);
now.set(Calendar.MONTH,12);
now.set(Calendar.DATE,6);
//set()同时设置
now.set(1995,12,6);

//add()方法为给定的日历字段添加给定的时间量
now.add(Calendar.YEAR,2);//正数增加
now.add(Calendar.YEAR,-2);//负数减少

//getTime()方法把日历对象转换为日期对象
Calendarc=Calendar.getInstance();
Datedate=now.getTime();
System.out.println(date);
```

# System类

1.与Date.getTime()作用相同，返回1970.1.1 8:00:00至今的毫秒值

```java
System.currentTimeMillis()
```



2.

```java
/*
arraycopy(Object src , int srcPos , Object dest , int destPos , int length )
原数组，原数组索引起始位置，目标数组，目标数组索引起始位置，复制元素个数
会覆盖目标数组的原数值

静态方法 可通过类名System直接调用
*/
int[] src={1,2,3,4,5,6};
int[] dest={7,8,9,10,11,12};

System.out.println(Arrays.toString(dest));
System.arraycopy(src,0,dest,0,3);
System.out.println(Arrays.toString(dest));

```

# StringBuilder类

* String类
  	底层是一个被final修饰的字符数组
  	`private final byte[] value`
  	字符串是常量，在创建后不能更改
  	字符串相加会有多个字符串，占用空间多：
  	`String str = "a" + "b" + "c"`
  		此时内存中有 ：`"a" , "b", "c", "ab", "abc"`共5个字符串
* StringBulider类
  	底层是一个没有被final修饰的字符数组（初始大小为16）
  	`byte[ ] value = new byte[16]
  	String str = "a" + "b" + "c"`
  		此时内存只有一个字符串“abc”
  	StringBuilder在内存中始终是一个数组，占用空间少，效率高，会自动扩容
  	
  	

* 构造方法：
  （1）空参数，构造一个空的StringBulider容器
  	`StringBulider sbl = new StringBulider( )`
  （2）带参数，构造一个StringBulider容器，并将字符串添加进去
  	`StringBulider sbl = new StringBulider( “hello”)`

* 常用方法：
  `bu1.append("world");`

  ​	append（）方法返回值是当前对象自身，即会改变字符串本身

  `bu1.reverse( ) `反转内容

  

  string->stringbulider

  ​	`StringBulider(String str)`

  ​	`stringbulider->string`

  bu1.toString( ) 将缓冲区内容转为字符串

# 包装类

* 作用

  ​	基本数据类型没有对应方法来操作这些数据，因此我们可以使用一个类，把基本数据类型的数据包装起来，这个类

  叫做包装类，在包装类中可以定义一些方法来操作基本数据类型

* 装箱
  	（1）构造方法（过时了）：

  ​		` Integer in = new Integer( 1 );`

  ​		`Integer in2 = new Integer( "1" ); `//字符串也行，但是字符串表示的必须是基本数据类型

  ​	（2）静态方法：

  ​		`Integer in = Integer.valueOf( 1 );`

  ​		`Integer in2 = Integer.valueOf( “1” );`

* 拆箱
  `int in = int2.intValue( );`

* 自动装箱（JDK1.5以后）
  `Integer in = 1;  `//相当于Integer in = new Integer(1);
  * 自动拆箱
    	`in = in +2;`
    	（1）int + 2  相当于 `in.intValue( ) + 2 =3`
    	（2）in = in + 2 相当于` in = new Integer( 3 );`
    	应用：
    	`ArrayList<Integer> list = new ArrayList( );`
    	`list.add( 1 );  `//自动装箱 `list.add(new Integer( 1 ) )`
    	`list.get( 0 );  ` //自动拆箱`list.get( 0 ).intValue( );`

# 基本数据类型与字符串之间的转换

* 基本数据类型（4类8种）
  	`byte short int long float double char boolean`
  	转字符串三种方式：
  	（1）123+“”;
  	（2）先转为包装类，再使用toSring( )方法
  			Integer i = 123；
  			String s = i.toString( );
  	（3）String.valueOf( );
* 字符串转基本数据类型
  	利用包装类的pase方法：
  	`int i = Integer.paseInt("100")
  	float j = Float.paseFloat("100")`
  	
  注意：Character类没有对应的pase方法，只有另外7类有

# Collection集合

| boolean add(E e)      | 向集合中添加元素         |
| --------------------- | ------------------------ |
| boolean remove(E e)   | 删除集合中某个元素       |
| void clear( );        | 清空所有元素             |
| boolean contains(E e) | 判断集合是否包含某个元素 |
| boolean isEmpty( )    | 判断是否为空             |
| int size()            | 获取集合长度             |
| Object[ ] toArray( )  | 将集合转为数组           |

```java
Collection<String> col = new HashSet<>();
col.add("hello");
col.add("world");
String[] str = newString[2];
col.toArray(str);
或
Collection<String> col = new HashSet<>();
col.add("hello");
col.add("world");
Object[ ] str = col.toArray( );
```

# Iterator迭代器

java.util.Iterator

Iterator迭代器是一个接口，无法直接使用，需要使用该接口的实现类对象

* 迭代器是通用的取出集合中元素的方法，不管有没有索引
  	（1）判断有没有元素
  	（2）有则取出元素，回到（1）
  	（3）没有就结束
  这种取出方式专业术语为迭代

* 方法

  | boolean | hasNext() | 如果仍有元素可以迭代，返回true                               |
  | ------- | --------- | ------------------------------------------------------------ |
  |         | next()    | 返回迭代的下一个元素，即取出元素，也将指针后移一位           |
  | void    | remove()  | 从迭代器指向的collection中移除迭代器返回的最后一个元素（不常用） |

* 使用

  实现方法：

  ​	Collection接口中有个iterator( )方法，返回的就是在当前collection元素上迭代的迭代器

  ​	迭代器的使用步骤：

  ​		（1）使用集合中的iterator( )方法获取迭代器的实现类对象，使用Iterator接口接收（多态，注意Iterator也是有泛

  型的，与集合泛型一致）
  		（2）使用Iterator接口中的方法hasNext（）判断还有没有下一个元素

  ​		（3）使用Iterator接口中的方法next( )取出集合下一个元素

  ```java
  	Collection<String> col = new ArrayDeque<>();
  	col.add("hello");
  	col.add("world");
  	Iterator<String> ite = col.iterator();
  	while(ite.hasNext()){
  		System.out.println(ite.next());
  }
  ```

  

* 原理

  ![7](https://raw.githubusercontent.com/skrdss/Notes/master/img/20210720092324.png)

#  增强for循环

也叫for each循环，是JDK1.5以后专门用来遍历**数组和集合**的，内部实现原理是个`Iterator`迭代器，所以在遍历过程中不能对集合中的元素进行增删操作

```java
Collection<String> col = new ArrayDeque<>();
col.add("hello");
col.add("world");
for(String i : col){
    System.out.println(i);
}
```

# 泛型

泛型是一种未知的数据类型，当我们不知道用什么数据类型的时候，可以使用泛型

E	e	:	Element	元素

T	t	:	Type	类型

## 泛型类



```java
public class ArrayList<E>{
    public boolean add(E e){}
    public E get(int index){}
}
```

不写泛型的话默认object类型，可以储存任意类型数据

缺点：不安全，会引发异常

```java
//使用泛型

ArrayList<String> list = new ArrayList<>();
//不使用泛型
ArrayList list = new ArrayList();
```

```java
//定义含有泛型的类
public class Genetic<E> {
    private E name;

    public void setName(E name) {
        this.name = name;
    }

    public E getName() {
        return name;
    }
}
```

```java
//定义含有泛型的方法
public <E> void method01(E e){}
public static <M> void method02(M m){}
```

## 泛型接口

```java
/*定义含有泛型的接口
	第一种：接口实现类确定泛型
	第二种：接口实现类依然使用接口泛型，创建对象时确定泛型
*/
	
public interface Generic<E>{
    public abstract void method(E e);
}
//第一种
public class  GenericImpl01 implements Generic<String>{
    @Override
    public void method(String s){
        sout(s);
    }
}

GenericImpl01 gi1 = new GenericImpl01();
//第二种
public class GenericImpl02<E> implements Generic<E>{
    @Override
    public void method(String s){
        sout(s);
    }
}
GenericImpl02<String> gi2 = new GenericImpl02<>();
GeneticImpl03<Integer> gi3 = new GenericImpl03<>();
```



## 泛型通配符

<?>	:	代表任意的数据类型

使用方法：

​				不能创建对象使用

​				只能作为方法参数使用

注意：泛型没有继承概念，如果形参为Object泛型，那么实参必须为Object泛型

```java
//例：输出所有ArrayList集合中的数据
    public static void pr(ArrayList<?> list){	//此处若为Object泛型，则传入的必须都为Object泛型，因此说泛型没有继承概念
        Iterator<?> it = list.iterator();
        while(it.hasNext()){
            System.out.println(it.next());
        }
    }

    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        ArrayList<String> list1 = new ArrayList();
        list1.add("hello");
        list1.add("world");
        ArrayList<Integer> list2 = new ArrayList<>();
        list2.add(1);
        list2.add(2);
        ArrayList<Boolean> list3 = new ArrayList<>();
        list3.add(true);
        list3.add(false);

        pr(list1);
        pr(list2);
        pr(list3);
    }
```

## 通配符高级使用——受限通配符

（看懂就行，平时用不到）

* 泛型的上限：

  <? extends E>	:	使用的泛型只能是E类型自身或其子类

  <? super E>		:	使用泛型只能是E类型自身或其父类

# 扑克牌案例

```java
//有牌，洗牌，发牌，看牌

public class pokers {
    public static void main(String[] args) {
        ArrayList<String> poker = new ArrayList<>();
        String[] shapes = {"♠","♥","♣","♦"};
        String[] numbers = {"2","A","K","Q","J","10","9","8","7","6","5","4","3"};

        poker.add("king");
        poker.add("queen");

        ArrayList<String> p1 = new ArrayList<>();
        ArrayList<String> p2 = new ArrayList<>();
        ArrayList<String> p3 = new ArrayList<>();
        ArrayList<String> di = new ArrayList<>();

        for (String shape : shapes) {
            for (String number : numbers) {
                poker.add(shape+number);
            }
        }

        Collections.shuffle(poker);//Collections的静态方法，随机打乱顺序
        int index = 0;
        for (String s : poker) {
            if(index>50){
                di.add(s);
            }
            if(index%3 == 0){
                p1.add(s);
            }else if(index % 3 == 1){
                p2.add(s);
            }else if(index % 3 == 2){
                p3.add(s);
            }
            index++;
        }
        System.out.println(p1);
        System.out.println(p2);
        System.out.println(p3);
        System.out.println(di);

    }
}
```

# List集合

特点：

​		1.有序

​		2.允许重复元素

​		3.有索引

特有方法：

```java
public void add(int index,E element);//指定位置添加元素
public E get(int index);//获取指定位置元素
public E remove(int index);//删除指定位置元素
public E set(int index,E element);//替换指定位置元素

```

注意：操作索引时一定要注意索引越界

## ArrayList

List的数组实现，底层为数组，查询快，增删慢

不是同步的，即是多线程的，速度快

遍历方法：

​	1.普通```for```循环，注意获取第```i```个元素是```get(i)```而不是```array[i]```

​	2.迭代器

​	3.增强```for```循环

## LinkedList

List的链表实现，底层为双向链表，查询慢，增删快

不是同步的，即是多线程的，速度快

```java
/*
特有的一些方法
*/
public void addFirst(E e);//插入开头
public void addLast(E e);//插入结尾
public void push(E e);//插入开头

public E getFirst();
public E getLast();

public E removeFirst();
public E removeLast();
public E pop();//相当于removeFirst

public boolean isEmpty();
```

## Vector

List的数组实现，底层为数组

是同步的，即是单线程的，速度慢

# Set集合

不包含重复元素

没有索引，不能使用普通`for`循环遍历

## HashSet

底层为哈希表，查询速度非常快

无序，无索引，不允许存储重复元素

### 哈希表

`java.lang`包中的`Object`类中方法：`int hashCode()`返回对象的哈希码值

哈希表 = 数组 + 链表 / 红黑树（当相同哈希值后的链表超过8位，就会把链表转为红黑树，用来提高查询速度）

Set集合在调用`add（）`方法时，会调用元素的`hashCode()`方法和`epuals()`方法判断元素是否重复


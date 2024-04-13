**堆和栈：**[(2条消息) 【面试专栏】第一篇：Java基础：基础概念、四大引用、常考方法解析、面向对象、枚举、序列化、异常体系_木秀林的博客-CSDN博客](https://blog.csdn.net/qq_35530042/article/details/125729230)

常量池：string有两种创建方式，一个是直接创建 string s1=“abc” 字符串常量池会将对象的引用进行保存，后面如果创建重复的字符串，就会直接将字符串常量池里的引用返回，第二种是new

[int和Integer的区别-java教程-PHP中文网](https://www.php.cn/java-article-442893.html#:~:text=int和Integer的区别 1、Integer是int的包装类，int则是java的一种基本数据类型,2、Integer变量必须实例化后才能使用，而int变量不需要 3、Integer实际是对象的引用，当new一个Integer时，实际上是生成一个指针指向此对象；而int则是直接存储数据值)

数据转换：

在运算过程中，由于不同的数据类型会转换成同一种数据类型，所以整型、浮点型以及字符型都可以参与混合运算。自动转换的规则是从低级类型数据转换成高级类型数据。转换规则如下：

- 数值型数据的转换：byte→short→int→long→float→double。

- Java int类型默认是不能转换为Boolean类型的

- 在byte型（- 128~127）数值的范围内，所以输出没有问题，同理可得，只要被赋的值在小精度数据类型的范围内，输出结果都是没有问题

  如 byte a = 100 √
      byte b  = 300 ×

- 字符型转换为整型：char→int。



局部变量：在方法中，在for/while语句，

在类中定义若是直接定义为非静态全局变量，只能在对象的函数使用
					若是加上static为静态全局变量，可以在main（main是静态的）可以调用，

![image-20220927173733998](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220927173733998.png)

Random 包：import java.util.Random

Random r = new Random();

r.nextInt(5) ------[0,5)
r.nextInt(5)+1 -------[1,6)

常量：static final 类型 类型名（大写）;

枚举类型：public enum 类名{

元素1，元素2；

}

调用枚举：类名.元素n

**遍历**

**vector遍历**

```java
public class VectorTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
	Vector v=new Vector();
	v.add(123);
	v.add("I love you");
	v.add(Integer.valueOf(234));
	v.add(23.5);
	//for
	for(int i=0;i<v.size();i++) {
		System.out.println(v.get(i));
	}
	//for-in
	for(Object s:v) {
		System.out.println(s);
	}
```

**arraylist遍历**

JAVA+ MACHINE TASK(DT,KNN)



# 1.基础

## 1.1Java特性：跨平台性

##  ------JDK（含JVM JAVA虚拟机可以在不同系统下都可以运行.class 文件）

​	JDK=JRE+JAVA开发工具
​	JRE=JVM+核心类库

## 1.2入门细节：

**入口：public static void main(String[] args){**

**}**

**每个程序只允许一个public class，但可以有多个class**

**如果源文件包含一个public类,则文件名必须按该类名命名**

crtl+shift+d  复制粘贴一体化

## 1.3文档注释

生成一个javadoc帮助文档

/**
*......
*......
*/

## 1.4小数误区

![image-20220803201635856](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220803201635856.png)![image-20220803201639547](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220803201639547.png)![image-20220803201640920](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220803201640920.png)

## 1.5boolean

boolean相当于c++里的bool

## 1.6其他数据类型相互转化String类型

//其他数据类型转String类型     String s1=‘a’+“”；(字符串拼接)

**基本数据型态转换成String:**

**String.valueOf(其他数据类型 b)**

//String类型转其他数据类型  ->使用基本数据类型的包装类（Integer，Float，Double···）

   int num = Interger.parseInt("123");

  double a = Double.parseDouble("123");

## 1.7输入

![image-20220804203558812](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220804203558812.png)

1.引入/导入scanner类所在的包
2.创建Scanner对象
3.接收用户输入（通过使用相应的方法）

char ------next().charAt(0)
string-------next()
int --------nextInt()
double ------nextDouble

```java
import java.util.Scanner;
	public class Input{
        public static void main(String [] args){//以下内容都需写在main函数里
        Scanner myscanner = new Scanner(System.in);//设置对象
        String s = myscanner.next();
	    int a =	myscanner.nextInt();
	    System.out.println(s+"s"+a);
        }
    }
```

1.8输出

1.参数有区别：

System.out.println() 可以不写参数

System.out.print(参数) 参数不能为空.必须有

2.效果有区别

println :会在输出完信息后进行换行,产生一个新行

print: 不会产生新行

3.println更简洁, print更灵活

print可以后面跟"\n"来达到和println一样的效果

也可以跟"\t" 制表符, 等.



## 1.8获取String里的某一位字符

用String的函数 ：s.charAt(0) 第0个字符

```java
import java.util.Scanner;
	public class Input{
        public static void main(String [] args){//以下内容都需写在main函数里
        Scanner myscanner = new Scanner(System.in);//设置对象
        String s = myscanner.next();
         char a = s.charAt(0);
	    System.out.println(a);
        }
    }
```

## 1.9创建数组

```
int s[] = new int [n];
```

![image-20220811195209296](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220811195209296.png)

## 1.10static

static 创建的变量，所有类都可以访问，属于类，但不属于类的对象，在类加载时候就生成，并且只初始化一次

调用变量时，可以直接类.变量
调用方法时，可以直接类.方法（并且static方法只能访问static变量）

同成员变量，成员方法也可以分为以下两种：

静态方法（或称为类方法），指被 static 修饰的成员方法。
实例方法，指没有被 static 修饰的成员方法。
静态方法与实例方法的区别：

静态方法，属于类，而不属于类的对象。
1）它通过类直接被调用，无需创建类的对象。
2）静态方法中，不能使用 this 关键字，也不能直接访问所属类的实例变量和实例方法；
3）静态方法中，可以直接访问所属类的静态变量和静态方法。
4）同this 关键字，super 关键字也与类的实例相关，静态方法中不能使用 super 关键字。
实例方法，可直接访问所属类的静态变量、静态方法、实例变量和实例方法。

```
静态全局变量与非静态全局变量的区别：
非静态全局变量的作用域是整个源程序， 当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。

静态全局变量则限制了其作用域， 即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用， 因此可以避免在其它源文件中引起错误。

从以上分析可以看出， 把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。
```



# 2.类与对象

2.1创建对象

Cat cat1 = new Cat1（）;

**Child c = new Child()形式的c引用的话，在栈区定义Child类型引用变量c，然后将堆区对象的地址赋值给它**

![image-20220820102637818](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220820102637818.png)![image-20220820102639236](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220820102639236.png)

**内存分配： **[0198_韩顺平Java_对象分配机制_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fh411y7R8?p=199&spm_id_from=pageDriver&vd_source=1f4583ef56826b10e90ed107f460bd8b)![image-20220821094901346](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220821094901346.png)

引用数据类型包括对象、数组

# 3.方法重载

方法重载细节：
（1）方法名相同
（2）形参列表必须不同
（3）返回类型：无要求

**int... 可变参数**

可传入0到n个数字（函数参数自动匹配传入的个数），并且可变参数的实参可以为数组

实质：可变参数实际为数组

例子：求和

```java
import java.util.Scanner;
public class bubble{
	public static void main(String [] args){
		person p = new person();
		int s[]={1,4,5};
        System.out.println(p.m(12,13));
        System.out.println(p.m(s));//可传入数组
	}
}
class person{
	public int m(int... s){//s实际上为数组
		int sum = 0;
		for(int i = 0; i < s.length; i++){
			sum+=s[i];
		}
		return sum;
	}
}
```

此外，可变参数可以跟普通参数一起，只是可变参数要放在最后

局部变量不能加修饰符（public，private，protected）

# 4.类与构造器

```java
import java.util.Scanner;
public class bubble{
	public static void main(String [] args){
		person p1 = new person();
		System.out.println(p1.age+p1.name);
		person p2 = new person(11,"abc");
		System.out.println(p2.age+p2.name);
	}
}
class person{
	int age = 10;//全局变量name默认值为10
	String name="a";//全局变量name默认值为a
	person()
	{
		;
	}
	person(int oage,String oname){
		age = oage;
		name = oname;
	}
}
```

![image-20220906214039241](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220906214039241.png)

this的特殊用法：用一个构造器访问另一个构造器：**this（参数列表）且必须放在第一条语句**

```java
import java.util.Scanner;
public class bubble{
	public static void main(String [] args){
		person p1 = new person();

	}
}
class person{
	int age = 10;
	String name="a";
	person()
	{
		this(12,"aaaws");
		System.out.println("shuchu");
			}
	person(int age,String name){
		this.age = age;
		this.name = name;
		System.out.println(age + name);
	}
}
```

# 5.IDEA

自动生成构造器 fn+alt+ins

删除改行 crtl + d

查看类的层级关系：crtl + H

将光标放在一个方法上，输入crtl + B 可以定位到方法上

自动分配变量名：new person（）.var![image-20220907233400180](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220907233400180.png)

自动补全（已经有这个单词出现过了）：alt + /

**快速模板：**

sout -----System.out

fori-------for(int i = 0; i <  ; i++)

# 6.包

包的本质：创建不同的**文件夹/目录**来保存类文件

同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别

![image-20220907235032904](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220907235032904.png)

引入包：import ···

声明当前类所在目录（包）：package 文件

注意事项：（1）package放在第一行
				  （2）一个类中只能有一个package
				(3)import 在package 与类之间，可以导入有多个包![image-20220908002031441](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220908002031441.png)

同包的权限大于子类，若是在同包下的子类，默认也可以访问，不同报下的子类，默认无法访问

# 7.vector<Arraylist<>,enumType

遍历

**vector 遍历**

```java
import java.util.Vector;

public class VectorTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
	Vector v=new Vector();
	v.add(123);
	v.add("I love you");
	v.add(Integer.valueOf(234));
	v.add(23.5);
	//for
	for(int i=0;i<v.size();i++) {
		System.out.println(v.get(i));
	}
	//for-in
	for(Object s:v) {
		System.out.println(s);
	}
	//Lambda Expression-for-each
	v.stream().forEach(obj->System.out.println(obj));
	v.forEach(obj->System.out.println(obj));
}
}
```

**arraylist遍历**

```
import java.util.Arraylist;
public class Arraylist{
	public static void main(String [] args){
	Arraylist<int> array = new <int>Arraylist;
	
	}
}
```



# 8.继承

## 8.0基础

public class 子类名 extends 父类名

一般为main（main中创建子类的对象并调用父类的成员与方法），一个父类，两个子类



**注意事项**

1、父类的私有属性和方法子类无法直接访问，要通过公共的方法

2、当创建子类对象时，一定先会调用父类的无参构造器

3、继承只能单继承（继承一个父类）

## 8.1super（）this（）

要调用父类的哪一个构造器，就用super()与父类的构造器对应

当调用父类的无参构造器时，则调用super() ---默认调用（在创建子类的构造函数之前会默认调用父类的无参构造函数）
当调用父类的有参构造器时，则调用super(参数)  

this()的使用:

新语法，如果当前的构造方法区调用另一个本类中的构造方法时，可以使用this(参数列表)，也就是当前构造方法的代码和被调用的构造方法里的代码类似时，在当前构造方法中使用this(参数列表)，实现代码复用。



注意：super()只能在构造器的第一行，在构造方法，this()只能出现的第一行，并且每个构造方法中最多只能有一个this()，super()和this（）不能同时存在

例1.

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221009101530598.png" alt="image-20221009101530598" style="zoom:50%;" />
最关键：分析出子类构造函数有默认super（），在B（）构造函数里无super（），因为无法与this（）共存。

例2.

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221009101936745.png" alt="image-20221009101936745" style="zoom:50%;" />

输出：A类---B类有参---C类有参---C类无参

## 8.2super

super:代表父类引用，用于访问父类的属性、方法、构造器

其实与c++类似，java的父类若没有有参构造器，可以不用显式出无参构造器

**基本语法**

1.访问父类的属性，但不能访问private属性
super.属性名
2.访问父类的方法，不能访问private方法
super.方法（）
3.调用父类的构造器，只能在第一行
super（）
4.super不限于父类，若爷爷类和子类有共同成员，也可以使用super，若是多个基类有相同成员，则按照就近原则（父亲>爷爷）

**super的好处**：

1.调用父类的构造器（分工明确，父类属性由父类初始化，子类属性由子类初始化）

一般习惯：子类有参构造器
public son(int a,int b,int c){
super(a,b);//父类有构造器就用了
this->c = c;
}

2.当子类有和父类相同的成员重名时，为了访问父类的成员，必须使用super，如果没有重名，可以用super或this或直接访问

# 9.方法重写/方法覆盖

1.子类的方法的参数，名称与父类一样
2.子类的方法返回类型必须等于父类的返回类型或者是父类的返回类型的子类
3.子类方法权限不能缩小父类的访问权限

![image-20221010195203686](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221010195203686.png)

# 10.多态

**向上转型：**

多态通过重载和重写来实现

向上转型：一个对象的编译类型（父类固定）和运行类型（不同的子类对象运行类型不同，可以变化---多态的特点）可以不一样（跟c++的父类指针或引用指向子类对象）

animal a = new dog(); 父类引用指向子类对象

特点：可以调用父类的所有成员，不可以调用子类特有（因为在编译阶段，能调用哪些成员，由编译类型决定）

​			当最终运行效果看子类（运行类型）的具体实现，即调用方法时，按照子类开始查找	

例子：

![image-20221010212123558](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221010212123558.png)

main.java中的方法void feed(Animal animal,Food food){}，对象调用时传入Animal子类，Food子类，增加了代码的复用性

```
向上转型是字子类对象当作父类来用，本质生说它是子类的对象，但是是父类的引用。所以只能看见父类的成员变量，而子类自己的成员变量就看不到。如果必须访问子类的成员变量，就要强制转换（向下转型）。 方法就要看子类中是不是重写了父类的方法，如果重写了，而且是父类的引用指向子类对象，那么在运行期间，new的哪个对象就执行哪个对象的方法。 方法和成员变量不同就是因为java的核心机制
```

**向下转型(父类指针转为子类本身指针）：**

 ![image-20221019004047233](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221019004047233.png)

Animal animal = new cat();

Cat cat = (Cat) animal;//向下转型

```java
向上转型：变量、方法重写，而父类指针调用父类变量，子类方法
向下转型：当子类有特有属性或者方法调用时，使用向下转型即可调用
public class main {
    public static void main(String[] args) {
        base b = new son("tOM");
        System.out.println(b.count);
        b.print();
        son s = (son)b;//向下转型
        s.printage();
        //一步到位：((son)b).printage()
    }

}
class base{
    base(){

    }
    base(String name){
        name = name;
    }
    int count = 10;
    private String name;
    void print(){
        System.out.println("调用父类");
    }
}
class son extends base{
    son(String oname){
        super(oname);
    }
    int count = 5;
    private int age = 999;
    //子类重写的方法
    void print(){
        System.out.println("调用子类");
    }
    //子类特有的方法
    void printage(){
        System.out.println(age);
    }
}

-----------------------------------------
输出：10 
    调用子类
    999
```



意义：

1. 向上转型的主要意义在于：当我们需要多个同父的对象调用某个方法时（重写的父类中的方法），通过向上转换后，则可以确定参数的统一，方便程序设计。
2. 向下转型的主要意义在于：通过一个形参为基类型的入口传入子类型的参数，通过向下转型还原为子类型，调用子类型中（特）有的方法，这便是多态。

动态绑定机制：

​    1.调用对象方法的时候，该方法会和该对象的内存地址/运行类型绑定

​	2.当调用对象属性时，没有动态绑定机制，哪里声明，哪里使用

```java
例子

public class main {
    public static void main(String[] args) {
        father a = new son();
        System.out.println(a.sum());
        System.out.println(a.sum1());
    }
}
class father{
    private int i = 10;
    //当遇到一个新的方法时，若子类有则调用子类，没有再调用父类的
	//当遇到一个新的变量时，直接调用父类的
    public int sum(){
        return getI()+ 10;
    }
    public int sum1(){
        return i + 10;
    }
    public int getI(){
        return i;
    }
}
class son extends father{
    public int i = 20;
    public int getI(){
        return i;
    }
}
```

数组对象：<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221019203832383.png" alt="image-20221019203832383" style="zoom:67%;" />

A instance of B：判断A是否为B类型或者B的子类 

# 11.Object类

== and equal

==：

 **1、“==”比较两个变量本身的值，即两个对象在内存中的首地址。**

**(java中，对象的首地址是它在内存中存放的起始地址，它后面的地址是用来存放它所包含的各个属性的地址，所以内存中会用多个内存块来存放对象的各个参数，而通过这个首地址就可以找到该对象，进而可以找到该对象的各个属性)**

 **2、“equals()”比较字符串中所包含的内容是否相同。**

```
		equals与==
		String s1="abc";
		String s2="abc";
		String s3 = new String("abc");
		String s4 = new String("abc");
		System.out.println(s1==s2);//true,同时指向常量池“abc”字符串
		System.out.println(s1.equals(s2));//true
		System.out.println(s3==s4);//false
		System.out.println(s3.equals(s4));//true
```

```
+的深入解析：		
		String s1 = "ab";
		String s2 = s1+"c";
		String s3 = "abc";
		System.out.println(s2==s3);
		System.out.println(s2.equals(s3));
```

Hashcode:<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221020100916031.png" alt="image-20221020100916031" style="zoom:67%;" />

toString:

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221020102637152.png" alt="image-20221020102637152" style="zoom:67%;" />

练习1：![image-20221020170640905](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221020170640905.png)

```java
public class main {
    public static void main(String[] args) {
        person[] s = new person[3];
        s[0] = new person("tom",22,"boss");
        s[1] = new person("alex",18,"worker");
        s[2] = new person("Timmy",122,"leader");
    //年龄从小到大排序
        person temp ;
        for (int i = 0; i < s.length-1; i++) {
            for (int j = 0; j < s.length-1-i; j++) {
                if(s[j].getAge() > s[j+1].getAge()){

                    temp = s[j];
                    s[j] = s[j+1];
                    s[j+1] = temp;
                }
            }
        }
        for (int i = 0; i < s.length; i++) {
            System.out.println(s[i]);
        }
      //名字从长到短排序
        for (int i = 0; i < s.length-1; i++) {
            for (int j = 0; j <s.length-i-1 ; j++) {
                if(s[j].getName().length() < s[j+1].getName().length()){
                    temp = s[j];
                    s[j] = s[j+1];
                    s[j+1] = temp;
                }
            }
        }
        for (int i = 0; i < s.length; i++) {
            System.out.println(s[i]);
        }
    }
}
-----------------------------
输出：
person{name='alex', age=18, job='worker'}
person{name='tom', age=22, job='boss'}
person{name='Timmy', age=122, job='leader'}
person{name='Timmy', age=122, job='leader'}
person{name='alex', age=18, job='worker'}
person{name='tom', age=22, job='boss'}
```



# 12.代码块

代码块：

**基本语法
[修饰符]{
代码
};**

static代码块 作用：对类进行初始化，并且随着类的加载而执行，并且只执行一次
类的加载时机：
（1）创建实例对象（new）
（2）创建子类对象实例，父类也会被加载
（3）使用类的静态成员时（静态属性，静态方法）

作用：创建对象时，都会先调用代码块的内容，可以把重复内容放进代码块中

```java
public class codeblock {
    public static void main(String[] args) {
        b b1 = new b();
        b b2 = new b();
    }
}
class a{
    static {
        System.out.println("静态a代码块被调用");
    }
    {
        System.out.println("普通a代码块被调用")
    }
}
class b extends a{
    static {
        System.out.println("静态b代码块被调用");
    }
    {
        System.out.println("普通b代码块被调用")
    }
}
结果：
静态a代码块被调用
静态b代码块被调用
普通a代码块被调用
普通b代码块被调用
普通a代码块被调用
普通b代码块被调用
```

普通的代码块,在new对象时,会被隐式的调用。被创建一次，就会调用一次。
如果只是使用类的静态成员时，普通代码块并不会执行

静态优先级大于普通代码块（当优先级相同时，按照顺序执行）

```java
public class codeblock {
    public static void main(String[] args) {
        a b1 = new a();

    }
}
class a {
    private static int n1 = getN1();
    static {
        System.out.println("A 静态代码块01");
    }
    static int getN1(){
        System.out.println("getN1()被调用");
        return 100;
    }
}
```

```
class a{
	静态
	a(){//super()
    //调用本类的普通代码块
    代码
    }
}
```

调用顺序总结：

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221023224232603.png" alt="image-20221023224232603" style="zoom:67%;" />

# 13.单例设计模式

单例设计模式：采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例,并且该类只提供一个取得其对象实例
的方法

饿汉式

方法：

1.将构造器私有化（在内部new一个实例对象，防止对象在外部创建）

为了保证不递归调用，且在外部也能访问到子类的对象和方法，需要在变量和方法前都加上static

2.提供一个公共static方法，返回对象

```java
public class danlimoshi {
    public static void main(String[] args) {
        //若只输出这句话： System.out.println(n1);
        girl her = girl.getInstance();
        System.out.println(her);
    }

}
class girl{
    private String name;
	priavte static int n1 = 10;
    private static girl a = new girl("解决");//首先对象在内部创建，外部要得到这个对象需要使用getInstance函数得到，若girl对象和下面的getInSTANCE函数都没有static，外部无法直接通过类名调用函数（因为外部无对象），

    private girl(String name){
        this.name=name;
        System.out.println("对象被调用");
    }
    public static girl getInstance(){
        return a;
    }
    @Override
    public String toString() {
        return "girl{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

[(14条消息) private static与public static的用法及区别（Java）_Max_张红帅的博客-CSDN博客_private static](https://blog.csdn.net/u013165504/article/details/45459453)

饿汉式缺点：可能造成对象浪费，因为可能创建了对象，但没有使用

懒汉式：改正了饿汉式的优点，但有可能导致线程安全问题

```java

public class lanhanshi {
    public static void main(String[] args) {
        System.out.println(girll.n1);
        girll instance = girll.getInstance();
    }

}
class girll{
    private String name;
    public static int n1 = 10;
    private static girll girll;
    private  girll(String name){
        this.name=name;
    }
    public static girll getInstance(){
        if(girll == null){
            girll = new girll("her");
        }
        return girll;
    }
}
```

饿汉式VS懒汉式
1.二者最主要的区别在于创建对象的时机不同:饿汉式是在类加载就创建了对象实例
而懒汉式是在使用时才创建。
2.饿汉式不存在线程安全问题，懒汉式存在线程安全问题。
3.饿汉式存在浪费资源的可能。因为如果程序员一个对象实例都没有使用，那么饿汉
式创建的对象就浪费了，懒汉式是使用时才创建，就不存在这个问题。
4.在我们javaSE标准类中，java.lang.Runtime就是经典的单例模式。

# 13.final

1)当不希望类被继承时,可以用final修饰.
2)当不希望父类的某个方法被子类覆盖/重写(override)时,可以用final关键字
修饰。
3)当不希望类的的某个属性的值被修改,可以用final修饰.

细节：

一、必须赋初值，并且以后不能再修改，

赋值位置：1.定义时 2.在构造器中 3.在代码块中

若final修饰的是静态变量，则赋值位置：1.定义时 2.在代码块中

二、final类不能继承，但可以实例化对象

三、类不是final类，但含有final方法，该方法可以继承，不能重写

4)当不希望某个局部变量被修改,可以使用final修饰

5）final + static 可以调用局部常量，并且不会导致类的加载



# 14.抽象类/接口

在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。

抽象类除了不能实例化对象之外，类的其它功能依然存在，成员变量、成员方法和构造方法的访问方式和普通类一样。

由于抽象类不能实例化对象，所以抽象类必须被继承，才能被使用

final，static 不能与 abstract 共用

 因为static修饰的方法是静态方法，其可以直接被类所调用。而abstract修饰的方法为抽象方法，即无方法体的方法，不能够被直接调用，需要在子类或实现类中去编写完整的方法处理逻辑后才能使用

```
public class ab {
    public static void main(String[] args) {
        b b1= new b();
        b1.speak();
    }
}

abstract class a{
    abstract void speak();
}
class b extends a{
    void speak(){
        System.out.println("aa");
    }
}
```

接口：interface

# 15.内部类

内部局部类：

内部类的定义放在了方法里

```java
public class innerclass {
    public static void main(String[] args) {
        outer o1 = new outer();
        o1.m1();
    }
}
class outer{
    private int n1 =10;
    private void m2(){
    }
    public void m1(){
        //1.内部类（通常定义在外部类的方法中，或者代码块里）
        class inner{//2.内部类不用添加访问修饰符
            private int n1 = 5;//若内部类与外部类有相同的变量名，采用就近原则，如果想访问外部类，则使用外部类.this.成员访问,其中外部类.this为o1
            public void f1(){
                System.out.println("内部n1="+n1+"外部n1="+outer.this.n1);//3.可以访问私有类的所有属性（包括私有的）
            }
        }
        //内部类的对象在外部类的方法创建
        inner i1 = new inner();
        i1.f1();
    }

}
```

成员内部类：内部类的定义放在了成员的位置（但两者创建对象都是方法里）

```java
public class memberinnerclass {
    public static void main(String[] args) {
        outer4 out = new outer4();
        out.t1();
    }
}
class outer4{
    private int n3 =10;
    public String name = "san";
    class inner4{
        public void say(){
            System.out.println("n3="+n3+" name="+name);
        }
    }
    public void t1(){
        //外部类使用内部类创建内部类的对象
        inner4 inn = new inner4();
        inn.say();
    }

}
```

外部类使用内部类：创建内部类的对象，再访问

内部类使用外部类：直接使用

其他类使用内部类：其他类创建外部类，再用外部类.内部类 对象名 = 外部类.new 内部类（）    ---把内部类当作外部类一个成员

【注：如果外部类与内部类重名，按照就近原则，则是内部类的变量，若要访问外部类的变量，则用外部类.this.变量使用】

某个局部类你只需要用一次，那么你就可以使用匿名内部类

```java
未使用匿名内部类：
public class innerclass2 {
    public static void main(String[] args) {
        Outer04 o = new Outer04();
        o.method();
    }
}
class Outer04 {
    private int n1 = 10;

    public void method(){
        A tiger = new Tiger();
        tiger.cry();
    }
}
interface A{
    public void cry();
}
class Tiger implements A{
    public void cry(){
        System.out.println("Tiger is crying");
    }
}
class Father{
    public Father(String name){
    }
    public void test(){
    }
}
```

```java
使用了匿名内部类
public class innerclass2 {
    public static void main(String[] args) {
        Outer04 o = new Outer04();
        o.method();
    }
}
class Outer04 {
    private int n1 = 10;

    public void method(){
        //基于接口的匿名内部类
       A tiger=new A(){
           @Override
           public void cry() {
               System.out.println("tiger is crying");
           }
       };
       tiger.cry();
        //基于父类的匿名内部类
       Father f = new Father("Tom"){

       };
        //基于抽象类的匿名内部类
        animal a =  new animal(){
            public void speak(){
                System.out.println("wawadddw");
            }
        };
        a.speak();
    }
}
interface A{
    public void cry();
}

class Father{
    public Father(String name){
    }

    public void test(){
    }
}
abstract class animal{
    abstract public void speak();
}
```

匿名内部类实质：在类内直接创建一个对象去继承该类外的父类或接口(在创建对象同时继承，无需在类外继承，并且若只使用一次，可增加效率)，其中匿名的意思是（该类Outer$1未明示去继承）

本质就是生成一个一次性的基于接口或者是类的类，在调用方法的时候创建，方法执行完成之后销毁

好处：简化书写

<img src="https://img-blog.csdn.net/20160611152318968" alt="img" style="zoom:87%;" />

语法：

类外的父类名或者接口名 变量名 = new 类外名（）{

重写函数

}；

可以直接调用函数，连都不用赋给一个对象

```java
public class innerclass2 {
    public static void main(String[] args) {
        Outer04 o = new Outer04();
        o.method();
    }
}
class Outer04 {
    private int n1 = 10;

    public void method(){
       new A(){
           @Override
           public void cry() {
               System.out.println("tiger is crying");
           }
       }.cry();
       //直接di
    }
}
interface A{
    public void cry();
}
```

匿名内部类中使用最普遍的一种情况，**即以实参的形式使用**

![img](https://img-blog.csdn.net/20160523145609690)

 匿名内部类是省略了【实现类/子类名称】，但是匿名对象是省略了【对象名称】

匿名内部类的使用：[(14条消息) 匿名内部类详解_CoolTomato_的博客-CSDN博客_匿名内部类](https://blog.csdn.net/qq_34944851/article/details/51449420)



# 16.for-each

for(元素类型t 元素变量x : 遍历对象obj){ 
     引用了x的java语句; 
} 

```
public static void forDisplay(int[] a){  
        System.out.println("使用 for 循环数组");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println();
    }
public static void foreachDisplay(int[] data){
        System.out.println("使用 foreach 循环数组");
        for (int a  : data) {
            System.out.print(a+ " ");
        }
    }
```

```
int[] arr = {1, 2, 3, 4, 5};
        
        System.out.println("----------使用 for 循环------------");
        for(int i=0; i<arr.length; i++)
        {
            System.out.println(arr[i]);
        }
        
        System.out.println("---------使用 For-Each 循环-------------");
        
        //增强的 for 循环 For-Each
        for(int element:arr)
        {
            System.out.println(element);
        }
        
        System.out.println("---------For-Each 循环二维数组-------------");
        
        //遍历二维数组
        int[][] arr2 = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}} ;
        for(int[] row : arr2)
        {
            for(int element : row)
            {
                System.out.println(element);
            }
        }
        
        //以三种方式遍历集合 List
        List<String> list = new ArrayList<String>();
        
        list.add("Google");
        list.add("Runoob");
        list.add("Taobao");
        
        System.out.println("----------方式1：普通for循环-----------");
        for(int i = 0; i < list.size(); i++)
        {
            System.out.println(list.get(i));
        }
        
        System.out.println("----------方式2：使用迭代器-----------");
        for(Iterator<String> iter = list.iterator(); iter.hasNext();)
        {
            System.out.println(iter.next());
        }
        
        System.out.println("----------方式3：For-Each 循环-----------");
        for(String str: list)
        {
            System.out.println(str);
        }
```

# 17.arraylist&list

创建：ArrayList<String> s = new ArrayList<>();

插入：add（）

修改：set（索引，修改的内容）

删除：remove（）

```java
import java.util.ArrayList;

public class arraylist {
    public static void main(String[] args) {
        ArrayList<String> s = new ArrayList<>();
        s.add("s1");
        s.add("s2");
        s.add("s3");
        System.out.println(s.get(1));
        s.remove(1);
        s.set(0,"s11");
        System.out.println(s);
        for (int i = 0; i < s.size(); i++) {
            System.out.println(s.get(i));
        }
        for(String i:s){
            System.out.println(i);
        }
    }}
```

List list = new Arraylist();

list里的元素都是Object类型



# 18.lamdba 表达式

[(14条消息) Java中Collections.sort()的使用!_飞渡浮舟~~的博客-CSDN博客_collections.sort](https://blog.csdn.net/qq_23179075/article/details/78753136)

[(14条消息) Java 中 Comparable 接口的意义和用法._nvd11的博客-CSDN博客_comparable](https://blog.csdn.net/nvd11/article/details/27393445)

语法：(parameters) -> expression 或 (parameters) ->{ statements; }

```
// 1. 不需要参数,返回值为 5  
() -> 5  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

# 19.异常处理

若认为一段代码可能出现异常/问题，可以使用try-catch异常处理机制解决

try-catch：选中要异常处理的代码，若出现异常，也可以继续运行

![image-20221030101541387](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030101541387.png)

异常分为运行时异常（编译器编译不出来问题，一般是逻辑问题）和编译时异常

常见运行时异常：

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030103148195.png" alt="image-20221030103148195" style="zoom:80%;" />

常见编译时异常：

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030103226623.png" alt="image-20221030103226623" style="zoom:67%;" />

**异常处理：没有catch（捕获异常）就throw（抛出异常）**

**t-c-f语法：**

try{

代码（可能有异常，若发生异常，则异常下面的代码不执行，直接跳到catch）

}

catch（Exception e）{

//捕获到异常
1.当异常发生时
2.系统将异常封装成 Exception 对象 e ，传递给catch
3.得到异常对象后，程序员自己处理
4.如果没有发生异常，catch代码不执行
5.可编写多个catch匹配不同异常}

finally{
1.无论是否发生异常，都执行此段
2.释放管理的代码一般放在finally}

```java
public class exception1 {
    public static void main(String[] args) throws  Exception {

        try {
            String str = "斯文尼";
            int a= Integer.parseInt(str);//异常，try下面的代码不会执行了，直接跳到catch
            System.out.println("数字+"+a);
        } catch (Exception e) {
            System.out.println("异常信息="+e.getMessage());
        }
        System.out.println("程序继续运行");
    }
}
输出：异常信息=For input string: "斯文尼"
程序继续运行 	
```



```java
例1.检验输入是否是整数
import  java.util.Scanner;
public class throws1 {
    public static void main(String[] args) {
        int n = 1;
        String s="";
        Scanner s2 = new Scanner(System.in);
        while(true){
            System.out.println("请输入一个整数");
               s =  s2.next();
            try {
                n = Integer.parseInt(s);
                break;
            } catch (NumberFormatException e) {
                System.out.println("输入的不是整数");
            }

        }
        System.out.println("输入的整数n="+n);
    }
}
```

**throw：**扔给上一级

如果一个方法(中的语句执行时)可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理,而由该方法的调用者负责处理。
在方法声明中用throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类。

运行异常会默认调用throws，而编译异常不会默认调用throws

父类的throws 后的范围必须大于等于子类throws 后的范围

```java
public class throws2 {
    public static void main(String[] args) {

    }
}
class father {
    public void say() throws Exception{

    }
}
class son extends father{
    public void say() throws RuntimeException{

    }
}
```

throw 与 throws 的区别

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030164603178.png" alt="image-20221030164603178" style="zoom:67%;" />

[(14条消息) try-catch-finally的执行顺序_Sasura_321的博客-CSDN博客_catch finally执行顺序](https://blog.csdn.net/qq_40933663/article/details/88824391)

throw:[Java 异常处理 之 手动抛出异常 - 格物致知_Tony - 博客园 (cnblogs.com)](https://www.cnblogs.com/niujifei/p/14294051.html)

throw 语法：throw` `new` `异常类名(``"异常产生的原因"``);

注意：

（1）throw 关键字必须写在方法的内部

（2）throw 关键字后边 new 的对象必须是 Exception或者Exception的子类对象

　　　　　　**throw 关键字后边创建的是RuntimeException或者是 RuntimeException的子类对象,我们可以不处理,默认交给JVM处理(打印异常对象,中断程序)**

​						**throw 关键字后边创建的是编译异常(写代码的时候报错),我们就必须处理这个异常,要么throws,要么try...catch**

```
// 主方法
 public static void main(String[] args) {
        //int[] arr = null;
        int[] arr = new int[3];
        int e = getElement(arr,3);
        System.out.println(e);
 }
// 定义一个方法，获取数组指定索引处的元素
public static int getElement(int[] arr,int index){
        /*
            我们可以对传递过来的参数数组,进行合法性校验
            如果数组arr的值是null
            那么我们就抛出空指针异常,告知方法的调用者"传递的数组的值是null"
         */
        if(arr == null){
            throw new NullPointerException("传递的数组的值是null");
        }

        /*
            我们可以对传递过来的参数index进行合法性校验
            如果index的范围不在数组的索引范围内
            那么我们就抛出数组索引越界异常,告知方法的调用者"传递的索引超出了数组的使用范围"
         */
        if(index<0 || index>arr.length-1){
            /*
　　　　　　　　判断条件如果满足，当执行完throw抛出异常对象后，方法已经无法继续运算。
　　　　　　　　这时就会结束当前方法的执行，并将异常告知给调用者。这时就需要通过异常来解决。
　　　　　　　　*/
            throw new ArrayIndexOutOfBoundsException("传递的索引超出了数组的使用范围");
        }

        int ele = arr[index];
        return ele;
    }
    注：NullPointerException、ArrayIndexOutOfBoundsException 是一个运行期异常,我们不用处理,默认交给JVM处理
```

# 20.集合

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030201806549.png" alt="image-20221030201806549" style="zoom:87%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030201952620.png" alt="image-20221030201952620" style="zoom:87%;" />

集合主要分为单列集合（collection）和双列集合（map）

**Collection**

1)collection实现子类可以存放多个元素,每个元素可以是Object
2)有些Collection的实现类,可以存放重复的元素，有些不可以
3)有些Collection的实现类，有些是有序的(List),有些不是有序(Set)4) Collection接口没有直接的实现子类,是通过它的子接口Set和
List 来实现的



**迭代器Iterator**

我们在开发中经常需要遍厉集合，所以JDK专门提供了一个接口`java.util.Iterator`，这个接口的作用主要是用来迭代访问Collection中的元素的，所以换为迭代器。

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221030211411696.png" alt="image-20221030211411696" style="zoom:67%;" />

next（）：将迭代器下移，并返回元素（Object类型）

hasnext（）：判断是否还有下一个元素

```java
public class collection {
    public static void main(String[] args) {
        Collection c = new ArrayList();
        c.add(new book(10,"a"));
        c.add(new book(11,"b"));
        c.add(new book(12,"c"));
        Iterator iterator = c.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println(next);
        }
        for (Object o :c) {
            System.out.println(o);
        }
    }}
class book{
        int price;
        String name;

    public book(int price, String name) {
        this.price = price;
        this.name = name;
    }

    @Override
    public String toString() {
        return "book{" +
                "price=" + price +
                ", name='" + name + '\'' +
                '}';
    }
}
```

**for-each：**

for（元素类型 元素名：集合名或数组名）{
访问元素
}

21.泛型

[(16条消息) Java泛型详解，史上最全图文详解_Java程序员-张凯的博客-CSDN博客_java泛型](https://blog.csdn.net/qq_41701956/article/details/123473592)

**泛型的本质是参数化类型**

# 21.网络编程

IP地址：唯一标识主机（由网络号和主机号组成）

IPV4:4个字节，IPV6：16个字节

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221102190953784.png" alt="image-20221102190953784" style="zoom:67%;" />

电脑访问服务其实是访问端口（IP相当于小区，端口相当于门牌号）

协议：TCP/IP协议,数据的组织形式

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221102193203660.png" alt="image-20221102193203660" style="zoom:67%;" />

”三次握手“---可靠

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221102193533889.png" alt="image-20221102193533889" style="zoom:70%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221102200128806.png" alt="image-20221102200128806" style="zoom:80%;" />

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class Internet {
    public static void main(String[] args) throws UnknownHostException {
        //1.获取本机的InetAddress 对象
        InetAddress inetAddress = InetAddress.getLocalHost();
        System.out.println(inetAddress);//LAPTOP-UE3V8FOR/192.168.137.1
        //2.根据指定主机名 获取 InetAddress对象
        InetAddress host1 = InetAddress.getByName("LAPTOP-UE3V8FOR");
        System.out.println(host1);
        //3.根据域名 获取 InetAddress对象
        InetAddress host2 = InetAddress.getByName("www.baidu.com");
        System.out.println(host2);
        //4.通过 InetAddress对象，获取对应的地址
        String hostAddress = host2.getHostAddress();
        System.out.println(hostAddress);
        //5.通过 InetAddress对象，获取对应的域名
        String hostName = host2.getHostName();
        System.out.println(hostName);
    }
}
```

# 22.I/O

文件在程序中是以流的形式来操作的

1.file对象创建
2.file调用createNewFile函数

```java
public class IO1 {
    public static void main(String[] args) {

    }
    @Test
    public void create(){
        String filepath ="e:\\news1.txt";
        File file = new File(filepath);
        try {
            file.createNewFile();
            System.out.println("文件创建成功");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

file的函数：![image-20221105084517795](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221105084517795.png)

**流的分类**

按照数据单位	分为字节流（8 bit）二进制文件 和字符流（按字符）文本文件

| 抽象基类 | 字节流      | 字符流 |
| -------- | ----------- | ------ |
| 输入     | InputStream | reader |
| 输出     | OuputStream | writer |

```
字节流单字节读入
public class IO3 {
    public static void main(String[] args) throws IOException {
        String pathname = "D:\\hello.txt";
        File file = new File(pathname);
        FileInputStream i = new FileInputStream(file);
        int readData = 0;
        //从该输入流读取一个字节的数据，如果返回-1，则代表文件读取结束
        while((readData=i.read())!=-1){
            System.out.print((char)(readData));
        }
        i.close();
    }
}
```

```
字节流字节数组读入
public class IO3 {
    public static void main(String[] args) throws IOException {
        String pathname = "D:\\hello.txt";
        File file = new File(pathname);
        FileInputStream i = new FileInputStream(file);
        byte[] buf = new byte[8];
        int readLen = 0;
        //从该输入流读取8个字节的数据，如果返回-1，则代表文件读取结束
        //若读取正常，返回的是读取字节的个数
        while((readLen=i.read(buf))!=-1){
            //第一种输出方式
            for (byte a :buf
            ) {
                System.out.print((char)a);
            }
            //第二种输出方式
            System.out.print(new String(buf,0,readLen));
            //将字符数组转成字符串: String(char[],offset,count):将字符数组中的一部分转成字符串。
        }
        i.close();
    }
}
```

```
字节流写出
public class outputstream {
    public static void main(String[] args) throws IOException {
        String pathname = "D:\\hi.txt";
        File file = new File(pathname);
        FileOutputStream o = new FileOutputStream(file);//FileOutputStream(file,true)代表追加
        //写入一个字节
        o.write('a');
        //写入一个字符串
        String s="hello,world";
        o.write(s.getBytes());
    }
}
```

```
文件拷贝
public class Filecopy {
    public static void main(String[] args) throws IOException {
        String pathname1 = "D:\\1.jpg";
        File file = new File(pathname1);
        FileInputStream i = new FileInputStream(file);
        String pathname2 = "E:\\1.jpg";
        File file2 = new File(pathname2);
        FileOutputStream o = new FileOutputStream(file2);
        byte[] b = new byte[1024];//一定要有一个中间变量，文件---程序---文件
        while((i.read(b)!=-1)){
            o.write(b);
        }
        System.out.println("传输完毕");
    }
}
```

字符流Reader/Writer

```
FileReader 单个字符
public class Filereader {
    public static void main(String[] args) throws IOException {
        String pathname ="D:\\hello.txt";
        File file = new File(pathname);
        FileReader f = new FileReader(file);
        int data = 0;
        while((data = f.read())!=-1){
            System.out.print((char)data);
        }
    }
}
FileReader 多个字符
public class Filereader {
    public static void main(String[] args) throws IOException {
        String pathname ="D:\\hello.txt";
        File file = new File(pathname);
        FileReader f = new FileReader(file);
        char[] buf=new char[1024];
        while((f.read(buf))!=-1){
            System.out.print(buf);
        }
    }
}
```

对于FileWriter，一定要关闭流，或者flush才能把真正把数据写入到文件

```
public class Filewriter {
    public static void main(String[] args) throws IOException {
        String pathname="D:\\3.txt";
        File file = new File(pathname);
        FileWriter w = new FileWriter(file);
        char[] s = {'1','2','3'};
        //write后可以跟字符，字符串，char数组
        w.write('中');
        w.write("dawawda");
        w.write(s);
        //write(String,off,len) or write(char[],off,len)截取一段
        w.close();//非常重要，只有关闭后，才能写入到文件里
    }
}
```

**字节流和处理流**

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221107192148946.png" alt="image-20221107192148946" style="zoom:60%;" />

处理流BufferedReader里面就有一个Reader对象，可以使用所有节点流的对象，BufferedWriter同理

```java
public class bufferedr {
    public static void main(String[] args) throws IOException {
        String pathname="d:\\hello.txt";
        File file = new File(pathname);
        FileReader fileReader = new FileReader(file);
        BufferedReader buffedReader = new BufferedReader(fileReader);
        //按行读取，效率高
        String line;
        while((line=buffedReader.readLine())!=null){
            System.out.println(line);
        }
        buffedReader.close();
    }
}
```

```java
public class bufferedw {
    public static void main(String[] args) throws IOException {
        String pathname = "d:\\hi.txt";
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(new File(pathname)));
        bufferedWriter.write("lzp");
        bufferedWriter.newLine();//换行
        bufferedWriter.write('1');
        bufferedWriter.close();
    }
}
```

# 23.swing

上机：

Menu：![image-20221114173546861](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221114173546861.png)

Component：![image-20221114173746657](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221114173746657.png)

## swing组件

 窗口、菜单栏、对话框、标签、按钮、[文本框](https://so.csdn.net/so/search?q=文本框&spm=1001.2101.3001.7020)等等，这些“元素”统一被称为 **组件**（`Component`）

JTextField(输入）：

```
// 获取文本框中的文本
String getText().trim()
getText ().trim ()的作用是:在获得的文本中除去空格.
```

JTextArea（显示）：

```
//用于显示文本框的文本
void setText(String text);

```



JFileChooser:`JFileChooser`，文件选取器。JFileChooser为用户选择文件提供了一种简单的机制，包括 **打开文件** 和 **保存文件**。

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221114201843014.png" alt="image-20221114201843014" style="zoom:67%;" />

```
// 获取选择的文件（一般在用户选择完文件点击了确认或保存后通过该方法获取选中的文件）
void getSelectedFile()
chooser.getSelectedFile().getAbsolutePath()//取文件的路径
```

在open里的操作：

```
//第一步：基础（接口和继承类）
接口：有一个返回List类型的函数
public interface ReadFileInter {
	//abstract methods
	public List<Student> readFromFile(String pathname);
	
}
实现类
public class ReadTxtFileImpl implements ReadFileInter {

	@Override
	public List<Student> readFromFile(String pathname) {
		// TODO Auto-generated method stub
		List<Student> arlist=new ArrayList<Student>();
		File f = new File(pathname);
		FileReader fr;
		try {
			fr = new FileReader(f);
			BufferedReader br=new BufferedReader(fr);//缓冲区完成文件的高效读入操作
			String line=null;
			//依次每行读入到程序中，通过br的readline先读入到line中，判断是否为空
			while((line=br.readLine())!=null) {
				String []fs=line.split(",");
				Student s=new Student(fs[0],fs[1],fs[2],fs[3],Integer.parseInt(fs[4]),Integer.parseInt(fs[5]));
				arlist.add(s);
		}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (NumberFormatException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return arlist;
	}
	}
	//将存放Student的两个list声明为全局变量，方便调用
	private List<Student>arlist;//输入（全部学生）
	private List<Student>selist;//输出

openfile单击后的操作（打开文件）：
public void actionPerformed(ActionEvent e) {
				ReadFileInter2 fi = new ReadTxtFilelmpl2();//创建一个对象，实现接口类
				List<Student>arlist = new ArrayList<Student>(); //创建一个接受对象arlist
				JFileChooser chooser = new JFileChooser();
			    FileNameExtensionFilter filter = new FileNameExtensionFilter(
			        "txt & csv", "txt", "csv");//修改文件格式
			    chooser.setFileFilter(filter);
			    int returnVal = chooser.showOpenDialog(null);
			    if(returnVal == JFileChooser.APPROVE_OPTION) {
			    //从chooser获取到的路径，在从路径获取到的数据存放在arlist里
			       arlist=fi.readFromFile(chooser.getSelectedFile().getAbsolutePath());
			    }			    
			}
searchbyID单击后的操作（输入要查找的信息，并搜索，若输入为空，则显示全部）：
public void actionPerformed(ActionEvent e) {
				String id=input.getText().trim();
				
				if(id.length()==0) {
					selist=arlist;
				}
				else {//not empty
							//初始化selist
							selist = new ArrayList<Student>();
							//遍历arlist（需在全局变量里），if语句挑选出符合条件的
							for(Student s:arlist) {
								if(s.getId().contains(id)) {//search by id
									selist.add(s);
								}
							}
						}
				//show students on GUI's table
				//通过Jtable来显示
			    String[] col={"ID","Name","Gender","School","Score","Age"};//列名
			    DefaultTableModel tm=new DefaultTableModel(col,0);
			    for(Student s:selist) {
			    	Vector<String> line=new Vector<String>();
			    	line.add(s.getId());
			    	line.add(s.getName());
			    	line.add(s.getGender());
			    	line.add(s.getSchool());
			    	line.add(String.valueOf(s.getScore()));
			    	line.add(String.valueOf(s.getAge()));
			    	tm.addRow(line);
			    }//end of for
			    table.setModel(tm);
				
				
			}//end of handler
		});

保存的接口：
public interface WriteFileInter {
	public void writeToFile(String pathname,List<Student>list);
}
实现类：
public class WriteTxtFileImpl implements WriteFileInter {

	@Override
	public void writeToFile(String pathname, List<Student> list) {
		// TODO Auto-generated method stub
		try {
			BufferedWriter bw = new BufferedWriter(new FileWriter(new File(pathname)));
			for(Student s:list) {
				bw.write(s.getId()+" "+s.getName()+" "+s.getScore());//3 columns to file
				bw.newLine();
			}
			bw.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}


savefile单击后的操作（将selist保存在一个txt文件（路径通过choose选择）中）：
public void actionPerformed(ActionEvent e) {
				WriteFileInter wf=new WriteTxtFileImpl();
				//savefile dialog
				JFileChooser chooser = new JFileChooser();
			    FileNameExtensionFilter filter = new FileNameExtensionFilter(
			        "txt & csv files", "txt", "csv");
			    chooser.setFileFilter(filter);//设置后缀器
			    int returnVal = chooser.showSaveDialog(null);//打开目录
			    if(returnVal == JFileChooser.APPROVE_OPTION) {
			    	//写入到新文件
			    	wf.writeToFile(chooser.getSelectedFile().getAbsolutePath(), selist);	
			    }
				
			}
```

要显示[JTable](https://so.csdn.net/so/search?q=JTable&spm=1001.2101.3001.7020)组件（需要用到）TableModel接口（需要下面这个类才能实现）DefaultTableModel类（Jable需要TableModel，然而Tablemodel这个接口需要DefaultTableModel类实现）

![image-20221114204403054](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221114204403054.png)

第一个参数为列名，第二个参数为行数

并且需要在DefaultTableModel填数据，需要Vector类型的数据

所以

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221114211848236.png" alt="image-20221114211848236" style="zoom:67%;" />

# 24.workbench

第一步：建立数据库（注意设置utf-8)

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120094849459.png" alt="image-20221120094849459" style="zoom:67%;" />

第二步：建表（注意设置utf-8）

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120094938144.png" alt="image-20221120094938144" style="zoom:67%;" />

第三步：导入数据

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120095350930.png" alt="image-20221120095350930" style="zoom:80%;" />

## 数据库与java连接（JDBC)

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120110748736.png" alt="image-20221120110748736" style="zoom:80%;" />

其中sql语句若是select 则有返回数据结果集，若是insert 只用向数据库插入数据即可

连接步骤：

第一步：将mysql-connector-java里的jar包复制到新建的文件夹中

![image-20221120111354853](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120111354853.png)

第二步：导入jar包（在classpath中 选择add JARs 导入第一步里的jar文件）

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120111523147.png" alt="image-20221120111523147" style="zoom:80%;" />

第三步：到mysql官网，documentation

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120111946678.png" alt="image-20221120111946678" style="zoom:47%;" />

![image-20221120112030117](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120112030117.png)

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120112215628.png" alt="image-20221120112215628" style="zoom:67%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120112358491.png" alt="image-20221120112358491" style="zoom:67%;" />

把其中conn copy下来

```
conn =
       DriverManager.getConnection("jdbc:mysql://localhost/test?" +
                                   "user=minty&password=greatsqldb");
```

其中需要改动的地方有三处："jdbc:mysql://localhost/数据库名字?" +"user=用户名&password=密码"



```
public class DBTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//step1 Get DB Connection
		Connection conn=null;
		Statement stmt=null;
		ResultSet rs=null;
		try {
			//第一步：数据库的连接
			conn =DriverManager.getConnection("jdbc:mysql://localhost/mystudb?" +
				                                   "user=root&password=root");
			//第二步：创建语句
			stmt=conn.createStatement();
			//第三步：执行语句
			rs = stmt.executeQuery("select * from stu");
			//第四步：显示结果集
			while(rs.next()) {
				System.out.println(rs.getString(1)+rs.getString(2)+rs.getInt(5));
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}
```

[conn.createStatement() 这个东西不太明白，求解答-CSDN社区](https://bbs.csdn.net/topics/390092534)

把上面一个程序的第一步和其他步独立开来，封装分成不同的程序，方便管理，并且加上GUI界面

```
创建实例类：StudentDAO（与原来的Student不同的地方是int改为了Integer类型（Integer是int的Warpper类，是面向对象的即OOP的对象类型。）
	private String id;
	private String name;
	private String gender;
	private String school;
	private Integer score;
	private Integer age;
第一步：数据库的连接 DBconnectFactory.java
public class DBconnectFactory {
 	//注意这里添加了static类型，优点是可以在外面直接通过类名调用到该函数，不用实例化对象
	public static Connection createDBConnect(String type) {
		Connection conn = null;
		//添加了数据库类型的区分
		if(type.equals("mysql")) {
			try {
				conn =DriverManager.getConnection("jdbc:mysql://localhost/mystudb?" +
				        "user=root&password=root");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		else if(type.equals("sqlserver")) {
			
		}
		return conn;
	}
}
第二步：sql语句的执行 
首先定义查询接口
public interface QueryServiceInter2 {
	public List<StudentDAO2> selectAll();
	public List<StudentDAO2> selectById(String id);
}
实现类
public class QueryServiceImpl2 implements QueryServiceInter2 {
	Connection conn = null;
	PreparedStatement ps =null;
	ResultSet rs = null;
	//在构造函数中进行数据库的连接
	public QueryServiceImpl2(String type) {
		conn = DBconnectFactory.createDBConnect(type);
	}
	@Override
	public List<StudentDAO2> selectAll() {
		// TODO Auto-generated method stub
		List<StudentDAO2> stulist = new ArrayList<StudentDAO2>();
		try {
			//创建语句（prepareStatement预编译）
			ps = conn.prepareStatement("select * from stu");
			//执行语句
			rs = ps.executeQuery();
			while(rs.next()) {
				StudentDAO2 st =new StudentDAO2(rs.getString(1),rs.getString(2),rs.getString(3),
						rs.getString(4),rs.getInt(5),rs.getInt(6));
			//结果集读取数据的方法主要是getXXX() ，他的参数可以s整型表示第几列（是从1开始的），还可以是列名。返回的是对应的XXX类型的值
				stulist.add(st);
			}
			
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return stulist;
	}

	@Override
	public List<StudentDAO2> selectById(String id) {
		// TODO Auto-generated method stub
		List<StudentDAO2> stulist = new ArrayList<StudentDAO2>();
		try {
			//注意这里的id是变量（需要输入的）
			ps = conn.prepareStatement("SELECT * FROM mystudb.stu where id like '%"+id+"%'");
			rs = ps.executeQuery();
			while(rs.next()) {
				StudentDAO2 st =new StudentDAO2(rs.getString(1),rs.getString(2),rs.getString(3),
						rs.getString(4),rs.getInt(5),rs.getInt(6));
				stulist.add(st);
			}
			
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return stulist;
	}

}

定义插入接口
public interface InsertServiceInter {
	public void insert(String input);
}

实现类
public class InsertServiceImpl implements InsertServiceInter {
	Connection conn = null;
	PreparedStatement ps = null;
	public InsertServiceImpl(String type) {
		conn=DBconnectFactory.createDBConnect(type);
	}
	@Override
	public void insert(String input) {
		// TODO Auto-generated method stub
		String[] v=input.split(",");
		String sql="insert into stu values('"+v[0]+"','"+v[1]+"','"+v[2]+"','"+v[3]+"',"+v[4]+","+v[5]+")";
		try {
			ps=conn.prepareStatement(sql);
			ps.execute();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}

```

**关于PreparedStatement理解：**

PreparedStatement是Statement的子接口，表示预编译的 SQL 语句的对象，SQL 语句被预编译并存储在PreparedStatement 对象中。然后可以使用此对象多次高效地执行该语句。如果有参数的话还需要添加输入的参数。

PreparedStatement对象不仅包含了SQL语句，而且大多数情况下这个语句已经被预编译过，因而当其执行时，只需DBMS运行SQL语句，而不必先编译。当你需要执行Statement对象多次的时候，PreparedStatement对象将会大大降低运行时间，当然也加快了访问数据库的速度。
这种转换也给你带来很大的便利，不必重复SQL语句的句法，而只需更改其中变量的值，便可重新执行SQL语句。选择PreparedStatement对象与否，在于相同句法的SQL语句是否执行了多次，而且两次之间的差别仅仅是变量的不同。如果仅仅执行了一次的话，它应该和普通的对象毫无差异，体现不出它预编译的优越性。

**GUI界面：**

首先先修改默认布局

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120171531424.png" alt="image-20221120171531424" style="zoom:67%;" />

JSplitPane将操作界面分为两块，左边为操作区，右边为显示区

![image-20221120171400459](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120171400459.png)

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120172242346.png" alt="image-20221120172242346" style="zoom:50%;" />并且设置resizeWeight 为0.5

接下来使用JPanel（在比较复杂的布局要求时，就需要使用布局管理器的组合使用，这个时候就需要使用到面板组件JPanel）

来分别给左右两边上桌布![image-20221120172321439](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120172321439.png)

并对JPanel的属性进行一些修改<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120172658133.png" alt="image-20221120172658133" style="zoom:67%;" />

需要使用的组件（左边操作区的组件记得要改名）：<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120195023491.png" alt="image-20221120195023491" style="zoom:80%;" />

JRadioButton按钮选项合并：crtl选中两个，并右键选择set buttongroup--new standard![image-20221120194817034](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120194817034.png)

JComboBox

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120195323662.png" alt="image-20221120195323662" style="zoom:87%;" />

Jtable使用在右边的显示区，并且需要右键surround with JScrollPane

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20221120173347027.png" alt="image-20221120173347027" style="zoom:67%;" />

```
GUI:
searchbyID单击后的操作：
public void actionPerformed(ActionEvent e) {
				String id = textField.getText().trim();
				QueryServiceInter2 qsi = new QueryServiceImpl2("mysql");
				if(id.length()==0) {
					//select all
					list=qsi.selectAll();
				}
				else {
					list=qsi.selectById(id);
				}
				
				String[] col={"ID","Name","Gender","School","Score","Age"};//列名
			    DefaultTableModel tm=new DefaultTableModel(col,0);
			    for(StudentDAO2 s:list) {
					/*第一种方法
					 * Vector<String> line=new Vector<String>(); line.add(s.getId());
					 * line.add(s.getName()); line.add(s.getGender()); line.add(s.getSchool());
					 * line.add(String.valueOf(s.getScore())); line.add(String.valueOf(s.getAge()));
					 * tm.addRow(line);
					 */
			    	//第二种方法
			    	String[] line= 	 		  				  	 {s.getId(),s.getName(),s.getGender(),s.getSchool(),String.valueOf(s.getScore())
				    ,String.valueOf(s.getAge())	};
			    	tm.addRow(line);
			    }//end of for
			    table.setModel(tm); 
				
			}
			
			
```

# 25.倒计时



```
private long days;
private Timer timer;
public void actionPerformed(ActionEvent e) {
				//Date-LocalDataTime--
				LocalDateTime start=LocalDateTime.now();//开始时间
				LocalDateTime end=LocalDateTime.of(2022,12,2,0,0,0);//结束时间
				Duration duration=Duration.between(start, end);//间隔
				days=duration.toDays();
				//间隔天数
				String input=textField.getText().trim();
				timer=new Timer(Integer.parseInt(input),
						//匿名nei'bu'lei
						new ActionListener() {
							@Override
							public void actionPerformed(ActionEvent e) {
								//show days-hours-seconds
								lblNewLabel.setText(days+"  天");
								days--;
								if(days==0) timer.stop();
							}
					
				});
				timer.start();
				
			}
```

方法Timer（） 具有以下参数：

- int延迟 - 初始延迟和事件间延迟的毫秒
- 动作侦听器 - 初始侦听器;可以为空

# 26.synchronized

synchronized:多线程获得目标对象的锁，谁有了锁才可以访问该对象内部的共享资源（使得不会大家同时去访问该资源）

# 27.反射

编译：写代码时如果没爆红，就编译成功
运行：![image-20230402204804857](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402204804857.png)

开放封闭原则：对软件设计者来说，必须在不需要对原有的系统进行修改的情况下，实现灵活的系统扩展

只有依赖于抽象。实现开放封闭的核心思想就是对抽象编程，而不对具体编程，因为抽象相对稳定。让类依赖于固定的抽象，所以对修改就是封闭的

反射：加载完类之后，在堆内存的方法区中就产生了一个class类型的对象（一个类只有一个class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为:反射。（对象----->类------>包）

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402155855617.png" alt="image-20230402155855617" style="zoom:80%;" />

**Class类**

Class就是一个普通的类，这个类描述的是所有的类的公共特性

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402165251945.png" alt="image-20230402165251945" style="zoom:80%;" />

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402165232072.png" alt="image-20230402165232072" style="zoom:80%;" />

`Class`类就是通过`Field`和`Method`来描述类中的字段和方法

如何使用Class？

Class就是一个普通的类啊，使用普通的类就是创建一个对象，才能使用啊 很简单啊，我们创建一个Class的对象就行了啊，就可以调用对象的各种方法

如何获取一个Class的对象

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402164335292.png" alt="image-20230402164335292" style="zoom:80%;" />

由于构造函数是private属性，则无法直接new一个对象

方法：

```
public static void main(String[] args){
        //第一种：调用运行时类的属性：.class
        Class c1 = String.class;

        //第二种：通过运行时类的对象，调用getClass()
        String s = "hello,world";
        Class c2 = s.getClass();

        //第三种,以上面的Book类为例,其实就是动态加载类，注意捕获异常，因为类有可能不存在 
        try {
            Class c3 = Class.forName("com.test.Book");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
```

一般我们把Class的对象叫**字节码**

- 有个某个类的字节码以后，我们就知道知道这个类的许多信息了
- Class一般是在运行时使用，你只要告诉我类名，我就可以知道这个类中有多少方法，有多少字段，怎么调用等等
- Filed，Method(还有其它的，我们只说这2个)，分别是描述类的字段和类的方法的

[一篇文章彻底搞懂Java的大Class到底是什么 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/372418927)

[(37条消息) Class和class? 类对象和类的对象? 一篇文章让你摸到头脑_class和class_张友谅的博客-CSDN博客](https://blog.csdn.net/zzy372219506/article/details/90934987)

理解：一个class类（A）既可以当作一个类，也可当作一个对象，当作对象时，Class就是A的类，Class类描述类的结构，A描述本身的结构，里面有getConstructor，getDeclaredMethod，getDeclaredConstructor方法

对象调用方法？方法调用对象？其实只是把方法当作对象

Person类是Class类的对象

```
	public void test1(){
        Person p1 = new Person("Tom",12);
        p1.age=10;
        System.out.println(p1.toString());
        p1.show();
        //不能调用私有属性、私有方法
    }
    @Test
    public void test2() throws Exception {
        Class clazz = Person.class;
        //通过反射，创建Person类的对象
        Constructor constructor = clazz.getConstructor(String.class, int.class);
        Object tom = constructor.newInstance("Tom", 12);
        Person p = (Person)tom;
        System.out.println(p.toString());
        //通过反射，调用对象指定的属性、方法
        //调用属性
        Field age = clazz.getDeclaredField("age");
        age.set(p,10);
        System.out.println(p.toString());
        //调用方法
        Method show = clazz.getDeclaredMethod("show");
        show.invoke(p);
        //反射可以调用私有属性、方法和构造器
        Field name = clazz.getDeclaredField("name");
        name.setAccessible(true);
        name.set(p,"lisi");
        System.out.println(p.toString());
    }
```

反射与直接new对象：

反射优点：动态性，编译时不用确定具体创建哪个对象，运行时前端需要哪个对象，后台就创建哪个对象

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402175742971.png" alt="image-20230402175742971" style="zoom:80%;" />

需要login，则造loginServlet对象

1.类的加载过程:
程序经过javac.exe命令以后，会生成一个或多个字节码文件(.class结尾)。
接着我们使用java.exe命令对某个字节码文件进行解梓运行。相当字节码文件加载到内存中。此过程就称为类的加载。加载到内存中的类，我们就称为运行时类，此运行时类，就作为Class的一个实例。
2.换句话说，class的实例就对应着一个运行时类。
3.加载到内存中的运行时类，会缓存一定的时间。在此时间之内，我们可以通过不同的方式来获取此运行时类。

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230402204416303.png" alt="image-20230402204416303" style="zoom:67%;" />

不同方式获取的运行时类c1、c2、c3都是同一个

<img src="C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20230403190537408.png" alt="image-20230403190537408" style="zoom:80%;" />

代理

●代理设计模式的原理:
使用一个代理将对象包装起来,然后用该代理对象取代原始对象。任何对原始对象的调用都要通过代理。代理对象决定是否以及何时将方法调用转到原始对象上。

动态代理：在运行时代理类才真正确定下来，所以只需一个代理类完成全部的代理功能

有接口情况：JDK动态代理

无接口情况：CGLIB动态代理


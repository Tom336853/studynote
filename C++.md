enum枚举：![image-20220307084044097](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220307084044097.png)

[C++中enum（转载） - LeonWen - 博客园 (cnblogs.com)](https://www.cnblogs.com/leonwen/p/6659215.html)

指针数组输入

[定义一个指针数组，如何对它输入字符串？_百度知道 (baidu.com)](https://zhidao.baidu.com/question/1604531804114116147.html)

[(3条消息) 经常犯的错误：结构体指针只定义而没有初始化_kk_forward的博客-CSDN博客](https://blog.csdn.net/kwb2015130086/article/details/88046029)

在定义数组时，数组大小必须是常量，不能使变量或变量表达式[vs2015 c++ 编译出错,说 表达式的计算结果不是常数,请问是什么问题，如何解决。_百度知道 (baidu.com)](https://zhidao.baidu.com/question/714259744832313805.html)

字符串指针：[C语言字符串指针（指向字符串的指针） - 徐唱 - 博客园 (cnblogs.com)](https://www.cnblogs.com/xc90/articles/10797236.html)

c++初始化式：[(3条消息) 初始化式构造函数_xiaosi2468的博客-CSDN博客](https://blog.csdn.net/xiaosi2468/article/details/6642230?locationNum=1&fps=1)

用static修饰的变量，在他的作用域内，定义语句就只执行一次///////对静态局部变量是在编译时赋初值的，即只保留初值一次，以后每次调用函数时不再重新赋初值而只是保留上次函数调用结束时的值

[(3条消息) 详解scanf、gets、getchar和getch 使用及其原理。_计量小菜鸡的博客-CSDN博客](https://blog.csdn.net/qq_15345177/article/details/84671081?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-84671081.pc_agg_new_rank&utm_term=gets的原理&spm=1000.2123.3001.4430)

[C语言中全局变量调用后会被修改值吗_百度知道 (baidu.com)](https://zhidao.baidu.com/question/876670372503007772.html)

[(4条消息) c++ 定义对象数组报错no matching function for call to_cjh_hit的博客-CSDN博客](https://blog.csdn.net/weixin_43915798/article/details/106128280)

int f()
{ static int i=0;

int s=1; s+=i; i++;
return s; }
main()
{ int i,a=0;
for(i=0;i<5;i++) a+=f()；
prinft("%d\n",a);}答案：15

用const修饰的成员，需要用初始化列表来初始化

[必须在构造函数初始化列表里初始化的数据成员有哪些_Tony_Xian的博客-CSDN博客_构造函数必须初始化类的所有数据成员](https://blog.csdn.net/boiled_water123/article/details/105849494)

# 1.类和对象

## 1.1对象特性

### 1.1.1浅拷贝和深拷贝

补充：new 数据类型
new作用：在堆区开辟数据，利用new创建的数据，会返回该数据对应的类型的指针
delete作用：手动释放内存

```c++
#include<bits/stdc++.h>
using namespace std;
int* func(){
	//在堆区创建整型数据new int(10) 
	// new返回的是该数据类型的指针 
	int *p=new int(10);
	return p;
}

void test01(){
	int*p=func();
	cout<<*p<<endl;
	delete p;
} 
void test02(){
	//创建10个整型数据的数组 
	int *array=new int[10];
	for(int i=0;i<10;i++){
		array[i]=i;
	}
	for(int i=0;i<10;i++){
	cout<<array[i];
	}
	//释放数组的时候，需要加上一个[]
	delete []array; 
}

int main(){
	test01();
	test02();
}
```

浅拷贝：拷贝构造函数里的赋值，如person p2(p1);

深拷贝：利用new创建堆区，指针指向堆区的数据

![image-20220221212506496](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220221212506496.png)浅拷贝，一个一个字符的拷贝，然而height也是逐字节拷贝，把地址也拷贝，当执行析构函数时，p2后进先出，先执行析构，释放堆区，而p1再执行析构函数，释放一个空区域，则非法操作（浅拷贝的问题是堆区的内存重复释放)

原代码（浅拷贝）：

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	public:
		person(){
			cout<<"person的默认构造函数"<<endl;
		}
		person(int age,int height){
			m_age=age;
			m_height=new int(height);//在堆区创建一个整型的height数据 
			cout<<"person的有参构造函数"<<endl; 
		}
		person(const person& p){
			m_age=p.m_age;
			m_height=p.m_height;
			cout<<"person的拷贝构造函数"<<endl; 
		}
		~person(){//析构函数的作用释放堆区数据 
			if(m_height!=NULL){
				delete m_height;
				m_height=NULL; 
			}
			
			cout<<"person的析构构造函数"<<endl; 
		}
		int m_age;
		int *m_height;
};
void test01(){
	person p1(18,160);
	cout<<p1.m_age<<&p1.m_height;
	person p2(p1);
	cout<<p2.m_age<<&p2.m_height;
} 
int main(){
	test01();
}
```

解决方法：自己实现拷贝构造函数，创建一个新的空间，解决浅拷贝问题

![image-20220221220020721](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220221220020721.png)

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	public:
		person(){
			cout<<"person的默认构造函数"<<endl;
		}
		person(int age,int height){
			m_age=age;
			m_height=new int(height);//在堆区创建一个整型的height数据 
			cout<<"person的有参构造函数"<<endl; 
		}
		person(const person& p){
			m_age=p.m_age;
			m_height=new int(*p.m_height);
			cout<<"person的拷贝构造函数"<<endl; 
		}
		~person(){//析构函数的作用释放堆区数据 
			if(m_height!=NULL){
				delete m_height;
				m_height=NULL; 
			}
		
			cout<<"person的析构构造函数"<<endl; 
		}
		int m_age;
		int *m_height;
};
void test01(){
	person p1(18,160);
	cout<<p1.m_age<<*p1.m_height;
	person p2(p1);
	cout<<p2.m_age<<*p2.m_height;
} 
int main(){
	test01();
}
```

### 1.1.2初始化列表

初始化列表可以为该类成员对象赋值，也可为创建基类构造函数

什么时候初始化列表调用？

1.成员有对象（且该对象为有参构造函数），ps：其实也可以调用子类对象的构造函数

当为成员对象初始化时，既可以直接初始化列表（传一个对象赋值或者调用对象的构造函数可以直接），也可以调用成员对象的set（）函数

![image-20220615163725934](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220615163725934.png)

![image-20220615163734662](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220615163734662.png)

2.该类为派生类，则为基类创建构造函数



```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	public:
		//传统初始化操作 
		//person(int a,int b,int c){
			//m_a=a;
			//m_b=b;
			//m_c=c;
		
		//初始化列表
		person():m_a(10),m_b(20),m_c(30){
		
		}
		int m_a;
		int m_b;
		int m_c;
};
void test01(){
	//person a(10,20,30);
	person a;
	cout<<a.m_a<<a.m_b<<a.m_c;
}
int main(){
	test01();
}
```

### 1.1.3类对象作为类成员(类中类)

注意：会先构造最里面的类的对象，先有phone，再有person



```c++
#include<bits/stdc++.h>
using namespace std;
class phone{
	public:
		phone(string name){
			p_name=name;
		}
		string p_name;
};
class person{
	public://phone m_phone=pname等同于phone m_phone(pname)
		person(string name,string pname):m_name(name),m_phone(pname){
			
		}
		string m_name;
		phone m_phone;
};
void test01(){
	//person a(10,20,30);
	person a("Tom","Apple");
	cout<<a.m_name<<a.m_phone.p_name;//类中类🤭
}
int main(){
	test01();
}
```

### 1.1.4静态成员

静态成员变量：变量在公共public区域，所有对象共享同—份数据（private区域外部不可以访问），在编译阶段分配内存（全局区），类内声明，类外定义

```c++
#include<bits/stdc++.h>
using namespace std;

class person{
	public:
		
		static int m_age;
};
//初始化
int person::m_age=10;//与全局变量区分，要加person：：作用域下 （类外定义）
void test01(){
	person a;
    a.m_age=20;
	cout<<a.m_age<<endl;//a.m_age改为20; 
	}

void test02(){//1、通过对象进行访问
	person p;
cout << p.m_age << endl;
//2、通过类名进行访问(静态变量特有，因为所有类共享一个数据，可以直接通过类访问这个数据，只需加上person：：的作用域)
cout << person ::m_age << endl ;
}

int main(){
	test01();
	test02();
}
```

静态成员函数：所有对象共享同一个函数（公共区域共享，私有区域无法访问），静态成员函数只能访问静态成员变量

```c++
#include<bits/stdc++.h>
using namespace std;

class person{
	public:static void func(){
		cout<<"static void func调用";
		age=20;//静态成员函数可以访问静态成员变量
		cout<<age;
		//height=10;//静态成员函数不可以访问非静态成员变量，无法区分到底是哪个对象的


	}
	static int age;
	//int height;
};
 int person:: age=10;

void test01(){
	
	}

void test02(){//1、通过对象进行访问
	person p;
	p.func();
//2、通过类名进行访问
	person::func() ;
}

int main(){
	test01();
	test02();
}
```

### 1.1.5 成员对象和成员函数分开存储

只有非静态成员对象存储在类里面，其余如静态成员对象、静态/非静态成员函数都不存储在类内。

空对象占用的字节：1字节

### 1.1.6this指针

this用途：1.解决名称冲突
				  2.返回对象本身用*this



```c++
#include<bits/stdc++.h>
using namespace std;

class person{
	public:
		
		person(int age){
			this->age=age;//this指针指向被调用成员函数所属的对象（p） 
		}
		void personaddperson(person &b){
			this->age+=b.age;//this指针指向a 
		}
		int age;
};


void test01(){
	person p(18);
	cout<<p.age<<endl;
	}
void test02(){
	person a(12);
	
	person b(20);
	a.personaddperson(b);
	cout<<a.age<<endl;
	
} 


int main(){
	test01();
	test02();
}
```

如果人a年龄一直加b的年龄的话，返回值需要改成*this（this为指针，指向person，返回类型需要改成person&（如果不用引用的方式返回，相当于返回与p2不同的另一个Person（只是age都是20），那么后续的加年龄操作与p2就没有关系了）

```c++
#include<bits/stdc++.h>
using namespace std;

class person{
	public:
		
		person(int age){
			this->age=age;//this指针指向被调用成员函数所属的对象（p） 
		}
		person& personaddperson(person &b){
			this->age+=b.age;//this指针指向a
			return *this;//*this为a本体 
		}
		int age;
};


void test01(){
	person p(18);
	cout<<p.age<<endl;
	}
void test02(){
	person a(12);
	
	person b(20);
	a.personaddperson(b).personaddperson(b).personaddperson(b);
	cout<<a.age<<endl;
	
} 


int main(){
	test01();
	test02();
}
```

注：this指针实质为指针常量，指向person不可更改，相当于person * const this；但是this指向的值可以更改

```c++
...
public: void showperson(){
	this->m_age=100;//this指向的值可以更改
}
int m_age;
...
```

如果想要this指向的值不可更改，保护数据的话

```c++
void showperson() const {
}//常函数：在成员函数后加const，使this指向的值不可更改
```

常对象：const person a；（对象里的值不可修改）

![image-20220222143329112](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220222143329112.png)

## 1.2函数重载

作用：函数名可以相同，提高复用性

函数重载条件：
（1）.作用域相同
（2）.函数名相同
（3）.函数参数类型不同或者个数不同或者顺序不同

```c++
#include<bits/stdc++.h>
using namespace std;
void func(){
	cout<<"a";
}
void func(int a){
	cout<< "abc";
}
void func(double a){
	cout<< "abcde";
}
int main(){
	func(2);
}
```

注意：函数返回值不可以作为重载条件

## 1.3 运算符重载

运算符重载概念:对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

### 1.2.1加号运算符重载

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	public:
        //成员函数重载
		person operator+(person &p){//这个是成员函数，不是构造函数
			person temp;
			temp.m_a=this->m_a+p.m_a;
			temp.m_b=this->m_b+p.m_b;
			return temp;
		}
	int m_a;
	int m_b; 
}; 
void test01(){
	person a;
	a.m_a=10;
	a.m_b=20;
	person b;
	b.m_a=10;
	b.m_b=20;
	person c=a+b;//本质为person c=a.operator+(b);
	cout<<c.m_a<<' '<<c.m_b;
}
int main(){
	test01();
}
```

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	friend person operator+(person &p1,person &p2);
	public:
	person(){
		
	}
	person(int a,int b){
		m_a=a;
		m_b=b;
	}
	void show(){
		cout<<m_a<<" "<<m_b<<endl;
	}
	private:
		
	int m_a;
	int m_b; 
}; 
//全局函数重载
person operator+(person &p1,person &p2){
			person temp;
			temp.m_a=p1.m_a+p2.m_a;
			temp.m_b=p1.m_b+p2.m_b;
			return temp;
		}
void test01(){
	person a(10,34);
	person b(22,14);
	
	person c=a+b;//本质person c=operator+(a,b);
	c.show();
}
int main(){
	test01();
}
```

### 1.3.2右移运算符重载

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	public:
	int a=10;
	int b=20;
};
ostream & operator<<(ostream &cout,person &p){//注意：返回值也为引用类型（因为要构成链式结构，传入为cout本身，通过引用&起了个别名，返回依旧要为引用，方便下一轮传入依旧是引用，自始至终都为cout本身
	cout<<p.a;
	cout<<p.b;
	return cout;
}
int main(){
	person p;
	cout<<p<<"hello"<<endl;
}
```

设置为友元，可访问私有成员

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
    friend ostream & operator<<(ostream &cout,person &p);
	private:
	int a=10;
	int b=20;
};
ostream & operator<<(ostream &cout,person &p){//注意：返回值也为引用类型（因为要构成链式结构，传入为cout本身，通过引用&起了个别名，返回依旧要为引用，方便下一轮传入依旧是引用，自始至终都为cout本身
	cout<<p.a;
	cout<<p.b;
	return cout;
}
int main(){
	person p;
	cout<<p<<"hello"<<endl;
}
```

1.3.3加加运算符重载

```c++
#include<bits/stdc++.h>
using namespace std;
class person{
	
    
	friend ostream & operator<<(ostream &cout,person &p);//若需后置则第二个参数去掉引用，因为后置返回无引用
	public:
	
	//前置
	person & operator++(){//先加一再返回
		this->a++;
		return *this;
	}
    //后置
    //int为占位参数，区分前置（因为通过返回值不可以作为区分条件）
	person operator++(int){//先返回(需保留,最后返回)再加一
		person b=*this;
		a++;
		return b;
	} 
	private:
	int a=10;
};

ostream & operator<<(ostream &cout,person &p){//注意：返回值也为引用类型（因为要构成链式结构，传入为cout本身，通过引用&起了个别名，返回依旧要为引用，方便下一轮传入依旧是引用，自始至终都为cout本身
	cout<<p.a;
	return cout;
}
int main(){
	person p;
	cout<<++p<<"hello"<<endl;
}
```



## 1.4友元

friend:可以访问私有属性

**1.全局函数（外部）作友元**

```c++
#include<iostream>
using namespace std;
class person{
friend void test(person *person);//声明前加一个friend
	public:
	
	string name;
	person(){
		name="asc";
		age=1;
	}
	private:
	int age;
};
void test(person *person){//依托于main函数构造的person p
	
	cout<<person->name<<endl;
	cout<<person->age<<endl;//可以访问私有属性
}
int main(){
	person p;
	test(&p);
}
```

附：若用作用域的方法可直接访问私有成员。

```c++
#include<iostream>
using namespace std;
class person{
 
	public:
	 void test(person *person);
	string name;
	person(){
		name="asc";
		age=1;
	}
	private:
	int age;
};
void person::test(person *person){
	
	cout<<person->name<<endl;
	cout<<person->age<<endl;
}
int main(){
	person p;
	p.test(&p);
}
```

**2.类作友元**

```c++
#include<iostream>
using namespace std;
class Building{
friend class GoodGay;//类作友元方法
	public:
	Building(){
		livingroom = "客厅";
		bedroom = "卧室"; 
	}
	
	string livingroom;
	private:
		string bedroom;
};
class GoodGay{
	public:
		GoodGay(){
			
		}
	void Good(Building mybuilding){
			building=&mybuilding;
		}
		void visit(){
		cout<<building->livingroom<<endl;
		cout<<building->bedroom<<endl;
	}
	private:Building * building;
};

int main(){
	Building a;
	GoodGay ta;
	ta.Good(a);
	ta.visit();
}
```

注意：Building类需放在GoodGay类前面，因为后面GoodGay在建立时需要建立Building

以下为错误的

```c++
#include<iostream>
using namespace std;
class Building;
class GoodGay{
	public:
		GoodGay(){
			
		}
	void Good(Building mybuilding){//创建了一个mybuilding对象，但不知道其中成员与属性，
			building=&mybuilding;
		}
		void visit(){
		cout<<building->livingroom<<endl;
		cout<<building->bedroom<<endl;
	}
	private:Building * building;
};
class Building{
friend class GoodGay;
	public:
	Building(){
		livingroom = "客厅";
		bedroom = "卧室"; 
	}
	
	string livingroom;
	private:
		string bedroom;
};
int main(){
	Building a;
	GoodGay ta;
	ta.Good(a);
	ta.visit();
}
```

![image-20220415100757686](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220415100757686.png)

[基类出现 “has incomplete type”是啥问题_百度知道 (baidu.com)](https://zhidao.baidu.com/question/1541225760334430467.html)

**3.成员函数作友元**

错误：

```c++
#include<iostream>
using namespace std;
class GoodGay；
class Building{
	friend void GoodGay::visit1();//非法使用GoodGay类，因为不知道GoodGay的具体成员函数
	public:
	
		Building(){
			livingroom = "客厅";
			bedroom = "卧室"; 
		}
		
	string livingroom;
	private :string bedroom;
};

class GoodGay{
	public:
		GoodGay(){
			building=new Building;//单纯的把Building和GoodGay类互换也是行不通的，因为GoodGay也需要用到Building具体实现，用new创造出来一个Building类
		}
		void visit1(){
			cout<<building->bedroom<<endl;
			cout<<building->livingroom<<endl;
		}
	private:Building *building;
};
int main(){
	GoodGay Jack;
	Jack.visit1();
}
```

[C++ 成员函数做友元遇到的问题 （错误C2027:使用了未定义类型）_纯白棒球帽的博客-CSDN博客_使用了未定义类型](https://blog.csdn.net/chuomei5332/article/details/109675366)、

**在声明之后，定义之前，此类是一个不完全类型(incompete type)，即已知向前声明过的类是一个类型，但不知道包含哪些成员。**
**不完全类型只能以有限方式使用，不能定义该类型的对象，不完全类型只能用于定义指向该类型的指针及引用，或者用于声明(而不是定义)使用该类型作为形参类型或返回类型的函数。**

解决方法：类内声明，类外实现成员函数（在等前面类都定义好后，后面再实现无障碍）

```c++
#include<iostream>//而且这种方式只能是GoodGay在前面，因为只有GoodGay类里创建的Building对象可以放到成员函数里去类外实现，而Building的友元声明必须放在类内里，这意味着必须放在后面。
using namespace std;

class Building;
class GoodGay{
	public:
		GoodGay();
		void visit1();
	private:Building *building;
};

class Building{
	friend void GoodGay::visit1();
	public:
	
		Building(){
			livingroom = "客厅";
			bedroom = "卧室"; 
		}
		
	string livingroom;
	private :string bedroom;
};
GoodGay::GoodGay(){
		building=new Building;
}
void GoodGay::visit1(){
			cout<<building->bedroom<<endl;
			cout<<building->livingroom<<endl;
		}
int main(){
	GoodGay Jack;
	Jack.visit1();
}
```

## 1.5继承

继承：减少重复代码

语法：class 子类 ：继承方式 父类

**类的访问权限**
类的访问权限有三种：

public 公共权限： 可以被该类中的函数、子类的函数、其友元函数访问,也可以由该类的对象访问
protected 保护权限： 可以被该类中的函数、子类的函数、以及其友元函数访问,但不能被该类的对象访问
private 私有权限：只能由该类中的函数、其友元函数访问,不能被任何其他访问，该类的对象也不能访问。
**三种权限的区别：**

public:可以被任意实体访问
protected:只允许本类及**子类**的成员函数访问
private:只允许本类的成员函数访问

继承方式：
公共继承：父类到子类的权限不发生改变
保护继承：父类的public和protect变为子类的protect，private不变
私有继承：父类所有权限全变为子类的private![image-20220427154229624](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220427154229624.png)



父类中所有成员（包括私有成员）都会被继承到子类

**继承中父子类同名成员/成员函数调用规则：子类直接调用
													                 父类需要加作用域**

```c++
#include<iostream>
using namespace std;
class fa{
	public:int a=11;
	void sx(){
		cout<<"fanb"<<endl;
	}
	private:int b=2;
}; 
class son:public fa{
	public:int a=10;
	void sx(){
		cout<<"sonnb"<<endl;
	}
	private:int c=21;
};
int main(){
	son wo;
	wo.sx();
	cout<<wo.a<<endl;//重点
	cout<<wo.fa::a<<endl;//重点
	wo.sx();
	wo.fa::sx();
}
```

注意：基类的构造函数（无论是默认构造还是有参构造，只要基类又出现）都需要在派生类中通过初始化列表的形式进行初始化![image-20220614212917972](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220614212917972.png)![image-20220614212922975](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220614212922975.png)

![image-20220614212820671](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220614212820671.png)



**多继承**

语法：class 子类 ：继承方式 父类，继承方式 父类···

## 1.6多态

**多态：同一接口的不同实现方式**

静态多态：函数重载与运算符重载 在编译时地址已确定，
动态多态：虚函数（virtual） 在定义时地址才确定

多态满足的条件：
（1）继承
（2）子类重写父类的虚函数

重写：指函数名，函数类型，传入值完全相同

多态使用：父类指针或引用指向子类对象

```c++
#include<iostream>
using namespace std;
class animal{
	public:
		virtual void speak(){
			cout<<"动物在说话"<<endl;
		}
};
class sheep : public animal{
	public:
		void speak(){
			cout<<"羊在说话"<<endl;
		}
};
class horse : public animal{
	public:
		void speak(){
			cout<<"马在说话"<<endl;
		}
};
//引用写法
void test(animal &animal){
	animal.speak();//如果想让羊或者马说话，就不能提前确定函数的地址，就需要用到virtual，在运行时才确定函数地址
}
int main(){
	sheep a;
	horse b;
	test(a);
	test(b);
} 
//指针写法
int main(){
	animal* a=new sheep;
	a->speak();
	delete a;
	a=new horse;
	a->speak();
	delete a;
} 
```

[黑马程序员匠心之作|C++教程从0到1入门编程,学习编程不再难_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1et411b73Z?p=136)P136

**底层原理：vfptr**

v-virtual
f-function
ptr-pointer
vfptr-虚函数指针 指向虚函数表（记录虚函数的地址）

当猫类没有重写时，其实就是继承（将父类所有的东西，包括指针继承到子类）![image-20220428113858574](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220428113858574.png)

**当猫类发生重写后，子类的虚函数表里的地址改为猫类的地址**

![image-20220428114644839](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220428114644839.png)

多态好处：代码结构清晰，可由基础的类扩展出上层的类，而且维护时不用整个代码一起读，而是找到出错的类进行修改，提高效率

例子计算器

```c++
//普通版
#include<iostream>
using namespace std;
class normalcal{
	public:
	int add(string s){
		if(s=="+")cout<<a+b<<endl;
		if(s=="-")cout<<a-b<<endl;
		if(s=="*")cout<<a*b<<endl;
	}
	int a;
	int b;
};
int main(){
	normalcal my;
	my.a=10;
	my.b=10;
	my.add("+");
}
```

```c++
//多态版
#include<iostream>
using namespace std;
class pluscal{
	public:
	virtual void yunsuan(){//需要在父类构造基础函数，子类才能重写
		;
	}
	int a;
	int b;
};
class addcal :public pluscal{
	public:
	void yunsuan(){
		cout<<a+b<<endl;
	}
};
class minuscal :public pluscal{
	public:
	void yunsuan(){
		cout<<a-b<<endl;
	}
};
class multiplycal :public pluscal{
	public:
	void yunsuan(){
		cout<<a*b<<endl;
	}
};
int main(){
	pluscal *a=new addcal;
	a->a=10;
	a->b=10;
	a->yunsuan();
}
```

```c++
class pluscal{
	public:
	virtual void yunsuan(){//这一条代码可以变为纯虚函数，因为无意义
		;
	}
	int a;
	int b;
};
```

```c++
class pluscal{
	public:
	virtual void yunsuan() = 0;//纯虚函数
	int a;
	int b;
};
```

纯虚函数：父类简化

含有纯虚函数的类叫做抽象类

注：抽象类不能构造对象，并且子类必须重写

**在栈区创建静态对象不用delete，系统会自动销毁**
**在类外创建动态对象时（堆区），直接在类外手动delete**
**在类内创建动态对象时（堆区），析构函数会自动delete**

**析构函数补充：**

在哪些情况下会程序会执行析构函数？

1. 如果一个函数中定义了一个对象，当这个函数被调用结束时，对象应该释放，在这个对象释放前自动执行析构函数
2. 静态（static）局部对象在函数调用结束时对象并不释放，因此也不调用析构函数，只有在main函数结束或者调用exit函数结束程序时，才调用static局部对象的析构函数
3. 如果定义了一个全局对象，则在程序的流程离开其作用域时（main函数结束或者调用exit函数结束程序），调用其全局对象的析构函数
4. 如果用new运算符动态的建立了一个对象，当用delete运算符释放对象时，先调用该对象的析构函数

**类中如果没有涉及到资源管理时，析构函数是否给出无所谓；但是如果涉及到资源管理，用户必须要显式给出析构函数，在析构函数中清理对象的资源（如在类内创建动态对象）**



虚析构的原因：多态使用时，如果子类创建动态对象，释放父类指针时无法调用子类的析构函数，导致内存泄漏

则需要虚析构：在父类的析构函数前加上virtual

```c++
#include<iostream>
using namespace std;
class animal{
	public:
	virtual void speak() = 0;
	virtual ~animal(){//
		;
	}
};
class sheep:public animal{
	public:
	sheep(string myname){
		name=new string(myname);
	}
	void speak(){
		cout<<*name<<"在说话"<<endl; 
	}
	~sheep(){//因为类内构造了一个动态对象，堆区的*name，所以要在析构函数中手动释放
		if(name!=NULL){
			delete name;
			name=NULL;
		}
	}
	string *name;
};
int main(){
	animal* tom=new sheep("Tom");
	tom->speak();
	tom->~animal();
}
```

例子 组装电脑

```c++
#include<iostream>
using namespace std;
class cpu{
	public:virtual void name()=0;
}; 
class card{
	public:virtual void name()=0;
};
class computer{
	public:
	computer(){
		;
	}
	computer(cpu* a,card* b){
		mycpu=a;
		mycard=b;
	}
	void work(){
		mycpu->name();
		mycard->name();
	}	
		
	private:
	cpu* mycpu;
	card* mycard;
};
class novacpu:public cpu{
	public:
	void name(){
		cout<<"使用novacpu"<<endl; 
	}
};
class lemoncard:public card{
	public:
	void name(){
		cout<<"使用lemoncard"<<endl; 
	}
};
int main(){
	cpu* a=new novacpu;
	card* b=new lemoncard;
	computer mycomputer(a,b);
	mycomputer.work();
	delete a;
	delete b;
}
```

或者computer也动态创建

```c++
#include<iostream>
using namespace std;
class cpu{
	public:virtual void name()=0;
}; 
class card{
	public:virtual void name()=0;
};
class computer{
	public:
	computer(){
		;
	}
	computer(cpu* a,card* b){
		mycpu=a;
		mycard=b;
	}
	void work(){
		mycpu->name();
		mycard->name();
	}	
		
	private:
	cpu* mycpu;
	card* mycard;
};
class novacpu:public cpu{
	public:
	void name(){
		cout<<"使用novacpu"<<endl; 
	}
};
class lemoncard:public card{
	public:
	void name(){
		cout<<"使用lemoncard"<<endl; 
	}
};
int main(){
	cpu* a=new novacpu;
	card* b=new lemoncard;
	computer *mycomputer1=new computer(a,b);
	mycomputer1->work();
	delete mycomputer1;
	delete a;
	delete b;
}
```

## 1.7文件

### 1.7.1写

**1.头文件fstream**
**2.创建一个文件流**
**ofsteam ofs;**
**3.打开文件**
**ofs.open(路径，打开方式);**
**4.输入文本**
**ofs<<"adaw"<<endl**
**5.关闭文件**
**ofs.close();**

```c++
#include<iostream>
#include<fstream>
using namespace std;
int main(){
	ofstream ofs;
	ofs.open("text.txt",ios::out);
	ofs<<"ASdaw"<<endl;
	ofs.close();
}
```

### 1.7.2读

1.#<fstream>
2.ifsream ifs
3.ifs.open(文件,ios::open)
4.string s;while(ifs>>s){
cout<<s<<endl;
}
5.ifs.close();

## 1.8define/头文件/源文件

![image-20220523081727471](C:\Users\Tom\AppData\Roaming\Typora\typora-user-images\image-20220523081727471.png)

**#pragma once是一个比较常用的C/C++杂注，只要在头文件的最开始加入这条杂注，就能够保证头文件只被编译一次。**

#ifndef的方式依赖于宏名字不能冲突，这不光可以保证同一个文件不会被包含多次，也能保证内容完全相同的两个文件不会被不小心同时包含。当然，缺点就是如果不同头文件的宏名不小心"撞车"，可能就会导致头文件明明存在，编译器却硬说找不到声明的状况

#pragma once则由编译器提供保证：同一个文件不会被包含多次。注意这里所说的"同一个文件"是指物理上的一个文件，而不是指内容相同的两个文件。带来的好处 是，你不必再费劲想个宏名了，当然也就不会出现宏名碰撞引发的奇怪问题。对应的缺点就是如果某个头文件有多份拷贝，本方法不能保证他们不被重复包含。

**#include的本质是拷贝文件，将文件的所有内容拷贝到源文件中，在预编译中完成，会将资源文件中的ascii值转换为相应的值（通过atoi函数）。**

使用尖括号的话，编译时会先在系统include目录里搜索，如果找不到才会在源代码所在目录搜索；使用双引号则相反，会先在源代码目录里搜索。这就意味着，当系统里（如/usr/include/里）有一个叫做math.h的头文件，而你的源代码目录里也有一个你自己写的math.h头文件，那么使用尖括号时用的就是系统里的；而使用双引号的话则会使用你自己写的那个。
假如你在a.h中inlude了b.h，b.h中inlude了a.h，没有这句的话，就会像套娃一样循环包含。

# 2.模板

## 2.1模板格式

template<typename T>定义T为新的数据类型

使用模板方法：

1.自动转换
int a;int b;
void swap（a，b）;

2.显式转换
int a;int b;
void swap<int>(a,b);

```c++
#include<iostream>
using namespace std;
template <typename T>
void wswap(T &a,T &b){
	T temp = a;
	a=b;
	b=temp;
}
int main(){
	int a=10;
	int b=219;
	wswap<int>(a,b);
	cout<<a<<endl;
	cout<<b<<endl;
} 
```

注意：模板必须确定T的数据类型，才能调用

```c++
#include<iostream>
using namespace std;
template <typename T>
	void print(){
		cout<<"调用成功"<<endl;
	}
int main(){
	print<int>();
}
```

## 2.2普通函数与模板区别

普通函数可以发生隐式转换

模板的显示类型转换可以发生隐式转换
而自动推导类型不能发生隐式转换

## 2.3优先级

1、普通函数>模板
2、若要强制调用模板，则需要空模板< >
3.模板同样也可以重载

```c++
#include<iostream>
using namespace std;

void te(){
	cout<<"调用普通函数"<<endl;
}
template <typename T>
void te(T a){
	cout<<"调用模板"<<endl;
}
template <typename T>
void te(T a,T b){
	cout<<"调用重载模板"<<endl;
}

int main(){
	te();
	te<>(10);
	te<>(10,20);
} 
```

## 2.4模板对于自定义对象

模板T不能直接代替自定义对象，需要对模板重载

template<>

```c++
#include<iostream>
using namespace std;
class person{
	public:
		string name;
		int age;
};
template<typename T>
bool equal(T &a,T &b){
	if(a==b)return true;
	else return false;
}
template<>bool equal(person &a,person &b){
	if(a.age==b.age&&a.name==b.name)return true;
	else false;
}
int main(){
	person a;
	person b;
	a.age=10;
	a.name="tom";
	b.age=20;
	b.name="qpod";
	cout<<equal(a,b);
}
```

## 2.5类模板

类模板：所谓类模板，实际是建立一个**通用类**，其**数据成员，成员函数**的**返回类型和形参类型**不具体指定，用一个虚拟的类型来代表。使用类模板定义对象时，系统会根据**实参的类型来取代类模板中虚拟类型**从而实现了不同类的功能

template <类型参数表>
class 类模板名{
  成员函数和成员变量
};

```c++
#include<iostream>
using namespace std;
template<typename agetype,typename nametype>
class animal{
	public:
	animal(agetype oage,nametype oname){
		age=oage;
		name=oname;
	}
	void speak(){
		cout<<age<<" "<<name;
	}
	
	agetype age;
	nametype name;
};
int main(){
	animal <int ,string>a(10,"aij");//类模板必须显式转换，不能自动推导
	a.speak();
}

```

**类模板作函数对象**

1.指定传入类型

```c++
#include<iostream>
using namespace std;

template<typename T1,typename T2>
class animal{
	public:
		animal(T1 oage,T2 oname ){
			age=oage;
			name=oname;
		}
		T1 age;
		T2 name;
};
void speak(animal<int,string>&a){
	cout<<a.age<<endl;
	cout<<a.name<<endl;
}
int main(){
	animal<int,string> a(10,"xci");
	speak(a);
}
```

## 2.6类模板与继承

子类必须知道父类的数据类型才能继承

```c++
#include<iostream>
using namespace std;

template<typename T1>
class animal{
	public:
		T1 type;
	void speak(){
		cout<<type<<endl;
	}
};
class sheep:public animal<string>{//指定父类类型
	public:
	sheep(string name){
		type=name;
	}
	
};
int main(){
	sheep a("kso");
	a.speak();
}
```

改进：如果想要灵活指定父类类型，需要子类也变为类模板

[(1条消息) 模板类的继承总结_慢慢的燃烧的博客-CSDN博客_模板类继承](https://blog.csdn.net/u010164190/article/details/72729824)

```c++
template<class T>
class Base
{
    T v1;
};

class Child : public Base<int>    //指定T的类型
{

};

template<class T>
class Child1 : public Base<T>    //灵活指定
{

};

template<class T, class T1>
class Child1 : public Base<T>    //灵活指定
{
    T1 v2;
};
```

**类模板的类外实现**

```c++
#include<iostream>
using namespace std;
template <typename T1,typename T2>
class animal{
	public:
		T1 age;
		T2 name;
		animal(T1 age,T2 name);
		void speak();
};
template <typename T1,typename T2>//注意：模板需要添加，因为下面传进来参数用到T1，T2
animal<T1,T2>::animal(T1 age,T2 name){//注意：1.作用域 2.模板的参数列表
			this->age=age;
			this->name=name;
		}
template <typename T1,typename T2>
void animal<T1,T2>::speak(){
	cout<<age<<endl;
	cout<<name<<endl;
}
int main(){
	animal <int,string>dog(10,"opo");
	dog.speak();
}
```

**分文件编写**：.h文件和.cpp文件合成一个hpp文件

原因因为按原来的方式main.cpp里有头文件class.h，而类模板里虽然有声明，但是不会创建成员函数（类模板的成员函数只有在调用时才会创建），不会找这成员函数，相当于从没有看见过class.cpp，所以在class.cpp函数不会实现，所以main.cpp在调用时报错

解决：

将cpp文件内容剪贴到.h文件中并更名为.cpp文件

## 2.7类模板与友元

请先复习1.4友元

全局函数（普通函数，不是成员函数）作友元，可以访问到类内私有成员

**1.类内实现**

```c++
#include<iostream>
using namespace std;

template<typename T1,typename T2>
class animal{
	 public:
	
	animal(T1 oname,T2 oage){
		age=oage;
		name=oname;
	}
	friend void speak(animal<T1,T2>animal){
		cout<<animal.name<<" "<<animal.age;
	}
	private:
	T1 name;
	T2 age;
}; 
int main(){
	animal <string ,int >a("ija",10);
	speak(a);
}
```






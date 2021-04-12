C语言基础知识点整理
-----------

1、变量
----

- [全局变量]：保存在内存的全局存储中，占用__静态__的存储单元。
- [局部变量]：局部变量保存在__栈__中，只有所在函数被调用时才动态分配存储空间。
- [栈]:由编译器进行管理，自动分配和释放，类似于数据结构中的栈。
- [堆]:用于程序动态申请分配和释放空间。C语言中的*malloc*和*free*,C++中的*new*和*delete*均是在堆中进行的。正常情况下，程序员申请的空间在使用结束后需要释放，否则程序结束后系统自动回收。
- [全局静态储存区]：分为DATA和BSS段。DATA段(全局初始化区)存储__初始化__的全局变量和静态变量。BSS段(全局未初始化区)存储__未初始化__的全局变量和静态变量。
- [文字常量区]：存放常量字符串，程序结束后系统__自动释放__。
- [程序代码区]：存放程序的**二进制代码**。

常量定义：
--------
  
**宏定义**

**#define**	

	#define MAX 10000;
	预处理阶段展开, 不带类型的常数，不做类型检查，不会分配内存。
	宏替换只做替换，不做计算，不做表达式求解。

**与typedef 的区别**  

	typedef 实质上是一个存储类的关键字
	typedef仅限定为类型定义符号名称。如int/float等
	typedef由编译器编译，而#define由预编译器编译

**const 定义**
	 
	const int var = 5;//必须一次性定义到具体的值
	const 定义的数严格意义上不是常量，是不允许改变的常变量。需要做类型检查，会在内存中分配。
	编译期间，一般不为普通const变量分配内存空间，而是保存在符号表中，

**例子**

	#define NUM 3.14159 //常量宏
	const doulbe Num = 3.14159; //此时并未将Pi放入ROM中 ......
	double i = Num; //此时为Pi分配内存，以后不再分配！
	double I= NUM; //编译期间进行宏替换，分配内存
	double j = Num; //没有内存分配
	double J = NUM; //再进行宏替换，又一次分配内存！

存储类
-----

**auto**

	{
		int mount;
		auto int mount; //auto只能用在函数内修饰局部变量。
	}
**register**
	
	定义存储在寄存器中的局部变量，这样定义只意味着可能存储在寄存器中，取决于硬件和实现的限制；
	只有局部自动变量和形参可以做寄存器变量。
	{
 		register int miles;//用于需要快速访问的变量
	}

**static**
	
	在程序生命周期内保持局部变量的存在，而不需要在每次进入和离开作用域时销毁和创建，
	也可以使用static 修饰全局变量，作用域会限制在它声明的文件内。
	#include <stdio.h>
	/* 函数声明 */
	void func1(void);
	 
	static int count=10;        /* 全局变量 - static 是默认的 */
	 
	int main()
	{
	  while (count--) {
	      func1();
	  }
	  return 0;
	}
	 
	void func1(void)
	{
	/* 'thingy' 是 'func1' 的局部变量 - 只初始化一次
	 * 每次调用函数 'func1' 'thingy' 值不会被重置。
	 */                
	  static int thingy=5;
	  thingy++;
	  printf(" thingy 为 %d ， count 为 %d\n", thingy, count);
	}
	结果为：
	 thingy 为 6 ， count 为 9
	 thingy 为 7 ， count 为 8
	 thingy 为 8 ， count 为 7
	 thingy 为 9 ， count 为 6
	 thingy 为 10 ， count 为 5
	 thingy 为 11 ， count 为 4
	 thingy 为 12 ， count 为 3
	 thingy 为 13 ， count 为 2
	 thingy 为 14 ， count 为 1
	 thingy 为 15 ， count 为 0

**extern**
	
	提供全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用extern时，
	对于无法初始哈的变量，会指向之前定义过的存储位置。
	extern用于在多个文件中共享全局变量或者函数
	main.c
	#include <stdio.h>
	int count ;
	extern void write_extern();
	 
	int main()
	{
	   count = 5;
	   write_extern();
	}
	
	support.c
	#include <stdio.h>
	extern int count;
	void write_extern(void)
	{
	   printf("count is %d\n", count);
	}

	$ gcc main.c support.c
	result:
	count is 5

运算符
-----
- 算数运算符
- 逻辑运算符
- 关系运算符
- 位运算符
- 赋值运算符
- 杂项运算符

**位运算符**  
	
	整型变量的交换：
	a=a^b;
	b=a^b; //b=(a^b)^b = a^(b^b) = a 
	a=a^b; //a=(a^b)^a = (a^a)^b = b
	
	2的幂指数
	((num > 0) && ((num & (num - 1)) == 0))

枚举类型
----
C语言中枚举类型被当做是`int`型或者`unsigned int`型，（定义其他类型会报错+）简单来讲，枚举类型中的每一个元素可以看作是一个个宏定义变量。枚举定义不会占用内存，只有枚举变量会占用内存。

函数的指针
---

	#include <stdio.h>
	int fun1(int,int);
	int fun1(int a, int b){
	    return a+b;
	}
	/* 要调用上面定义函数的主函数 */
	int main (){
    int (*pfun1)(int,int);
    pfun1=fun1;//这里&fun1和fun1的值和类型都一样，用哪个无所谓
    int a=(*pfun1)(5,7); //通过函数指针调用函数。
    printf("%d\n",a);
    int e = fun1(5,7);
    printf("%d\n",e);
    int b = (&fun1)(5,7);
    printf("%d\n",b);
    int c = (*fun1)(5,7);
    printf("%d",c);
    return 0;
	}
	//根据关系 fun1==*&fun1==fun1==&fun1可知，以上的运行结果会得到4个5+7。
	//因此在下面的函数指针数组实例中，action[2]()就相当于这里的(&fun1(5,7))，这点务必搞清楚。

回调函数
---
回调函数的最大作用在于解耦合，降低代码之间的互相依赖
	
	#include <stdlib.h>  
	#include <stdio.h>
	 
	// 回调函数
	void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
	{
	    for (size_t i=0; i<arraySize; i++)
	        array[i] = getNextValue();
	}
	 
	// 获取随机值
	int getNextRandomValue(void)
	{
	    return rand();
	}
	 
	int main(void)
	{
	    int myarray[10];
	    /、getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针
	    populate_array(myarray, 10, getNextRandomValue);
	    for(int i = 0; i < 10; i++) {
	        printf("%d ", myarray[i]);
	    }
	    printf("\n");
	    return 0;
	}

字符串
-----

*strlen* 与 *sizeof*的区别：
*strlen* 是函数，*sizeo*f 是运算操作符，二者得到的结果类型为`size_`t，即 `unsigned int` 类型。
*sizeof* 计算的是变量的大小，不受字符 \0 影响；
而 *strlen* 计算的是字符串的长度，以 \0 作为长度判定依据。

结构体
----
结构体数组的初始化
与其它类型数组一样，对结构体数组可以初始化如：

	struct student
	{
	    int mum;
	    char name[20];
	    char sex;
	    int age;
	    float score;
	    char addr[30];//最宽类型成员为float or int ，其宽度为4字节,所以整个结构体总大小为4的整数倍
	}stu[3] = {{10101,"Li Lin", 'M', 18, 87.5, "103 Beijing Road"},
	            {10101,"Li Lin", 'M', 18, 87.5, "103 Beijing Road"},
	            {10101,"Li Lin", 'M', 18, 87.5, "103 Beijing Road"}};

结构体变量的首地址能够被其最宽基本类型成员的大小所整除。
结构体每个成员相对于结构体首地址的偏移量(offset)都是成员大小的整数倍，如有需要编译器会在成员之间加上填充字节(internal adding)。即结构体成员的末地址减去结构体首地址(第一个结构体成员的首地址)得到的偏移量都要是对应成员大小的整数倍。
结构体的总大小为结构体最宽基本类型成员大小的整数倍，如有需要编译器会在成员末尾加上填充字节。

共用体
----

	union Data
	{
	   int i;
	   float f;
	   char  str[20];
	} data;
	共用体变量所占的内存长度等于最长的成员变量的长度。例如上述的共用体Data各占20个字节

**共用体作用：**节省内存，有两个很长的数据结构，不会同时使用，比如一个表示老师，一个表示学生，如果要统计教师和学生的情况用结构体的话就有点浪费了！用共用体的话，只占用最长的那个数据结构所占用的空间，就足够了！  
**应用场景**：通信中的数据包会用到共用体:因为不知道对方会发一个什么包过来，用共用体的话就很简单了，定义几种格式的包，收到包之后就可以直接根据包的格式取出数据。

位域
---
位域表示位(bit)数
位域只在结构体和共用体中使用
	struct 
	{
	    unsigned int age : 3;
	} Age;
	
	/*age 变量将只使用 3 位来存储这个值，如果您试图使用超过 3 位，则无法完成*/
	Age.age = 4;
	printf("Sizeof( Age ) : %d\n", sizeof(Age));
	printf("Age.age : %d\n", Age.age);
	
	// 二进制表示为 111 有三位，达到最大值
	Age.age = 7;
	printf("Age.age : %d\n", Age.age);
	
	// 二进制表示为 1000 有四位，超出，直接丢弃
	Age.age = 8;
	printf("Age.age : %d\n", Age.age);//实际结果为0

输入与输出
---

**标准I/O库 stdio.h**

	scanf(%m.nc/%d/%f...)，m是数据总长度,n是小数位数和printf()
	
	getchar()和putchar()//读取一个字符
	gets()和puts()读取一串字,gets()未指定缓冲区大小，容易导致溢出
	fgets(str,length,stdin)和fputs(str,length,stdout);


内联函数
---
	Inline int Max(int a, int b){
		return (x > y)?x:y
	}
	1.在内联函数内不允许使用循环语句和开关语句；
	2.内联函数的定义必须出现在内联函数第一次调用之前；
	3.类结构中所在的类说明内部定义的函数是内联函数。
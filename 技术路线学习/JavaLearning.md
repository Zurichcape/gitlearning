# Java学习笔记  

1. 虚方法
------

     //Salary是Employee的子类,两个类均有相同的checkMail()方法
     Salary s = new Salary("员工 A", "北京", 3, 3600.00);
     Emloyee e = new Salary("员工 B", "上海", 2, 2400.00);//
     System.out.println("使用 Salary 的引用调用 mailCheck -- ");
     s.checkMail();
     System.out.println("\n使用 Employee 的引用调用 mailCheck--");
     e.checkMail();
     两者调用各自的checkMail()方法时，在编译阶段，编译器使用Employee
     的checkMail()验证该语句，但在实际运行阶段，Java虚拟机(JVM)使用的是
     Salary中的checkMail()方法，以上整个过程被称作虚拟方法调用，该方法被叫做虚方法。

2. 泛型
------

**泛型的优点** 
 
    class Shape<T>{
	    private T t;
	    public void add(T t){
	        this.t = t;
	    }
	    public T get(){
	        return this.t;
	    }
    }

	
    1. 解决集合类型在运行期间类型转换异常，将类型检查提前到编译阶段，从而避免程序运行时才抛出异常。  
    2. 实现算法的复用，我们不用定义各种数据类型的类。

3. 序列化和反序列化
-----

1. 序列化就是一个对象被表示为一个字节序列，该字节序列包括对象的类型信息、对象的数据等。
2. 反序列化就是序列化对象写入文件之后，可以从文件中读取，并将其转化为对象类型，也就是根据序列化信息在内存中新建对象。
3. 一个类的对象序列化成功的条件：  
  该类必须实现java.io.Serializable接口  
  该类的所有属性必须是可序列化的

4. 多线程编程
-----

1. 

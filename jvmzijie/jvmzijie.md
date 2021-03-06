1. > javap -verbose Test.class >test.txt

2.编译过后：的字节码

~~~
Classfile /Users/liusonghuang/Desktop/Test.class
  Last modified 2018-10-19; size 546 bytes
  MD5 checksum 16af266ae7a5935518af685b8551f498
  Compiled from "Test.java"
public class Test
  minor version: 0
  major version: 49
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #5.#23         // java/lang/Object."<init>":()V
   #2 = Fieldref           #24.#25        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = Methodref          #26.#27        // java/io/PrintStream.println:(I)V
   #4 = Class              #28            // Test
   #5 = Class              #29            // java/lang/Object
   #6 = Utf8               <init>
   #7 = Utf8               ()V
   #8 = Utf8               Code
   #9 = Utf8               LineNumberTable
  #10 = Utf8               LocalVariableTable
  #11 = Utf8               this
  #12 = Utf8               LTest;
  #13 = Utf8               main
  #14 = Utf8               ([Ljava/lang/String;)V
  #15 = Utf8               args
  #16 = Utf8               [Ljava/lang/String;
  #17 = Utf8               a
  #18 = Utf8               I
  #19 = Utf8               b
  #20 = Utf8               c
  #21 = Utf8               SourceFile
  #22 = Utf8               Test.java
  #23 = NameAndType        #6:#7          // "<init>":()V
  #24 = Class              #30            // java/lang/System
  #25 = NameAndType        #31:#32        // out:Ljava/io/PrintStream;
  #26 = Class              #33            // java/io/PrintStream
  #27 = NameAndType        #34:#35        // println:(I)V
  #28 = Utf8               Test
  #29 = Utf8               java/lang/Object
  #30 = Utf8               java/lang/System
  #31 = Utf8               out
  #32 = Utf8               Ljava/io/PrintStream;
  #33 = Utf8               java/io/PrintStream
  #34 = Utf8               println
  #35 = Utf8               (I)V
{
  public Test();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 6: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LTest;

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=4, args_size=1
         0: iconst_1
         1: istore_1
         2: iconst_2
         3: istore_2
         4: iload_1
         5: iload_2
         6: iadd
         7: istore_3
         8: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
        11: iload_3
        12: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        15: return
      LineNumberTable:
        line 8: 0
        line 9: 2
        line 10: 4
        line 11: 8
        line 12: 15
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      16     0  args   [Ljava/lang/String;
            2      14     1     a   I
            4      12     2     b   I
            8       8     3     c   I
}
SourceFile: "Test.java"

~~~

## 代码优化

1.尽量使用基本类型而不是包装类型

> 编译器会进行Integer.valueOf()

2.尽量重用对象，不要循环创建对象

3.容器类初始化的时候指定长度

~~~
List list=new ArrayList(2) 
~~~



4.arrayList随机遍历快，LinkedList添加删除快

5.集合遍历尽量减少重复计算

~~~
for(int i=0,len=collection.size();i<len;i++)
~~~

6.使用Entry遍历Map

~~~
for(Map.Entry<String,String> entry:map.entrySet()){
    String key=entry.getKey();
    String value=entry.getValue();
}
~~~

7.不要手动调用System.gc()

8.及时消除过期对象的引用，防止内存泄漏

9.尽量使用局部变量，减少遍历的作用域

10.尽量使用ArrayList自己加同步而不使用Vector

11.尽量使用同步代码块而不是方法上加synchronized 

12.ThreadLocal缓存线程不安全的对象，SampleDateFormat

13.尽量延迟加载

14.尽量减少反射，加缓存

15.尽量使用连接池，线程池，对象池，缓存

16.及时释放资源，IO流，Socket，数据库连接。

17.慎用异常不要用抛异常表示正常的逻辑

18.String尽量少用正则表达式

> replace	replaceAll	
>
> split

19.输出日志使用不停的级别

20.日志中参数拼接使用占用福

~~~
log.info("orderId:"+order);    不推荐
log.info("orderId,{}",order);   推荐
~~~


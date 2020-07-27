## C++
<details>
  <summary>malloc/free 与 new/delete 之间的区别</summary>
  
  - new/delete 是 **C++ 关键字**，需要编译器支持；malloc/free 是**库函数**，需要头文件支持。
  - 使用 new 操作符申请内存分配时**无须指定内存块的大小**，编译器会根据类型信息自行计算；而 malloc 则需要**显式地指出所需内存块的大小**。
  - new 操作符内存分配成功时，**返回对象类型的指针**，类型严格与对象匹配，无须进行类型转换，故 new 是符合类型安全性的操作符；而 malloc 内存分配成功则是**返回 void 类型指针**，需要通过强制类型转换将 void* 指针转换成我们需要的类型。
  - new 内存分配失败时，**抛出 bac_alloc 异常**。malloc 分配内存失败时**返回 NULL**。
  -  new 会先调用 operator new 函数，**申请足够的内存**（通常底层使用 malloc 实现），然后**调用对应类型的构造函数**，初始化成员变量，最后返回自定义类型指针；delete 先**调用析构函数**，然后调用 operator delete 函数**释放内存**（通常底层使用 free 实现）；malloc/free 是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。
  - new 操作符从**自由存储区**（free store）上为对象动态分配内存空间；而 malloc 函数从**堆**上动态分配内存。自由存储区是 C++ 基于 new 操作符的一个抽象概念，凡是通过 new 操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C++ 使用 malloc 从堆上分配内存，使用 free 释放已分配的对应内存。自由存储区不等于堆。
  - operator new/operator delete **允许重载**，malloc/free **不允许重载**。
  > 参考：[经典面试题之 new 与 malloc 的区别](https://blog.csdn.net/nie19940803/article/details/76358673)，[new 与 malloc 的10点区别](https://www.cnblogs.com/shilinnpu/p/8945637.html)
  
</details>
<details>
  <summary>new 与 operator new 的区别（自问）</summary>

  - new(new operator) 是一个内置的**运算符**；operator new是一个**函数**。
  - new做的工作是调用 operator new(sizeof(A)) + 调用A::A() + 返回指针，即**分配内存+调用构造函数+返回指针**；而 operator new 仅仅做了**分配内存的工作**。
  > 参考：[C++ 内存分配（new, operator new）详解](https://blog.csdn.net/uestclr/article/details/51171025)，[一分钟理解 C++ new 和 operator new](https://zhuanlan.zhihu.com/p/113439671)

</details>
<details>
  <summary>编译过程中各阶段所作工作</summary>

  - **预处理-->编译-->汇编-->链接**。
  - 预处理（.c-->.i）：编译器将头文件编译进来，还有宏的替换。
  - 编译（.i->.s）：编译器主要做词法分析，语法分析，语义分析等工作，检查无错误后，将其翻译为汇编语言。
  - 汇编（.s-->.o）：汇编器将汇编语言翻译成目标机器指令，生成目标文件。
  - 链接（.o-->可执行文件）：链接器将有关的目标文件链接起来，转为可执行文件。
  > 参考：[程序编译的四个阶段](https://www.jianshu.com/p/539a712ed284)，[C++ 程序编译过程](https://zhuanlan.zhihu.com/p/45402323)

</details>
<details>
  <summary>编译过程中的编译阶段所作的工作（自问）</summary>
  
  - **词法分析-->语法分析-->语义分析**。
  - 词法分析：其任务是对源程序逐字扫描，从中识别出一个个“单词”，“单词”又叫做符号，他是程序语言的基本语法单位，如关键字，标识符，常数，运算符，分隔符等。
  - 语法分析：其任务是跟根据语言的规则将单词符号序列分解成语法单位，如“表达式”，“语句”，“程序”等。语法规则就是语法单位的构成规则，通过语法分析确定整个输入串能否构成一个语法上正确的程序。如果程序没有错误，语法分析后就能正确的构造出语法树，否则就会指出语法错误，并给出诊断。
  - 语义分析：其任务是对类型进行分析和检查，一般类型检查包括两点：类型载体及在其上的运算。如，整除取余运算符只能对整数使用，如果运算对象是浮点数就认为是类型错误。
  > 参考：[编译原理-编译过程概述](https://blog.csdn.net/GarfieldGCat/article/details/89200785)

</details>
<details>
  <summary>纯虚函数与虚函数</summary>
  
  - 包含纯虚函数的类称为抽象类。
  - 不能实例化含纯虚函的类。
  - 在纯虚函数的函数原型后加上`=0`代表该虚函数为纯虚函数。
  - 声明一个纯虚函数的目的是为了让派生类只继承函数接口；声明一个虚函数的目的是为了让抽象类继承该函数的接口和缺省实现。
  - 可以为纯虚函数提供定义，即可为其供应一份实现代码，调用它的唯一途径是使用时明确指出其 class 名称。例如：`a->base::test();`，其中`base`是一个抽象类，`a`是以`base`作为基类的派生类的一个实例，`test()`是在`base`内声明并定义的一个纯虚函数。
  > [参考：C++ 虚函数与纯虚函数的用法与区别](https://www.cnblogs.com/inception6-lxc/p/8597326.html)，[Effective C++ 条款34：区别接口继承和实现继承]()

</details>
<details> 
  <summary>int 类型的变量不赋予默认值，那么它的值是什么</summary>
  
  - 如果该变量处于静态存储区，即全局变量和静态变量存储的区域，那么变量会被默认初始化，其值置为0。
  - 如果该变量不在静态存储区，那么变量不会被初始化，其值为随机值。
  - 以上适用于所有内置类型的变量。
  > 参考：[C++ Primer 中“内置类型的默认初始化到底指什么](https://zhidao.baidu.com/question/745125691050893132.html)

</details>
<details>
  <summary>free 怎么保证能释放内存</summary>

</details>
<details>
  <summary>调用函数压栈，一层一层</summary>

</details>
<details>
  <summary>C++ 虚函数继承虚函数表</summary>

</details>

## 网络
<details>
  <summary>TCP 粘包拆包</summary>

</details>
<details>
  <summary>HTTP 与 HTTPS 的区别及实现过程</summary>

</details>
<details>
  <summary>HTTP 状态码</summary>

</details>
<details>
  <summary>socket 函数用法，IO 多路复用，bind</summary>

</details>
<details>
  <summary>浏览器输入 URL 直到显示发生了什么，哪些属于 TCP，哪些属于 UDP</summary>

</details>
<details>
  <summary>通信光缆被挖掘机挖断了，现在建立好的TCP的连接变成啥样，有进行四次挥手嘛 ：逐渐超时被动关闭</summary>

</details>

## 操作系统
<details>
  <summary>线程调用机制</summary>

</details>
<details>
  <summary>常见锁有哪些，区别，如何选择</summary>

</details>
<details>
  <summary>死锁</summary>

</details>
<details>
  <summary>共享内存在进程外如何访问，如何保证安全</summary>

</details>
<details>
  <summary>虚拟内存</summary>

</details>
<details>
  <summary>fork 功能</summary>

</details>
<details>
  <summary>中断过程，如何保存现场</summary>

</details>
<details>
  <summary>用户态与内核态的区别</summary>

</details>
<details>
  <summary>线程与进程的区别</summary>

</details>

## 数据结构
<details>
  <summary>B 树和 B+ 树的区别，分别是怎么实现的</summary>

</details>
<details>
  <summary>用过哪些数据结构</summary>

</details>
<details>
  <summary>图有哪两种遍历方式</summary>

</details>
<details>
  <summary>怎么用 STL 最快实现队，优先队列</summary>

</details>
<details>
  <summary>STL 里 map 的底层实现</summary>

</details>

## 算法
<details>
  <summary>MySQL 统计各个分数的人数</summary>

</details>
<details>
  <summary>二叉搜索树的第 k 大（小）的数</summary>

</details>
<details>
  <summary>基本的排序算法，时间空间复杂度</summary>

</details>
<details>
  <summary>归并排序的空间复杂度为什么是 O(n)</summary>

</details>
<details>
  <summary>S快速排序最好最坏时间复杂度，怎么样避免出现最差情况</summary>

</details>
<details>
  <summary>堆排序应用于大数据</summary>

</details>
<details>
  <summary>写一个二叉树实现一个堆，再实现堆排序</summary>

</details>
<details>
  <summary>map 实现分数排序，分数相同按照胶卷时间</summary>

</details>
<details>
  <summary>循环有序数组查找某个数在不在里面</summary>

</details>
<details>
  <summary>Z 字形遍历二叉树</summary>

</details>

## 数据库
<details>
  <summary>MySQL 索引为何失效</summary>

</details>
<details>
  <summary>事务是什么</summary>

</details>

## 设计模式
<details>
  <summary>说一下了解的设计模式，我们为什么会有设计模式，有什么优点</summary>

</details>
<details>
  <summary>单例模式的好处，工厂模式的好处，主要解决的问题有哪些</summary>

</details>

## 项目

## 自我介绍

## 其他
<details>
  <summary>拉普拉斯变换</summary>
  
  
</details>
<details>
  <summary>Git 与 GtiHub</summary>
  
  
</details>




## C++
<details>
  <summary>malloc/free 与 new/delete 之间的区别</summary>
  
  - `new` / `delete` 是 **C++ 关键字**，需要编译器支持；`malloc` / `free` 是**库函数**，需要头文件支持。
  - 使用 `new` 操作符申请内存分配时**无须指定内存块的大小**，编译器会根据类型信息自行计算；而 `malloc` 则需要**显式地指出所需内存块的大小**。
  - `new` 操作符内存分配成功时，**返回对象类型的指针**，类型严格与对象匹配，无须进行类型转换，故 `new` 是符合类型安全性的操作符；而 `malloc` 内存分配成功则是**返回 void 类型指针**，需要通过强制类型转换将 `void*` 指针转换成我们需要的类型。
  - `new` 内存分配失败时，**抛出 bac_alloc 异常**。`malloc` 分配内存失败时**返回 NULL**。
  -  `new` 会先调用 `operator new` 函数，**申请足够的内存**（通常底层使用 `malloc` 实现），然后**调用对应类型的构造函数**，初始化成员变量，最后返回自定义类型指针；`delete` 先**调用析构函数**，然后调用 `operator delete` 函数**释放内存**（通常底层使用 `free` 实现）；`malloc` / `free` 是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。
  - `new` 操作符从**自由存储区**（free store）上为对象动态分配内存空间；而 `malloc` 函数从**堆**上动态分配内存。自由存储区是 C++ 基于 `new` 操作符的一个抽象概念，凡是通过 `new` 操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C++ 使用 `malloc` 从堆上分配内存，使用 `free` 释放已分配的对应内存。自由存储区不等于堆。
  - `operator new` / `operator delete` **允许重载**，`malloc` / `free` **不允许重载**。
  > 参考：[经典面试题之 new 与 malloc 的区别](https://blog.csdn.net/nie19940803/article/details/76358673)，[new 与 malloc 的10点区别](https://www.cnblogs.com/shilinnpu/p/8945637.html)
  
</details>
<details>
  <summary>new 与 operator new 的区别（自问）</summary>

  - `new` (new operator) 是一个内置的**运算符**；`operator new` 是一个**函数**。
  - `new` 做的工作是调用 `operator new(sizeof(A))` + 调用`A::A()` + 返回指针，即**分配内存+调用构造函数+返回指针**；而 `operator new` 仅仅做了**分配内存的工作**。
  > 参考：[C++ 内存分配（new, operator new）详解](https://blog.csdn.net/uestclr/article/details/51171025)，[一分钟理解 C++ new 和 operator new](https://zhuanlan.zhihu.com/p/113439671)

</details>
<details>
  <summary>编译过程中各阶段所作工作</summary>

  - **预处理 $\Rightarrow$ 编译 $\Rightarrow$ 汇编 $\Rightarrow$ 链接**。
  - 预处理（.c-->.i）：编译器将头文件编译进来，还有宏的替换。
  - 编译（.i->.s）：编译器主要做词法分析，语法分析，语义分析等工作，检查无错误后，将其翻译为汇编语言。
  - 汇编（.s-->.o）：汇编器将汇编语言翻译成目标机器指令，生成目标文件。
  - 链接（.o-->可执行文件）：链接器将有关的目标文件链接起来，转为可执行文件。
  > 参考：[程序编译的四个阶段](https://www.jianshu.com/p/539a712ed284)，[C++ 程序编译过程](https://zhuanlan.zhihu.com/p/45402323)

</details>
<details>
  <summary>编译过程中的编译阶段所作的工作（自问）</summary>
  
  - **词法分析 $\Rightarrow$ 语法分析 $\Rightarrow$ 语义分析**。
  - 词法分析：其任务是对源程序逐字扫描，从中识别出一个个“单词”，“单词”又叫做符号，他是程序语言的基本语法单位，如关键字，标识符，常数，运算符，分隔符等。
  - 语法分析：其任务是跟根据语言的规则将单词符号序列分解成语法单位，如“表达式”，“语句”，“程序”等。语法规则就是语法单位的构成规则，通过语法分析确定整个输入串能否构成一个语法上正确的程序。如果程序没有错误，语法分析后就能正确的构造出语法树，否则就会指出语法错误，并给出诊断。
  - 语义分析：其任务是对类型进行分析和检查，一般类型检查包括两点：类型载体及在其上的运算。如，整除取余运算符只能对整数使用，如果运算对象是浮点数就认为是类型错误。
  > 参考：[编译原理-编译过程概述](https://blog.csdn.net/GarfieldGCat/article/details/89200785)

</details>
<details>
  <summary>纯虚函数与虚函数</summary>
  
  - 包含纯虚函数的类称为抽象类。
  - 不能实例化含纯虚函的类。
  - 在纯虚函数的函数原型后加上 `=0` 代表该虚函数为纯虚函数。
  - 声明一个纯虚函数的目的是为了让派生类只继承函数接口；声明一个虚函数的目的是为了让抽象类继承该函数的接口和缺省实现。
  - 可以为纯虚函数提供定义，即可为其供应一份实现代码，调用它的唯一途径是使用时明确指出其 class 名称。例如：`a->base::test();`，其中 `base` 是一个抽象类，`a` 是以 `base` 作为基类的派生类的一个实例，`test()` 是在 `base` 内声明并定义的一个纯虚函数。
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
  
  - `malloc` 的调用需要内存大小；`free` 的调用需要`malloc` 返回的内存地址，而不需要内存大小。
  - `malloc` 返回的地址的前面一块内存存有 `malloc` 分配的内存大小值，`free` 根据这个内存大小值释放同样大小的内存块。
  - 例子：假设你用 `malloc` 需要申请100个字节，实际是可能申请了104个字节，前4字节存有该块内存的实际大小100，并把前4个字节后的地址返回给你。`free` 释放的时候会根据传入的地址向前偏移4个字节，从这4字节获取具体的内存块大小并释放。
  > 参考：[C 语言中 free 怎么知道要删除多大的空间](https://www.zhihu.com/question/23196195)

</details>
<details>
  <summary>C++ 虚函数表</summary>


</details>
<details>
  <summary>C++ 三大特性概括（自问）</summary>

  - **封装 + 继承 + 多态**
  - 封装：隐藏对象的属性和实现细节，使得代码模块化，仅公开接口供用户使用。
  - 继承：代码复用的重要手段，可以在类原有的特性基础上进行拓展，增加功能。
  - 多态：一个接口，多种方法。
  - 以上仅为三大特性概念的概括。
  > 参考：[C++ 三大特性-封装](https://blog.csdn.net/cherrydreamsover/article/details/81942293)，[C++ 中的继承](https://blog.csdn.net/studyhardi/article/details/90744785)，[C++ 之多态性](https://blog.csdn.net/studyhardi/article/details/90815766)

</details>
<details>
  <summary>重载，覆写，隐藏（自问）</summary>
  
  - 重载：同一可访问区内被声明的几个具有不同参数列表（参数的类型，个数，顺序不同）的同名函数，根据参数列表确定调用那个函数。
  - 覆写：派生类中重新定义的函数，其函数名，参数列表，返回类型，所有都必须痛基类中被重写的函数一致，只有函数体不同。
  - 隐藏：根据 C++ 的名称遮掩规则，内层作用域的名称会遮掩外层作用域的名称，在类中，基类是外层作用域，派生类是内层作用域，派生类中的函数与屏蔽与其同名的基类函数。如果派生类中的函数与基类同名，并且参数不同，无论有无 `virtual` 关键字，基类的函数都被隐藏；如果派生类的函数与基类的函数同名，并且参数相同，但是函数没有 `virtual` 关键字，此时基类的函数被隐藏。
  ```C++
  class Base {
    private:
      int x;
    public:
      virtual void mf1() = 0;
      virtual void mf1(int);
      void mf3();
      void mfs(double);
  };

  class Derived: public Base {
    public: 
      virtual void mf1(); // 覆盖 void Base::mf1()，隐藏 void Base::mf1(int)
      virtual void mf3(); // 隐藏 void Base::mf3()，void Base::mf3(double)
      void mf4();
      void mf4(double); // 重载 void Derived::mf4()
  }
  ```
  > 参考：[C++ 中重载、重写（覆盖）和隐藏的区别](https://blog.csdn.net/zx3517288/article/details/48976097)，[重载、覆盖、隐藏的区别](https://blog.csdn.net/weixin_40087851/article/details/82012624)，[Effective C++ 条款33：避免遮掩继承而来的名称]()

</details>

## 网络
<details>
  <summary>TCP 粘包拆包</summary>

  - 拆包原因1：要发送的数据大于 TCP 发送缓冲区剩余空间大小。
  - 拆包原因2：待发送数据大于 MSS（最大报文段长度），TCP 在传输前将进行拆包。
  - 粘包原因1：要发送的数据小于 TCP 发送缓冲区的大小，TCP 将多次写入缓冲区的数据一次发送出去。
  - 粘包原因2：接收数据端的应用层没有及时读取接收缓冲区中的数据。
  - 解决方案1：给每个数据包添加包首部，首部中应该至少包含数据包的长度，这样接收端在接收到数据后，通过读取包首部的长度字段，便知道每一个数据包的实际长度
  - 解决方案2：发送端将每个数据包封装为固定长度，接收端每次从接收缓冲区中读取固定长度的数据就自然而然的把每个数据包拆分开来。
  - 解决方案3：可以在数据包之间设置边界，服务端从网络流中按边界分离出各个数据包。
  - TCP 是基于字节流的，虽然应用层和 TCP 传输层之间的数据交互是大小不等的数据块，但是 TCP 把这些数据块仅仅看成一连串无结构的字节流，没有边界；另外从 TCP 的帧结构也可以看出，在 TCP 的首部没有表示数据长度的字段，基于上面两点，在使用 TCP 传输数据时，才有粘包或者拆包现象发生的可能；UDP 是基于报文发送的，从 UDP 的帧结构可以看出，在 UDP 首部采用了 16bit 来指示 UDP 数据报文的长度，因此在应用层能很好的将不同的数据报文区分开，从而避免粘包和拆包的问题。
  - 还有 TCP 拆包粘包表现形式的图没有画。
  > 参考：[TCP粘包，拆包及解决方法](https://blog.csdn.net/wxy941011/article/details/80428470)，[计算机网络-自顶向下方法 3.5.1-TCP 连接]()
  
</details>
<details>
  <summary>HTTP 与 HTTPS 的区别及 HTTPS 流程</summary>
  
  - HTTP：HTTP 也称超文本传输协议，是 Web 的应用层协议，由一个客户程序和一个服务器程序组成，客户程序和服务器程序运行在不同的端系统中，通过交换 HTTP 报文进行会话。
  - HTTPS：HTTPS 也称安全超文本传输协议，在数据进行传输前，对数据进行加密，使用 HTTP + SSL 实现。
  - HTTP 明文传输，数据都是未加密的，安全性较差，HTTPS数据在传输过程是加密的，安全性较好。
  - 使用 HTTPS 协议需要到 CA（Certificate Authority，数字证书认证机构） 申请证书，一般免费证书较少，因而需要一定费用。
  - HTTP 页面响应速度比 HTTPS 快，主要是因为 HTTP 使用 TCP 三次握手建立连接，客户端和服务器需要交换3个包，而 HTTPS 除了 TCP 的三个包，还要加上 ssl 握手需要的9个包，所以一共是12个包。
  - HTTP 的标准端口80，HTTPS 标准端口是443。
  - **HTTPS 流程**：(1) 客户使用 https 的 url 访问 Web 服务器的443端口，向服务器发起 HTTPS 请求 $\Longrightarrow$ (2) 服务器收到请求后，将网站的证书信息（包含 CA 证书的公钥）传送一份给客户端 $\Longrightarrow$ (3) 客户收到服务器的证书后，验证其合法性，如果合法，客户生成一个随机值，作为客户的密钥，使用服务器公钥对随机值进行非对称加密 $\Longrightarrow$ (4) 将加密后的客户密钥发送给服务器 $\Longrightarrow$ (5) 服务器接收到客户发来的密文后，用服务器私钥进行非对称解密，解密之后的明文就是客户密钥，然后用客户密钥对数据进行对称加密，这样数据就成了密文 $\Longrightarrow$ (6) 服务器将密文发送给客户 $\Longrightarrow$ (7) 客户收到服务器发来的密文，用客户密钥对其进行对称解密，得到服务器发送的数据。
  - 一个 HTTPS 请求包含了两次 HTTP 传输，HTTPS 传输过程中使用了3个密钥，服务器公钥，用于非对称加密,服务器私钥，用于非对称解密，客户密钥，用于对称加密和解密。
  > 参考：[HTTP 和 HTTPS](https://www.cnblogs.com/zhenguoli/p/8622933.html)，[HTTPS 原理及流程](https://www.jianshu.com/p/14cd2c9d2cd2)，[HTTPS过程以及详细案例](https://www.cnblogs.com/helloworldcode/p/10104935.html)，[计算机网络-自顶向下方法]()

</details>
<details>
  <summary>HTTP 状态码</summary>

  - 200：请求成功。
  - 301：资源被永久转移到其他 URL。
  - 400：指示该请求不能被服务器理解。
  - 404：请求的资源不在服务器上。
  - 500：内部服务器错误。
  - 505：服务器不支持请求报文使用的 HTTP 协议版本。
  - 1**：信息，服务器收到请求，需要请求者继续执行操作。
  - 2**：成功，操作被成功接收并处理。
  - 3**：重定向，需要进一步的操作以完成请求。
  - 4**：客户端错误，请求包含语法错误或无法完成请求。
  - 5**：服务器错误，服务器在处理请求的过程中发生了错误。
  > 参考：[HTTP 状态码](https://www.runoob.com/http/http-status-codes.html)

</details>
<details>
  <summary>socket 网络编程（自问）</summary>
  
  - `SOCKET PASCAL FAR socket(int af, int type, int protocol)`：`socket` 函数用于创建套接字，参数 `af` 指定同学发生的区域：`AF_UNIX`，`AX_INET`，`AF_INET6`，`AF_UNIX` 为 UNIX 本地通信，`AX_INET` 是使用 IPV4 通信，`AF_INET6` 是使用 IPV6 通信；参数 `type`  描述要建立的套接字类型：`SOCK_STREAM`，`SOCK_DGRAM`，`SOCK_RAW`，`SOCK_STREAM` 是 流式套接字，`SOCK_DGRAM` 是 数据报式套接字，`SOCK_RAW` 是原始式套接字；参数 `protocol` 说明该套接字使用的特定协议，选择 TCP 或是 UDP。
  - `int PASCAL FAR bind(SOCKET s, const struct sockaddr FAR * name, int namelen)`：`bind` 函数将套接字地址（包括本地主机地址和本地端口地址）与所创建的套接字绑定起来。
  - `int PASCAL FAR connect(SOCKET s, const struct sockaddr FAR * name, int namelen)`：`connect` 函数用于建立连接。
  - `SOCKET PASCAL FAR accept(SOCKET s, struct sockaddr FAR* addr, int FAR* addrlen)`：`accept` 函数用于使服务器等待来自某客户进程的实际连接。
  - `int PASCAL FAR listen(SOCKET s, int backlog)`：`listen` 函数用于监听客户发来的连接请求。
  - `int PASCAL FAR send(SOCKET s, const char FAR *buf, int len, int flags)`：`send` 函数用于发送数据。
  - `int PASCAL FAR recv(SOCKET s, char FAR *buf, int len, int flags)`：`recv` 函数用于接收数据。
  - `int PASCAL FAR select(int nfds, fd_set FAR * readfds, fd_set FAR * writefds, fd_set FAR * exceptfds, const struct timeval FAR * timeout)`：`select` 函数用于检测一个或多个套接字的状态，对每一个套接字来说，这个调用可以请求读、写或错误状态方面的信息。请求给定状态的套接字集合由一个 `fd_set` 结构指示，在返回时，此结构被更新，以反映那些满足特定条件的套接字的子集，同时， `select` 函数调用返回满足条件的套接字的数目。
  - 使用 `select` 函数可以写出**非阻塞**的程序，可以进行 **IO 多路复用**。当程序中使用 `connect`，`accept`，`recv`这几个函数时，程序就是阻塞程序，执行到这些函数的时候必须等待某个事件发生，如果没有发生，进程或者线程就被阻塞，而且如果有多个套接字都要传输的时候，一个套接字在发送和接收过程中一直占用着设定的端口，此时其他套接字无法在该端口传输数据，其阻塞时间又会浪费实际可用的时间，使得效率很低。所以可以使用 `select` 函数，`select` 函数可以检测套接字的状态，只要轮询 `select` 函数，查看当前是否有可以处理的套接字即可。
  - 还差两张 socket 通信原理的图没画，一个 TCP 的，一个 UDP 的。
  > 参考：[socket 技术详解](https://www.cnblogs.com/fengff/p/10984251.html)，[socket 通信中 select 函数的使用和解释](https://www.cnblogs.com/gangzilife/p/9766292.html)，[IO 多路复用](https://www.jianshu.com/p/dd5b6893bef7)
  
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
<details>
  <summary>调用函数压栈，一层一层</summary>

</details>

## Linux

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
  <summary>事务是什么，事务的几个特性</summary>
  
  - MySQL 事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你既需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务。
  - **原子性**：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
  - **一致性**：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
  - **隔离性**：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
  - **持久性**：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。
  > 参考：[MySQL 事务](https://www.runoob.com/mysql/mysql-transaction.html)

</details>

## 设计模式
<details>
  <summary>为什么会有设计模式，有什么优点</summary>
  
  - 加快开发速度，设计模式是过去的程序员积累的经验，对某些场景下的代码设计有一定的总结，可以直接套用。
  - 提高**可维护性、可扩展性、可复用性**，初学者写的代码可能只考虑了功能实现，而没有考虑到后期维护问题，后期如果增加新功能代码应该如何修改，类之间的关系可能错综复杂，不熟悉该项目代码的人难以阅读及理解。
  > 参考：[设计模式简介](https://www.runoob.com/design-pattern/design-pattern-intro.html)，[设计模式总结篇](https://blog.csdn.net/niunai112/article/details/80034203)
  
</details>
<details>
  <summary>设计模式的几项原则（自问）</summary>

  - **开放封闭原则**：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码。
  - **里氏代换原则**：任何基类可以出现的地方，子类一定可以出现。LS 里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。
  - **依赖倒转原则**：针对接口编程，依赖于抽象而不依赖于具体。
  - **接口隔离原则**：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。
  - **最少知道原则**：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。
  - **合成复用原则**：尽量使用合成/聚合的方式，而不是使用继承。
  > 参考：[C++------简单工厂模式，工厂方法模式和抽象工厂模式](https://www.cnblogs.com/zhangzeze/p/9392598.html)

</details>
<details>
  <summary>简单工厂模式</summary>

  - 简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。
  - 例子：创建一个操作符工厂，该工厂可以生产各种操作符实例，客户在生产前需要告知该工厂类需要生产的操作符实例类型。
  ```C++
  #include <iostream>
  #include <string>
  using namespace std;
  #define ADD 1
  #define SUB 2
  #define MUL 3
  #define DIV 4

  class Operation {
  public:
      double getNumberA() { return _numberA; }
      double getNumberB() { return _numberB; }
      void setNumberA(int a) { _numberA = a; }
      void setNumberB(int b) { _numberB = b; }
      virtual double GetResult() { double result = 0; return result; }
  private:
      double _numberA = 0;
      double _numberB = 0;
  };

  class OperationAdd : public Operation {
  public:
      virtual double GetResult() { double result = 0; result = getNumberA() + getNumberB(); return result; }
  };

  class OperationSub : public Operation {
  public:
      virtual double GetResult() { double result = 0; result = getNumberA() - getNumberB(); return result; }
  };

  class OperationMul : public Operation {
  public:
      virtual double GetResult() { double result = 0; result = getNumberA() * getNumberB(); return result; }
  };

  class OperationDiv : public Operation {
  public:
      virtual double GetResult() {
          double result = 0;
          if (!getNumberB()) throw "Division by zero condition!";
          result = getNumberA() / getNumberB();
          return result;
      }
  };

  class OperationFactory {
  public:
      static Operation* createOperate(int operate) {
          Operation *op = nullptr;
          switch(operate) {
              case ADD: op = new OperationAdd(); break;
              case SUB: op = new OperationSub(); break;
              case MUL: op = new OperationMul(); break;
              case DIV: op = new OperationDiv(); break;
          }
          return op;
      }

  private:
      static Operation* op;
  };
  Operation* OperationFactory::op = nullptr;

  int main() {
      Operation* _operator_ = OperationFactory::createOperate(SUB);
      _operator_->setNumberA(5);
      _operator_->setNumberB(2);
      double res = _operator_->GetResult();
      cout << "result: " << res << endl;
      return 0;
  }
  ```
  - 优点：工厂类包含了必要的逻辑判断，根据客户端的选择条件动态实例化相关的类，对于客户端来说，去除了与具体产品的依赖。
  - 缺点：违反了开放封闭原则：对扩展开放，对修改关闭。如上面的例子，如果要增加一个求 `NumberA` 的 `NumberB` 次方的功能，除了要添加一个 `Operation` 的派生类`OperationPow`，还要对 `OperationFactory` 进行修改，在 `createOperate` 函数的 `switch` 内添加一个属于 `OperationPow` 的分支条件。
  > 参考：[C++------简单工厂模式，工厂方法模式和抽象工厂模式](https://www.cnblogs.com/zhangzeze/p/9392598.html)，[大话设计模式-简单设计模式]()

</details>
<details>
  <summary>工厂方法模式</summary>
  
  - 工厂方法模式是指定义一个用于创建对象的接口，让子类决定实例化哪个类，将创建过程延迟到子类中进行。
  - 例子：创建两个工厂，一个用于生产学雷锋的大学生，一个用于生产社区志愿者，当有老人需要帮助的时候，客户如果想要大学生可以找生产大学生的工厂，如果想要志愿者可以找生产志愿者的工厂。
  ```C++
  #include <iostream>
  #include <string>
  using namespace std;

  class LeiFeng {
  public:
      virtual void swap() { cout << "swap" << endl; }
      virtual void wash() { cout << "wash" << endl; }
      virtual void buyRice() { cout << "buyRice" << endl; }
  };

  class Upgrade: public LeiFeng {
  public:
      virtual void swap() { cout << "upgrade swap" << endl; }
      virtual void wash() { cout << "upgrade wash" << endl; }
      virtual void buyRice() { cout << "upgrade buyRice" << endl; }
  };

  class Volunteer: public LeiFeng {
      virtual void swap() { cout << "volunteer swap" << endl; }
      virtual void wash() { cout << "volunteer wash" << endl; }
      virtual void buyRice() { cout << "volunteer buyRice" << endl; }
  };

  class IFactory {
  public:
      virtual LeiFeng* createLeiFeng() = 0;
  };

  class UndergraduateFactory: public IFactory {
  public:
      virtual LeiFeng* createLeiFeng() { return new Upgrade(); }
  };

  class VolunteerFactory: public IFactory {
  public:
      virtual LeiFeng* createLeiFeng() { return new Volunteer(); }
  };

  int main() {
      IFactory* factory = new VolunteerFactory();
      LeiFeng* student = factory->createLeiFeng();
      student->swap();
      student->wash();
      student->buyRice();
      return 0;
  }
  ```
  - 优点：保留了简单工厂模式的优点，即据客户端的选择条件动态实例化相关的类，对于客户端来说，去除了与具体产品的依赖；同时也克服了简单工厂违背开放封闭原则的缺点。
  - 缺点：每次增加一个新产品，除了需要增加一个该产品的类（简单工厂模式也需要），还要再增加一个该产品的工厂类。
  > 参考：[C++------简单工厂模式，工厂方法模式和抽象工厂模式](https://www.cnblogs.com/zhangzeze/p/9392598.html)，[大话设计模式-简单设计模式]()

</details>
<details>
  <summary>抽象工厂模式</summary>

  - 抽象工厂模式是指提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
  - 例子：创建两个工厂，一个使用 SqlServer 语句作为材料，一个使用 Access 语句作为材料，两个工厂都会生产一系列不同的产品，但是两个工厂的生产的产品系列都是一样的，一个工厂有的产品另一个工厂也会生产同系列的产品，只是两个工厂使用的原材料不同，客户可以选择其中一个工厂为其生产所有不同系列的产品。
  ```C++
  #include <iostream>
  #include <string>
  using namespace std;

  class User {
  public:
      int getId() { return id; }
      string getName() { return name; }
      void setId(int i) { id = i; }
      void setName(string n) { name = n; }

  private:
      int id;
      string name;
  };

  class Department {
  public:
      int getId() { return id; }
      string getDepartName() { return depart_name; }
      void setId(int i) { id = i; }
      void setDepartName(string n) { depart_name = n; }

  private:
      int id;
      string depart_name;
  };

  class IUser {
  public:
      virtual void insert(User user) = 0;
      virtual User getUser(int id) = 0;
  };

  class SqlServerUser: public IUser {
  public:
      virtual void insert(User user) { cout << "sqlServer insert" << endl; }
      virtual User getUser(int id) { cout << "sqlServer getUser" << endl; return User(); }
  };

  class AccessUser: public IUser {
  public:
      virtual void insert(User user) { cout << "access insert" << endl; }
      virtual User getUser(int id) { cout << "access getUser" << endl; return User(); }
  };

  class IDepartment {
  public:
      virtual void insert(Department department) = 0;
      virtual Department getDepartment(int id) = 0;
  };

  class SqlServerDepartment: public IDepartment {
  public:
      virtual void insert(Department department) { cout << "sqlServer insert" << endl; }
      virtual Department getDepartment(int id) { cout << "sqlServer getDepartmentUser" << endl; return Department(); }
  };

  class AccessDepartment: public IDepartment {
  public:
      virtual void insert(Department department) { cout << "access insert" << endl; }
      virtual Department getDepartment(int id) { cout << "access getDepartmentUser" << endl; return Department(); }
  };

  class IFactory {
  public:
      virtual IUser* createUser() = 0;
      virtual IDepartment* createDepartment() = 0;
  };


  class SqlServerFactory: public IFactory {
  public:
      virtual IUser* createUser() { return new SqlServerUser(); }
      virtual IDepartment* createDepartment() { return new SqlServerDepartment(); }
  };

  class AccessFactory: public IFactory {
  public:
      virtual IUser* createUser() { return new AccessUser(); }
      virtual IDepartment* createDepartment() { return new AccessDepartment(); }
  };

  int main() {
      User* user = new User();
      Department* department =  new Department();
      IFactory* factory = new SqlServerFactory();
      IUser* iu = factory->createUser();
      iu->getUser(1);
      iu->insert(*user);
      IDepartment* ide = factory->createDepartment();
      ide->getDepartment(1);
      ide->insert(*department);
      return 0;
  }
  ```
  - 优点：用户可以更为便利的交换一系列产品，如上面的例子，此时使用的是 SqlServer 作为数据库，使用的插入、查找都是 SqlServer 语句，如果要切换成 Access 作为数据库，只需要将 `IFactory* factory = new SqlServerFactory();` 改为 `IFactory* factory = new AccessFactory();`，这里的插入、查找就是产品，`SqlServerFactory` 和 `AccessFactory` 生产出来的插入和查找都是不一样的。
  - 缺点：扩展十分麻烦，如果需要在增加一个项目表 IProject，则需要增加 `IProject`，`SqlserverProject`，`AccessProject` 共三个类，还需要修改`IFactory`，`SqlserverFactory`，`AccessFactory`共三个类。《大话设计模式》一书中介绍了使用**反射 + 抽象工厂**进行设计的抽象工厂模式，更占优势，但是 C++ 中并没有反射功能，实现起来可能比较麻烦，这个的实现先暂时搁置。
  > 参考：[C++------简单工厂模式，工厂方法模式和抽象工厂模式](https://www.cnblogs.com/zhangzeze/p/9392598.html)，[大话设计模式-简单设计模式]()

</details>
<details>
  <summary>单例模式</summary>
  
  - 单例模式保证一个类只有一个实例，并提供一个访问它的全局访问点。
  - 有缺陷的懒汉式单例模式，此方法有两个缺陷，一个是**线程安全问题**，当多线程获取单例时有可能引发竞态条件：如果一个线程在 `if` 语句中判断 `m_instance` 为空，假定此时只是刚刚判断完条件，刚进入内部，要执行但是还没有执行 `m_instance = new Singleton();` 语句，恰好第二个线程也尝试获取单例，这个时候判断 `m_instance` 还是空的，因为此时第一个线程还没有成功创建单例，于是线程一与线程二都会进行实例化，这样就有两个实例了，不符合单例的定义，使用加锁可解决此问题；二是**内存泄漏问题**，可能会忘记对单例对象进行 `delete`操作，这可能会造成内存泄漏，使用智能指针可解决此问题。
  ```C++
  #include <iostream>
  using namespace std;

  class Singleton {
  public:
      static Singleton* getInstance() { if (m_instance == nullptr) m_instance = new Singleton(); return m_instance; }
      ~Singleton() { cout << "singleton deconstruct!" << endl; }
  private:
      static Singleton* m_instance;
      Singleton () { cout << "singleton construct!" << endl; };
      Singleton(const Singleton& singleton) = delete;
      Singleton& operator=(const Singleton& singleton) = delete;
  };
  Singleton* Singleton::m_instance = nullptr;

  int main() {
      Singleton* instance1 = Singleton::getInstance();
      Singleton* instance2 = Singleton::getInstance();
      delete instance1;
      return 0;
  }
  ```
  - 线程安全、内存安全的单例模式，使用 `shared_ptr`，使用对象管理资源的思想，保证内存安全，加入双检锁，提供线程安全，为什么要有两重判断呢，这是因为创建锁的开销比较大，如果只是在 `if` 判断前加一个锁，那么不管之前是否创建过单例，每次使用这个函数的时候都进行加锁操作，成本较高，如果在加锁操作外面加一个 `if` 判断语句的话，如果之前创建过单例，就不会进行加锁操作了，但是加锁操作后面的那个 `if` 判断还是要保留的，因为两个或多个线程也许都可以通过第一个 `if` 判断，在一个线程执行完创建实例的工作后，由于没有另外的判断，已经通过第一个 `if` 判断的其他线程则会继续创建实例，所以需要加上两个判断语句。但实际上双检索有严重 bug，由于内存读写的乱序执行。
  ```C++
  #include <iostream>
  #include <memory>
  #include <mutex>
  using namespace std;

  class Singleton {
  public:
      static shared_ptr<Singleton> getInstance() {
          if (m_instance == nullptr) {
              lock_guard<mutex> lock(m_mutex);
              if (m_instance == nullptr)
                  m_instance = shared_ptr<Singleton>(new Singleton());
          }
          return m_instance;
      }
      ~Singleton() { cout << "singleton deconstruct!" << endl; }

  private:
      static shared_ptr<Singleton> m_instance;
      static mutex m_mutex;
      Singleton () { cout << "singleton construct!" << endl; };
      Singleton(const Singleton& singleton) = delete;
      Singleton& operator=(const Singleton& singleton) = delete;
  };
  shared_ptr<Singleton> Singleton::m_instance = nullptr;
  mutex Singleton::m_mutex; 

  int main() {
      shared_ptr<Singleton> instance1 = Singleton::getInstance();
      shared_ptr<Singleton> instance2 = Singleton::getInstance();
      return 0;
  }
  ```
  - Magic Static 版懒汉模式（推荐），这里所用到的特性是 C++11 标准中的 **Magic Static** 特性：如果当变量在初始化的时候，并发同时进入声明语句，并发线程将会阻塞等待初始化结束。这样保证了并发线程在获取静态局部变量（C++ 静态变量的生存期是从声明到程序结束）的时候一定是初始化过的，所以具有线程安全性。注意这里 `getInstance` 函数返回的是引用而不是指针，理由主要是无法避免用户使用 `delete m_instance;` 导致对象被提前销毁。
  ```C++
  #include <iostream>
  #include <memory>
  #include <mutex>
  using namespace std;

  class Singleton {
  public:
      static Singleton& getInstance() {
          static Singleton m_instance;
          return m_instance;
      }
      ~Singleton() { cout << "singleton deconstruct!" << endl; }

  private:
      Singleton () { cout << "singleton construct!" << endl; };
      Singleton(const Singleton& singleton) = delete;
      Singleton& operator=(const Singleton& singleton) = delete;
  };

  int main() {
      Singleton& instance1 = Singleton::getInstance();
      Singleton& instance2 = Singleton::getInstance();
      return 0;
  }
  ```
  - 饿汉式单例模式，饿汉式单例模式在单例定义的时候就会进行实例化，而懒汉式单例模式在需要使用的时候进行实例化。饿汉模式是线程安全的。
  ```C++
  #include <iostream>
  #include <memory>
  #include <mutex>
  using namespace std;

  class Singleton {
  public:
      static Singleton* getInstance() { return m_instance; }
      ~Singleton() { cout << "singleton deconstruct!" << endl; }

  private:
      static Singleton* m_instance;
      Singleton () { cout << "singleton construct!" << endl; };
      Singleton(const Singleton& singleton) = delete;
      Singleton& operator=(const Singleton& singleton) = delete;
  };
  Singleton* Singleton::m_instance = new Singleton();

  int main() {
      Singleton* instance1  = Singleton::getInstance();
      Singleton* instance2 = Singleton::getInstance();
      return 0;
  }
  ```
  - 懒汉式和饿汉式的选择：在访问量较小时，采用懒汉实现，这是以时间换空间；在访问量比较大，采用饿汉实现，这是以空间换时间。
  > 参考：[C++ 单例模式总结与剖析](https://www.cnblogs.com/sunchaothu/p/10389842.html)，[设计模式之单例模式（C++ 版）](https://segmentfault.com/a/1190000015950693)
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
<details>
  <summary>学习中令人愉悦的事情</summary>
  
  - 知识点串联起来的时候。
  
</details>




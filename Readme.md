# ZMQ 简介

 - ZeroMQ: http://www.zeromq.org
 - ZeroMQ API: http://api.zeromq.org/
 - ZGuide: http://zguide.zeromq.org/
 - ZGuide Example: https://github.com/booksbyus/zguide

 ZMQ（ØMQ、ZeroMQ, 0MQ）

 - 看起来像是一套嵌入式的网络链接库，但工作起来更像是一个并发式的框架。
 - 提供的套接字可以在多种协议中传输消息，如线程间、进程间、TCP、广播等。
 - 可以使用套接字构建多对多的连接模式，如扇出、发布-订阅、任务分发、请求-应答等。
 - ZMQ的快速足以胜任集群应用产品。
 - 异步I/O机制能够支持构建多核应用程序，完成异步消息处理任务。
 - ZMQ有着多语言支持，并能在几乎所有的操作系统上运行。
 - 应用程序无法直接和Socket 连接直接打交道，因为它们是被封装在ZMQ底层的。
 - ZMQ是iMatix公司的产品，以LGPL开源协议发布。

TCP套接字和ZMQ套接字之间的区别：

 - ZMQ 不仅支持 TCP 协议，也支持多种其他协议：inproc（进程内）、ipc（进程间）、tcp、pgm（广播）、epgm等。

 - ZMQ Socket 当客户端使用zmq_connect()时连接就已经建立了，并不要求该端点已有某个服务使用zmq_bind()进行了绑定。

 - ZMQ没有提供类似zmq_accept()的函数，因为当套接字绑定至端点时它就自动开始接受连接了.

 - ZMQ套接字传输的是消息（一段指定长度的二进制数据块），而不是字节（TCP）或帧（UDP）。
 
 - 连接是异步的，并由一组消息队列做缓冲；  
 ZMQ套接字在后台进行I/O操作，无论是接收还是发送消息，它都会先传送到一个本地的缓冲队列，这个内存队列的大小是可以配置的。

 - ZMQ套接字可以和多个套接字进行连接。  
 TCP协议只能进行点对点的连接，而ZMQ则可以进行一对一、一对多（类似于无线广播）、多对多（类似于邮局）、多对一（类似于信箱）。

## ZMQ 提供的 Socket 协议:

 - 单播传输协议（inporc, ipc, tcp）
 - 广播协议（epgm, pgm)

 ### 单播传输协议

 - TCP传输协议
    - 优点：
        - 灵活、便携、且足够快速
        - 对程序而言是透明的，客户端和服务端可以随时进行连接和绑定。

 - 进程间协议(IPC)  
    - 优点：
        - 已从网络传输中抽象出来，不需要指定IP地址或者域名
        - 
    - 缺点：
        - 在Windows操作系统上运作，这一点也许会在未来的ZMQ版本中修复。
        - 在UNIX系统上，使用ipc协议还需要注意权限问题；需要保证所有的程序都能够找到这个ipc端点。

 - 进程内协议(inproc)  
    - 优点：
       - 比ipc或tcp要快得多
    - 缺点：
        - 只能在同一个进程的不同线程之间进行消息传输
        - 必须先绑定到端点，它才能建立连接。通常的做法是先启动服务端线程，绑定至端点，后启动客户端线程，连接至端点。

### 广播协议

 - pgm
 - epgm

## ZMQ的核心消息模式（ZMQ Basic Message Patterns）:

 - **请求-应答模式(REQ-REP)**:  
    将一组服务端和一组客户端相连，用于远程过程调用或任务分发。
 - **发布-订阅模式(PUB-SUB)**:  
    将一组发布者和一组订阅者相连，用于数据分发。  
 - **管道模式(Pipeline)**:  
    使用扇入或扇出的形式组装多个节点，可以产生多个步骤或循环，用于构建并行处理架构。 
 - **排他对接模式(Pair)**:  
    将两个套接字一对一地连接起来，（这种模式应用场景很少） 

## ZMQ 内置装置(Build-in Device)

 - Queue (Request-reply broker)
 - Forwarder (pub-sub proxy server)
 - Steamer (linke forwarder but for pipeline flows)

## ZMQ 合法的套接字连接-绑定对（一端绑定、一端连接）：

 - PUB - SUB
 - REQ - REP
 - REQ - ROUTER
 - DEALER - REP
 - DEALER - ROUTER
 - DEALER - DEALER
 - ROUTER - ROUTER
 - PUSH - PULL
 - PAIR - PAIR

## ZMQ Poller


--------------------------------------
 
ZMQ之所那么吸引人眼球，原因之一就是它是建立在标准套接字API之上。因此，ZMQ的套接字操作非常容易理解，其生命周期主要包含四个部分：

 - 创建和销毁套接字：zmq_socket(), zmq_close()
 - 配置和读取套接字选项：zmq_setsockopt(), zmq_getsockopt()
 - 为套接字建立连接：zmq_bind(), zmq_connect()
 - 发送和接收消息：zmq_send(), zmq_recv()
 
 

 




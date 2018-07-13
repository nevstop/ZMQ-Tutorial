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

## ZMQ 知识
 
 - [Message](./COMPONENTS/Message.md): ZMQ 通讯的传输数据
 - [Socket](./COMPONENTS/Socket.md) : ZMQ 通讯单元，可以选择不同的协议实现。
 - [Pattern](./COMPONENTS/Patterns.md) : ZMQ Sockets 间的通讯模型
 - [API](./COMPONENTS/API.md) : 以 C API 为例，说明 ZMQ 的 API 分类和功能。

## ZMQ v2.x to ZMQ v3.x

### 兼容性升级(Compatible Changes)

 1. PUB-SUB 消息过滤从 subscriber 端修改为为 Publisher 端。在很多的 PUB-SUB 用例中能显著的改善效率。不同版本的 publisher 和 subscriber 能够混用。

### 非兼容升级(Incompatible Changes)

 1. send/recv 方法的修改。zmq_send()/zmq_recv() 修改为zmq_msg_send()/zmq_msg_recv()。旧的代码会编译失败。
 2. 有函数在成功时有>0 的返回值，失败时返回 -1。ZMQ v2.x 成功时始终返回0。需要修改代码，严格的检查 -1 或 <0 表示失败。
 3. zmq_poll() 等待的时间单位从us修改为ms。
 4. ZMQ_NOBLOCK 修改为 ZMQ_DONTWAIT。旧的代码会编译失败。
 5. ZMQ_HWM设置分割为 ZMQ_SNDHWM 和 ZMQ_RCVHWM 两个设置。旧的代码会编译失败。
 6. 多数 zmq_getsockopt() 返回值都被修改为整数，可能会发生一些运行时错误。
 7. ZMQ_SWAP 被移除。旧的代码会编译失败，这种模式将不被支持。

## ZMQ v3.x to ZMQ v4.x

 1. INPROC协议，在4.x之前，需要首先bind，然后才能connect，否则会有运行时错误。在4.x之后，没有了这个限制，可以像 TCP一样使用。
 

 




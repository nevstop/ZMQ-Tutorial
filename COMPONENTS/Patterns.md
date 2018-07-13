# ZMQ PATTERN

## ZMQ的核心消息模式(ZMQ Basic Message Patterns)

zmq 实现了四种基本的消息模式。这四种模式在zmq 核心中实现，作为zmq api 的一部分，能够在各种语言、系统中使用。  
These four core patterns are cooked into ZeroMQ. They are part of the ZeroMQ API, implemented in the core C++ library, and are guaranteed to be available in all fine retail stores.  

 - **请求-应答模式(REQ-REP)**:  
    将一组服务端和一组客户端相连，用于远程过程调用或任务分发。
 - **发布-订阅模式(PUB-SUB)**:  
    将一组发布者和一组订阅者相连，用于数据分发。  
 - **管道模式(Pipeline)**:  
    使用扇入或扇出的形式组装多个节点，可以产生多个步骤或循环，用于构建并行处理架构。 
 - **排他对接模式(Exclusive pair)**:  
    将两个套接字一对一地连接起来，(这种模式应用场景很少) 

## ZMQ 合法的套接字连接-绑定对(一端绑定、一端连接)：

 - PUB - SUB
 - REQ - REP
 - REQ - ROUTER(take care, REQ inserts an extra null frame)
 - DEALER - REP(take care, REP assumes a null frame)
 - DEALER - ROUTER
 - DEALER - DEALER
 - ROUTER - ROUTER
 - PUSH - PULL
 - PAIR - PAIR
 - XPUB and XSUB sockets(they're like raw versions of PUB and SUB)

## ZMQ 内置装置(Build-in Device)

 - Queue (Request-reply broker)
 - Forwarder (pub-sub proxy server)
 - Steamer (linke forwarder but for pipeline flows)

## High-Level Messaging Patterns

On top of those, we add high-level messaging patterns. We build these high-level patterns on top of ZeroMQ and implement them in whatever language we're using for our application. They are not part of the core library, do not come with the ZeroMQ package, and exist in their own space as part of the ZeroMQ community. For example the Majordomo pattern, which we explore in Chapter 4 - Reliable Request-Reply Patterns, sits in the GitHub Majordomo project in the ZeroMQ organization.  
One of the things we aim to provide you with in this book are a set of such high-level patterns, both small (how to handle messages sanely) and large (how to make a reliable pub-sub architecture).
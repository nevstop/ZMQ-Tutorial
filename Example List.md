# Example 列表

## Hello Word Example

 - 使用 REQ-REP 模式，演示 ZMQ 如何工作。
 - 创建一个客户端和一个服务端，客户端发送Hello给服务端，服务端返回World。

          +------------+
          |   Server   |
          +------------+
          |    REQ     |
          \---+--------/
              |    ^
              |    |
         "Hello"  "World"
              |    |
              v    |
          /--------+---\
          |    REP     |
          +------------+
          |   Client   |
          +------------+       
          
       Figure # - Hello Word

## Weather Station Example

 - 演示发布-订阅模式(PUB-SUB)。
 - 气象站范例，Station 将不同地域的天气信息发送到网络上，Client 通过zipcode, 订阅获取某地的天气信息。
 - 在使用SUB套接字时，必须使用zmq_setsockopt()方法来设置订阅的内容。如果你不设置订阅内容，那将什么消息都收不到，新手很容易犯这个错误。
 - 订阅信息可以是任何字符串，可以设置多次。只要消息满足其中一条订阅信息，SUB套接字就会收到。
 - 订阅者可以选择不接收某类消息，也是通过zmq_setsockopt()方法实现的。
 - 如果发布者没有订阅者与之相连，那它发送的消息将直接被丢弃；
 - 如果你使用TCP协议，那当订阅者处理速度过慢时，消息会在发布者处堆积。
 - 在目前版本的ZMQ中，消息的过滤是在订阅者处进行的。也就是说，发布者会向订阅者发送所有的消息，订阅者会将未订阅的消息丢弃。

                         +-------------+
                         |  Publisher  |
                         +-------------+
                         |     PUB     |
                         \-------------/
                              bind
                                |
                                |
                            updates
                                |
                +---------------+---------------+
                |               |               |
             updates         updates         updates
                |               |               |
                |               |               |
                v               v               v
             connect         connect         connect
          /------------\  /------------\  /------------\
          |    SUB     |  |    SUB     |  |    SUB     |
          +------------+  +------------+  +------------+
          | Subscriber |  | Subscriber |  | Subscriber |
          +------------+  +------------+  +------------+

                Figure # - Publish-Subscribe

## Distributed Calc Example

 - 分布式负载均衡计算范例
 - 使用 PUSH-Pull，演示管道模式(Pipeline)
 - PUSH 使用公平队列(fair queue)
    
                      +-------------+
                      |  Ventilator |
                      +-------------+
                      |    PUSH     |
                      \------+------/
                             |
                           tasks
                             |
             +---------------+---------------+
             |               |               |
            task            task             task
             |               |               |
             v               v               v
       /------------\  /------------\  /------------\
       |    PULL    |  |    PULL    |  |    PULL    |
       +------------+  +------------+  +------------+
       |   Worker   |  |   Worker   |  |   Worker   |
       +------------+  +------------+  +------------+
       |    PUSH    |  |    PUSH    |  |    PUSH    |
       \-----+------/  \-----+------/  \-----+------/
             |               |               |
           result          result          result
             |               |               |
             +---------------+---------------+
                             |
                          results
                             |
                             v
                      /-------------\
                      |    PULL     |
                      +-------------+
                      |    Sink     |
                      +-------------+
    
               Figure # - Parallel Pipeline

## Pair Example

 - 演示排他对接模式(Pair)
 - 双工通讯
 - socket 不保存任何状态，可以不用考虑发出的消息是否被读取
 - 一对一的连接
 - Server 侦听端口，Client 连接上。

 例子中 Client 每次发送两个消息，Server 只读取一个，Server 收到的消息会被缓存在队列中，下一次读取。

          +------------+
          |   Server   |
          +------------+
          |    PAIR    |
          \---+--------/
              ^    ^
              |    |
         "info"  "info"
              |    |
              v    v
          /--------+---\
          |    PAIR    |
          +------------+
          |   Client   |
          +------------+       
          
       Figure # - PAIR-PAIR


---------------

"REQ-REP": 使用 REQ-REP 模式，演示 1-N ZMQ 模型


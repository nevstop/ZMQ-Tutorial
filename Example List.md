# Example 列表

## Hello Word Example

 - 使用 REQ-REP 模式，演示 ZMQ 如何工作。
 - 创建一个客户端和一个服务端，客户端发送Hello给服务端，服务端返回World。

          +------------+
          |            |
          |   Server   |
          |            |
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
          |            |
          |   Client   |
          |            |
          +------------+
          
          
       Figure # - Hello Word

## Weather Station Example

 - 演示 Publish-Subscribe 模式
 - 气象站范例，Station 将不同地域的天气信息发送到网络上，Client 通过zipcode, 订阅获取某地的天气信息。
 - 在使用SUB套接字时，必须使用zmq_setsockopt()方法来设置订阅的内容。如果你不设置订阅内容，那将什么消息都收不到，新手很容易犯这个错误。
 - 订阅信息可以是任何字符串，可以设置多次。只要消息满足其中一条订阅信息，SUB套接字就会收到。
 - 订阅者可以选择不接收某类消息，也是通过zmq_setsockopt()方法实现的。
 - 如果发布者没有订阅者与之相连，那它发送的消息将直接被丢弃；
 - 如果你使用TCP协议，那当订阅者处理速度过慢时，消息会在发布者处堆积。
 - 在目前版本的ZMQ中，消息的过滤是在订阅者处进行的。也就是说，发布者会向订阅者发送所有的消息，订阅者会将未订阅的消息丢弃。

                         +-------------+
                         |             |
                         |  Publisher  |
                         |             |
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
          |            |  |            |  |            |
          | Subscriber |  | Subscriber |  | Subscriber |
          |            |  |            |  |            |
          +------------+  +------------+  +------------+


                Figure # - Publish-Subscribe
---------------

"REQ-REP": 使用 REQ-REP 模式，演示 1-N ZMQ 模型
"Distributed Calc": 使用 PUSH-Pull 模式，演示分布式负载均衡计算

# Example 列表

### Hello Word Example

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

---------------

"Weather Station": 使用气象站范例，演示如何使用 Publish-Subscribe 模式，建立单向数据分发。
"REQ-REP": 使用 REQ-REP 模式，演示 1-N ZMQ 模型
"Distributed Calc": 使用 PUSH-Pull 模式，演示分布式负载均衡计算

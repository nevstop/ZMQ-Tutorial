 # ZMQ API

 ZMQ之所那么吸引人眼球，原因之一就是它是建立在标准套接字API之上。因此，ZMQ的套接字操作非常容易理解，其生命周期主要包含四个部分：

 - 创建和销毁套接字：zmq_socket(), zmq_close()
 - 配置和读取套接字选项：zmq_setsockopt(), zmq_getsockopt()
 - 为套接字建立连接：zmq_bind(), zmq_connect()
 - 发送和接收消息：zmq_send(), zmq_recv()
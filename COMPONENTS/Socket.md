# ZMQ Socket Supported Protocols

 由于名称的原因和一部分特性，ZMQ Socket 会经常拿TCP/UDP Socket 进行类比。他们之间的区别：

 - ZMQ 不仅支持 TCP/UDP 协议，也支持多种其他协议：inproc（进程内）、ipc（进程间）、pgm（广播）、epgm等。

 - 流程上，zmq socket 操作更为简单。
    - ZMQ Socket 当客户端使用zmq_connect()时连接就已经建立了，并不要求该端点已有某个服务使用zmq_bind()进行了绑定。
    - ZMQ没有提供类似zmq_accept()的函数，因为当 Socket 绑定至端点时它就自动开始接受连接了.

 - 从传递的内容上：ZMQ Socket 传输的是消息（一段指定长度的二进制数据块），而不是字节（TCP）或帧（UDP）。
 
 - ZMQ Socket 连接是异步的，并由一组消息队列做缓冲；  
 ZMQ Socket 在后台进行I/O操作，无论是接收还是发送消息，它都会先传送到一个本地的缓冲队列，这个内存队列的大小是可以配置的。

 - 从连接数量上而言，TCP协议只能进行点对点的连接，而ZMQ则可以进行一对一、一对多（类似于无线广播）、多对多（类似于邮局）、多对一（类似于信箱）。


**ZMQ 目前提供的协议可以分两类如下**：
 - 单播传输协议（tcp, udp, ipc, inporc，vmic)
 - 广播协议（udp, epgm, pgm)

## TCP协议

- 协议：TCP/IP

- 单播传输协议

- 特点：
    - 最为推荐。灵活、便携、且足够快速，易扩展；
    - 对程序而言是透明的，客户端和服务端可以随时进行连接和绑定。
    - 在网络分布式系统中，TCP应该是首选。

## UDP协议

- 协议：UDP

- 支持单播/组播

- 特点：
    - 灵活、便携、快速、易扩展；
    - UDP is unreliable protocol transport of data over IP networks.
    - UDP transport can only be used with the ZMQ_RADIO and ZMQ_DISH Socket types.
 
## 进程间协议(IPC)  

- 协议：IPC：inter-process communication transport

- 单播传输协议

- 特点：
    - 已从网络传输中抽象出来，不需要指定IP地址或者域名
    - 不能在Windows操作系统上运行，只能在提供 UNIX domain Sockets 的系统上运行。
    - 在UNIX系统上，使用ipc协议还需要注意权限问题，需要保证所有的程序都能够找到这个ipc端点。
    - 后创建的 IPC 连接如果已经存在的话，会覆盖已存在的 IPC 连接。从这一点而言，IPC 相对于 TCP、UDP 而言是易失的。
    - IPC 连接创建的路径必须存在，否则绑定会失败。
    - IPC 路径有最大长度限制，此限制和操作系统定义相关。在 Linux 上，最大长度限制是 113个字符，并且此长度包括 "ipc://" 前缀。

## 进程内协议(inproc)  

- 协议：INPROC:local in-process (inter-thread) communication transport

- 单播传输协议

- 特点：
    - inproc 的消息传递直接通过内存交换进行，因此比ipc或tcp要快得多。
    - inproc 的消息传递，不需要 I/O threads的支持。因此，如果 zmq Context 只用于inproc Socket 类型，可以在初始化时，不创建 I/O threads。
    - inproc 只能用于同一个context 下创建的Sockets 间的通讯，这就意味着只能在同一个进程的不同线程之间进行消息传输
    - 在 v4.x 之前，必须先绑定到端点，它才能建立连接。通常的做法是先启动服务端线程，绑定至端点，后启动客户端线程，连接至端点。在 v4.x 之后，没有启动次序的要求。

### 实际通用组播协议(pgm/epgm) 

 - 协议：PGM (Pragmatic General Multicast)
    -PGM (Pragmatic General Multicast) 
    -ePGM (Encapsulated PGM)

 - 组播协议: 

    - ZMQ 实现了两种 PGM协议, 标准的 PGM 和 ePGM。  
    ZMQ implements two variants of PGM, the standard protocol where PGM datagrams are layered directly on top of IP datagrams as defined by RFC 3208 (the pgm transport) and "Encapsulated PGM" or EPGM where PGM datagrams are encapsulated inside UDP datagrams (the epgm transport).
    - PGM (Pragmatic General Multicast)是基于IP网络的可靠组播协议。
    - PGM/epgm 只能用于 ZMQ_PUB 和 ZMQ_SU B这两种 Socket 类型。
    - PGM Sockets 默认情况下速度是受限的。ZMQ_RATE 和 ZMQ_RECOVERY_IVL 与之相关。
    - PGM 实现需要访问 raw Socket，因此在一些操作系统中，需要额外的权限。如果程序不需要直接和其他 PGM 应用进行相互交互，ePGM 是被推荐的选择，因为它不需要额外的特殊权限。

### 虚拟机通信接口(VMCI)

 - 协议：VMCI (virtual machine communicatios interfacet)

- 单播传输协议

- 特点：
    - VMCI 传输支持在 vmware 虚拟机和宿主机之间、同一个宿主机上的虚拟机之间的消息通讯。(进程间通讯，类似于 IPC)

    - Mac OS X 10.9 及以上，不支持VMCI.
# ZMQ Socket Supported Protocols

 - 单播传输协议（tcp, udp, ipc, inporc)
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
    - UDP transport can only be used with the ZMQ_RADIO and ZMQ_DISH socket types.

## 进程间协议(IPC)  

- 协议：IPC：inter-process communication transport

- 单播传输协议

- 特点：
    - 已从网络传输中抽象出来，不需要指定IP地址或者域名
    - 不能在Windows操作系统上运行，只能在提供 UNIX domain sockets 的系统上运行。
    - 在UNIX系统上，使用ipc协议还需要注意权限问题，需要保证所有的程序都能够找到这个ipc端点。
    - 后创建的 IPC 连接如果已经存在的话，会覆盖已存在的 IPC 连接。从这一点而言，IPC 相对于 TCP、UDP 而言是易失的。
    - IPC 连接创建的路径必须存在，否则绑定会失败。
    - IPC 路径有最大长度限制，此限制和操作系统定义相关。在 Linux 上，最大长度限制是 113个字符，并且此长度包括 "ipc://" 前缀。

## 进程内协议(inproc)  

- 协议：INPROC:local in-process (inter-thread) communication transport

- 单播传输协议

- 特点：
    - inproc 的消息传递直接通过内存交换进行，因此比ipc或tcp要快得多。
    - inproc 的消息传递，不需要 I/O threads的支持。因此，如果 zmq Context 只用于inproc socket 类型，可以在初始化时，不创建 I/O threads。
    - inproc 只能用于同一个context 下创建的sockets 间的通讯，这就意味着只能在同一个进程的不同线程之间进行消息传输
    - 在 v4.x 之前，必须先绑定到端点，它才能建立连接。通常的做法是先启动服务端线程，绑定至端点，后启动客户端线程，连接至端点。在 v4.x 之后，没有启动次序的要求。

### pgm
 - zmq_pgm - ZMQ reliable multicast transport using PGM

        PGM (Pragmatic General Multicast) is a protocol for reliable multicast transport of data over IP networks.

        ØMQ implements two variants of PGM, the standard protocol where PGM datagrams are layered directly on top of IP datagrams as defined by RFC 3208 (the pgm transport) and "Encapsulated PGM" or EPGM where PGM datagrams are encapsulated inside UDP datagrams (the epgm transport).

        The pgm and epgm transports can only be used with the ZMQ_PUB and ZMQ_SUB socket types.

        Further, PGM sockets are rate limited by default. For details, refer to the ZMQ_RATE, and ZMQ_RECOVERY_IVL options documented in zmq_setsockopt(3).

        The pgm transport implementation requires access to raw IP sockets. Additional privileges may be required on some operating systems for this operation. Applications not requiring direct interoperability with other PGM implementations are encouraged to use the epgm transport instead which does not require any special privileges.

### epgm

### zmq_vmci - ZMQ transport over virtual machine communicatios interface (VMCI) sockets

        The VMCI transport passes messages between VMware virtual machines running on the same host, between virtual machine and the host and within virtual machines (inter-process transport like ipc).

        Communication between a virtual machine and the host is not supported on Mac OS X 10.9 and above.



# ZMQ Socket Supported Protocols

 - 单播传输协议（inporc, ipc, tcp）
 - 广播协议（epgm, pgm)

 ## 单播传输协议

 - TCP传输协议
    - 优点：
        - 最为推荐。灵活、便携、且足够快速，易扩展；
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

## 广播协议

 - pgm
 - epgm

## ZMQ Poller

## ZMQ Protocol

 - zmq_tcp - ZMQ unicast transport using TCP 

        TCP is an ubiquitous, reliable, unicast transport. When connecting distributed applications over a network with ØMQ, using the TCP transport will likely be your first choice.

 - zmq_udp - ZMQ UDP multicast and unicast transport 

        Synopsis

        UDP is unreliable protocol transport of data over IP networks. UDP support both unicast and multicast communication.

        Description

        UDP transport can only be used with the ZMQ_RADIO and ZMQ_DISH socket types.

 - zmq_ipc - ZMQ local inter-process communication transport  

        The inter-process transport passes messages between local processes using a system-dependent IPC mechanism.

        The inter-process transport is currently only implemented on operating systems that provide UNIX domain sockets.

        any existing binding to the same endpoint shall be overridden. That is, if a second process binds to an endpoint already bound by a process, this will succeed and the first process will lose its binding. In this behaviour, the ipc transport is not consistent with the tcp or inproc transports.

        the endpoint pathname must be writable by the process. When the endpoint starts with /, e.g., ipc:///pathname, this will be an absolute pathname. If the endpoint specifies a directory that does not exist, the bind shall fail.

        on Linux only, when the endpoint pathname starts with @, the abstract namespace shall be used. The abstract namespace is independent of the filesystem and if a process attempts to bind an endpoint already bound by a process, it will fail. See unix(7) for details.

        IPC pathnames have a maximum size that depends on the operating system. On Linux, the maximum is 113 characters including the "ipc://" prefix (107 characters for the real path name).

 - zmq_inproc - ZMQ local in-process (inter-thread) communication transport  

        The in-process transport passes messages via memory directly between threads sharing a single ØMQ context.

        No I/O threads are involved in passing messages using the inproc transport. Therefore, if you are using a ØMQ context for in-process messaging only you can initialise the context with zero I/O threads. See zmq_init(3) for details.

        When connecting a socket to a peer address using zmq_connect() with the inproc transport, the endpoint shall be interpreted as an arbitrary string identifying the name to connect to. Before version 4.0 he name must have been previously created by assigning it to at least one socket within the same ØMQ context as the socket being connected. Since version 4.0 the order of zmq_bind() and zmq_connect() does not matter just like for the tcp transport type.

 - zmq_pgm - ZMQ reliable multicast transport using PGM

        PGM (Pragmatic General Multicast) is a protocol for reliable multicast transport of data over IP networks.

        ØMQ implements two variants of PGM, the standard protocol where PGM datagrams are layered directly on top of IP datagrams as defined by RFC 3208 (the pgm transport) and "Encapsulated PGM" or EPGM where PGM datagrams are encapsulated inside UDP datagrams (the epgm transport).

        The pgm and epgm transports can only be used with the ZMQ_PUB and ZMQ_SUB socket types.

        Further, PGM sockets are rate limited by default. For details, refer to the ZMQ_RATE, and ZMQ_RECOVERY_IVL options documented in zmq_setsockopt(3).

        The pgm transport implementation requires access to raw IP sockets. Additional privileges may be required on some operating systems for this operation. Applications not requiring direct interoperability with other PGM implementations are encouraged to use the epgm transport instead which does not require any special privileges.
 
 - zmq_vmci - ZMQ transport over virtual machine communicatios interface (VMCI) sockets

        The VMCI transport passes messages between VMware virtual machines running on the same host, between virtual machine and the host and within virtual machines (inter-process transport like ipc).

        Communication between a virtual machine and the host is not supported on Mac OS X 10.9 and above.

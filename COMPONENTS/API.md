# ZMQ API

**网页参考**：[ZMQ 4.x API Webpage](http://api.zeromq.org/)

ZMQ之所那么吸引人眼球，原因之一就是它是建立在标准套接字API之上。因此，ZMQ的套接字操作非常容易理解，其生命周期主要包含四个部分：

 - 创建和销毁套接字：zmq_socket(), zmq_close()
 - 配置和读取套接字选项：zmq_setsockopt(), zmq_getsockopt()
 - 为套接字建立连接：zmq_bind(), zmq_connect()
 - 发送和接收消息：zmq_send(), zmq_recv()
  
[-] zmq - ZMQ lightweight messaging kernel  
[-] zmq_atomic_counter_dec - decrement an atomic counter  
[-] zmq_atomic_counter_destroy - destroy an atomic counter  
[-] zmq_atomic_counter_inc - increment an atomic counter  
[-] zmq_atomic_counter_new - create a new atomic counter  
[-] zmq_atomic_counter_set - set atomic counter to new value  
[-] zmq_atomic_counter_value - return value of atomic counter  
[-] zmq_close - close ZMQ socket  
[-] zmq_curve_keypair - generate a new CURVE keypair  
[-] zmq_curve_public - derive the public key from a private key  
[-] zmq_curve - secure authentication and confidentiality  
[-] zmq_errno - retrieve value of errno for the calling thread  
[-] zmq_getsockopt - get ZMQ socket options  
[-] zmq_gssapi - secure authentication and confidentiality   
[-] zmq_msg_close - release ZMQ message  
[-] zmq_msg_copy - copy content of a message to another message  
[-] zmq_msg_data - retrieve pointer to message content  
[-] zmq_msg_gets - get message metadata property  
[-] zmq_msg_get - get message property  
[-] zmq_msg_init_data - initialise ZMQ message from a supplied buffer  
[-] zmq_msg_init_size - initialise ZMQ message of a specified size  
[-] zmq_msg_init - initialise empty ZMQ message  
[-] zmq_msg_more - indicate if there are more message parts to receive  
[-] zmq_msg_move - move content of a message to another message  
[-] zmq_msg_recv - receive a message part from a socket  
[-] zmq_msg_routing_id - return routing ID for message, if any  
[-] zmq_msg_send - send a message part on a socket  
[-] zmq_msg_set_routing_id - set routing ID property on message  
[-] zmq_msg_set - set message property  
[-] zmq_msg_size - retrieve message content size in bytes  
[-] zmq_null - no security or confidentiality  
[-] zmq_plain - clear-text authentication  
[-] zmq_poll - input/output multiplexing  
[-] zmq_proxy_steerable - built-in ZMQ proxy with control flow  
[-] zmq_proxy - start built-in ZMQ proxy  
[-] zmq_recvmsg - receive a message part from a socket  
[-] zmq_recv - receive a message part from a socket  
[-] zmq_send_const - send a constant-memory message part on a socket  
[-] zmq_sendmsg - send a message part on a socket  
[-] zmq_send - send a message part on a socket  
[-] zmq_setsockopt - set ZMQ socket options  
[-] zmq_socket_monitor - monitor socket events  
[-] zmq_socket - create ZMQ socket  
[-] zmq_strerror - get ZMQ error message string  
[-] zmq_tipc - ZMQ unicast transport using TIPC  
[-] zmq_z85_decode - decode a binary key from Z85 printable text  
[-] zmq_z85_encode - encode a binary key as Z85 printable text

## ZMQ Info

- zmq_version - report ZMQ library version 
- zmq_has - check a ZMQ capability(protocol/security mechanism)

## ZMQ Context

- zmq_ctx_new - create new ZMQ context 
- [deprecated]zmq_init - initialise ZMQ context  

        The zmq_ctx_new() function creates a new ØMQ context.

- zmq_ctx_set - set context options  
- zmq_ctx_get - get context options  

        - ZMQ_IO_THREADS: Get number of I/O threads
        - ZMQ_MAX_SOCKETS: Get maximum number of sockets
        - ZMQ_MAX_MSGSZ: Get maximum message size(Default value is INT_MAX)
        - ZMQ_SOCKET_LIMIT: Get largest configurable number of sockets
        - ZMQ_IPV6: Set IPv6 option
        - ZMQ_BLOCKY: Get blocky setting
        - ZMQ_MSG_T_SIZE: Get the zmq_msg_t size at runtime

- zmq_ctx_shutdown - shutdown a ZMQ context  

        The zmq_ctx_shutdown() function shall shutdown the ØMQ context context.

        Context shutdown will cause any blocking operations currently in progress on sockets open within context to return immediately with an error code of ETERM. With the exception of zmq_close(), any further operations on sockets open within context shall fail with an error code of ETERM.

        This function is optional, client code is still required to call the zmq_ctx_term(3) function to free all resources allocated by zeromq.

- zmq_ctx_term - terminate a ZMQ context  
- [deprecated]zmq_term - terminate ZMQ context  
- [deprecated]zmq_ctx_destroy - terminate a ZMQ context 

        The zmq_ctx_term() function shall destroy the ØMQ context context.

        Context termination is performed in the following steps:

        Any blocking operations currently in progress on sockets open within context shall return immediately with an error code of ETERM. With the exception of zmq_close(), any further operations on sockets open within context shall fail with an error code of ETERM.
        After interrupting all blocking calls, zmq_ctx_term() shall block until the following conditions are satisfied:
        All sockets open within context have been closed with zmq_close().
        For each socket within context, all messages sent by the application with zmq_send() have either been physically transferred to a network peer, or the socket's linger period set with the ZMQ_LINGER socket option has expired.
        For further details regarding socket linger behaviour refer to the ZMQ_LINGER option in zmq_setsockopt(3).


## ZMQ Socket

 - zmq_close - close ZMQ socket  

 - zmq_bind - accept incoming connections on a socket
        The zmq_bind() function binds the socket to a local endpoint and then accepts incoming connections on that endpoint.

 - zmq_unbind - Stop accepting connections on a socket

 - zmq_connect - create outgoing connection from socket 
        The zmq_connect() function connects the socket to an endpoint and then accepts incoming connections on that endpoint

 - zmq_disconnect - Disconnect a socket

        The zmq_disconnect() function shall disconnect a socket specified by the socket argument from the endpoint specified by the endpoint argument. Any outstanding messages physically received from the network but not yet received by the application with zmq_recv() shall be discarded. The behaviour for discarding messages sent by the application with zmq_send() but not yet physically transferred to the network depends on the value of the ZMQ_LINGER socket option for the specified socket.

        The endpoint argument is as described in zmq_connect(3)

        The default setting of ZMQ_LINGER does not discard unsent messages; this behaviour may cause the application to block when calling zmq_ctx_term(). For details refer to zmq_setsockopt(3) and zmq_ctx_term(3).


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

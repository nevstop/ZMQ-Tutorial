# ZMQ API

**网页参考**：[ZMQ 4.x API Webpage](http://api.zeromq.org/)

The ZMQ lightweight messaging kernel is a library which extends the standard socket interfaces with features traditionally provided by specialised messaging middleware products. ZMQ sockets provide an abstraction of asynchronous message queues, multiple messaging patterns, message filtering (subscriptions), seamless access to multiple transport protocols and more.

This documentation presents an overview of ZMQ concepts, describes how ZMQ abstracts standard sockets and provides a reference manual for the functions provided by the ZMQ library.

## Context

The ZMQ context keeps the list of sockets and manages the async I/O thread and internal queries.

Before using any ZMQ library functions you must create a ZMQ context. When you exit your application you must destroy the context. These functions let you work with contexts:

### Create a new ZMQ context
- zmq_ctx_new - create new ZMQ context 
- [deprecated]zmq_init - initialise ZMQ context  

### Work with context properties
- zmq_ctx_set - set context options  
- zmq_ctx_get - get context options  

        - ZMQ_IO_THREADS: Get number of I/O threads
        - ZMQ_MAX_SOCKETS: Get maximum number of sockets
        - ZMQ_MAX_MSGSZ: Get maximum message size(Default value is INT_MAX)
        - ZMQ_SOCKET_LIMIT: Get largest configurable number of sockets
        - ZMQ_IPV6: Set IPv6 option
        - ZMQ_BLOCKY: Get blocky setting
        - ZMQ_MSG_T_SIZE: Get the zmq_msg_t size at runtime

### Destroy a ZMQ context
- zmq_ctx_shutdown - shutdown a ZMQ context  

        The zmq_ctx_shutdown() function shall shutdown the ZMQ context context.

        Context shutdown will cause any blocking operations currently in progress on sockets open within context to return immediately with an error code of ETERM. With the exception of zmq_close(), any further operations on sockets open within context shall fail with an error code of ETERM.

        This function is optional, client code is still required to call the zmq_ctx_term(3) function to free all resources allocated by zeromq.

- zmq_ctx_term - terminate a ZMQ context  
- [deprecated]zmq_term - terminate ZMQ context  
- [deprecated]zmq_ctx_destroy - terminate a ZMQ context 

        The zmq_ctx_term() function shall destroy the ZMQ context context.

        Context termination is performed in the following steps:

        Any blocking operations currently in progress on sockets open within context shall return immediately with an error code of ETERM. With the exception of zmq_close(), any further operations on sockets open within context shall fail with an error code of ETERM.
        After interrupting all blocking calls, zmq_ctx_term() shall block until the following conditions are satisfied:
        All sockets open within context have been closed with zmq_close().
        For each socket within context, all messages sent by the application with zmq_send() have either been physically transferred to a network peer, or the socket's linger period set with the ZMQ_LINGER socket option has expired.
        For further details regarding socket linger behaviour refer to the ZMQ_LINGER option in zmq_setsockopt(3).

## Messages

A ZMQ message is a discrete unit of data passed between applications or components of the same application. ZMQ messages have no internal structure and from the point of view of ZMQ itself they are considered to be opaque binary data.

Never access zmq_msg_t members directly, instead always use the zmq_msg family of functions.

### Initialise a message
 - zmq_msg_init_data - initialise ZMQ message from a supplied buffer  
 - zmq_msg_init_size - initialise ZMQ message of a specified size  
 - zmq_msg_init - initialise empty ZMQ message  

### Release a message
 - zmq_msg_close - release ZMQ message

### Sending and receiving a message
 - zmq_msg_send - send a message part on a socket  
 - zmq_msg_recv - receive a message part from a socket 
 - [deprecated]zmq_recvmsg - receive a message part from a socket  
 - [deprecated]zmq_sendmsg - send a message part on a socket
 - zmq_recv - receive a message part from a socket  
 - zmq_send - send a message part on a socket   
 
        ZMQ_DONTWAIT
        For socket types (DEALER, PUSH) that block when there are no available peers (or all peers have full high-water mark), specifies that the operation should be performed in non-blocking mode. If the message cannot be queued on the socket, the zmq_msg_send() function shall fail with errno set to EAGAIN.
        ZMQ_SNDMORE
        Specifies that the message being sent is a multi-part message, and that further message parts are to follow. Refer to the section regarding multi-part messages below for a detailed description.

### Access message content
 - zmq_msg_data - retrieve pointer to message content  
 - zmq_msg_size - retrieve message content size in bytes  
 - zmq_msg_more - indicate if there are more message parts to receive  

### Work with message properties
 - zmq_msg_set - set message property  
 - zmq_msg_gets - get message metadata property  
 - zmq_msg_get - get message property 

### Message manipulation
 - zmq_msg_copy - copy content of a message to another message
 - zmq_msg_move - move content of a message to another message 

### Routing ID
 - zmq_msg_routing_id - return routing ID for message, if any  
 - zmq_msg_set_routing_id - set routing ID property on message 

## Sockets

ZMQ sockets present an abstraction of a asynchronous message queue, with the exact queueing semantics depending on the socket type in use. See zmq_socket(3) for the socket types provided.

### Creating a socket
 - zmq_socket - create ZMQ socket

### Closing a socket
 - zmq_close - close ZMQ socket  

### Establishing a message flow
 - zmq_bind - accept incoming connections on a socket
        The zmq_bind() function binds the socket to a local endpoint and then accepts incoming connections on that endpoint.

 - zmq_unbind - Stop accepting connections on a socket

 - zmq_connect - create outgoing connection from socket 
        The zmq_connect() function connects the socket to an endpoint and then accepts incoming connections on that endpoint

 - zmq_disconnect - Disconnect a socket

        The zmq_disconnect() function shall disconnect a socket specified by the socket argument from the endpoint specified by the endpoint argument. Any outstanding messages physically received from the network but not yet received by the application with zmq_recv() shall be discarded. The behaviour for discarding messages sent by the application with zmq_send() but not yet physically transferred to the network depends on the value of the ZMQ_LINGER socket option for the specified socket.

        The endpoint argument is as described in zmq_connect(3)

        The default setting of ZMQ_LINGER does not discard unsent messages; this behaviour may cause the application to block when calling zmq_ctx_term(). For details refer to zmq_setsockopt(3) and zmq_ctx_term(3).

### Sending and receiving messages
 - zmq_msg_send - send a message part on a socket  
 - zmq_msg_recv - receive a message part from a socket 
 - zmq_recv - receive a message part from a socket  
 - zmq_send - send a message part on a socket  

 - zmq_send_const - send a constant-memory message part on a socket   
 
        The zmq_send_const() function shall queue a message created from the buffer referenced by the buf and len arguments. The message buffer is assumed to be constant-memory and will therefore not be copied or deallocated in any way. 

### Manipulating socket options
 - zmq_setsockopt - set ZMQ socket options
 - zmq_getsockopt - get ZMQ socket options 

         - ZMQ_AFFINITY: Retrieve I/O thread affinity
         - ZMQ_BACKLOG: Retrieve maximum length of the queue of outstanding connections
         - ZMQ_CONNECT_TIMEOUT: Retrieve connect() timeout
         - ZMQ_CURVE_PUBLICKEY: Retrieve current CURVE public key
         - ZMQ_CURVE_SECRETKEY: Retrieve current CURVE secret key
         - ZMQ_CURVE_SERVERKEY: Retrieve current CURVE server key
         - ZMQ_EVENTS: Retrieve socket event state
                 - ZMQ_POLLIN
                 - ZMQ_POLLOUT
         - ZMQ_FD: Retrieve file descriptor associated with the socket
         - ZMQ_GSSAPI_PLAINTEXT: Retrieve GSSAPI plaintext or encrypted status
         - ZMQ_GSSAPI_PRINCIPAL: Retrieve the name of the GSSAPI principal
         - ZMQ_GSSAPI_SERVER: Retrieve current GSSAPI server role
         - ZMQ_GSSAPI_SERVICE_PRINCIPAL: Retrieve the name of the GSSAPI service principal
         - ZMQ_HANDSHAKE_IVL: Retrieve maximum handshake interval
         - ZMQ_IDENTITY: Retrieve socket identity
         - ZMQ_IMMEDIATE: Retrieve attach-on-connect value
         - ZMQ_INVERT_MATCHING: Retrieve inverted filtering status
         - ZMQ_IPV4ONLY: Retrieve IPv4-only socket override status
         - ZMQ_IPV6: Retrieve IPv6 socket status
         - ZMQ_LAST_ENDPOINT: Retrieve the last endpoint set
         - ZMQ_LINGER: Retrieve linger period for socket shutdown
         - ZMQ_MAXMSGSIZE: Maximum acceptable inbound message size
         - ZMQ_MECHANISM: Retrieve current security mechanism
         - ZMQ_MULTICAST_HOPS: Maximum network hops for multicast packets
         - ZMQ_MULTICAST_MAXTPDU: Maximum transport data unit size for multicast packets
         - ZMQ_PLAIN_PASSWORD: Retrieve current password
         - ZMQ_PLAIN_SERVER: Retrieve current PLAIN server role
         - ZMQ_PLAIN_USERNAME: Retrieve current PLAIN username
         - ZMQ_USE_FD: Retrieve the pre-allocated socket file descriptor
         - ZMQ_RATE: Retrieve multicast data rate
         - ZMQ_RCVBUF: Retrieve kernel receive buffer size
         - ZMQ_RCVHWM: Retrieve high water mark for inbound messages
         - ZMQ_RCVMORE: More message data parts to follow
         - ZMQ_RCVTIMEO: Maximum time before a socket operation returns with EAGAIN
         - ZMQ_RECONNECT_IVL: Retrieve reconnection interval
         - ZMQ_RECONNECT_IVL_MAX: Retrieve maximum reconnection interval
         - ZMQ_RECOVERY_IVL: Get multicast recovery interval
         - ZMQ_SNDBUF: Retrieve kernel transmit buffer size
         - ZMQ_SNDHWM: Retrieves high water mark for outbound messages
         - ZMQ_SNDTIMEO: Maximum time before a socket operation returns with EAGAIN
         - ZMQ_SOCKS_PROXY: Retrieve SOCKS5 proxy address
         - ZMQ_TCP_KEEPALIVE: Override SO_KEEPALIVE socket option
         - ZMQ_TCP_KEEPALIVE_CNT: Override TCP_KEEPCNT socket option
         - ZMQ_TCP_KEEPALIVE_IDLE: Override TCP_KEEPIDLE (or TCP_KEEPALIVE on some OS)
         - ZMQ_TCP_KEEPALIVE_INTVL: Override TCP_KEEPINTVL socket option
         - ZMQ_TCP_MAXRT: Retrieve Max TCP Retransmit Timeout
         - ZMQ_THREAD_SAFE: Retrieve socket thread safety
         - ZMQ_TOS: Retrieve the Type-of-Service socket override status
         - ZMQ_TYPE: Retrieve socket type
         - ZMQ_ZAP_DOMAIN: Retrieve RFC 27 authentication domain
         - ZMQ_VMCI_BUFFER_SIZE: Retrieve buffer size of the VMCI socket
         - ZMQ_VMCI_BUFFER_MIN_SIZE: Retrieve min buffer size of the VMCI socket
         - ZMQ_VMCI_BUFFER_MAX_SIZE: Retrieve max buffer size of the VMCI socket
         - ZMQ_VMCI_CONNECT_TIMEOUT: Retrieve connection timeout of the VMCI socket

### Monitoring socket events

 The zmq_socket_monitor() method lets an application thread track socket events (like connects) on a ZeroMQ socket. Each call to this method creates a ZMQ_PAIR socket and binds that to the specified inproc:// endpoint. To collect the socket events, you must create your own ZMQ_PAIR socket, and connect that to the endpoint.
 
 The _zmq_socket_monitor()_ method supports only connection-oriented
transports, that is, TCP, IPC, and TIPC.

 - zmq_socket_monitor - monitor socket events

Supported events 

        - ZMQ_EVENT_CONNECTED  
        - ZMQ_EVENT_CONNECT_DELAYED  
        - ZMQ_EVENT_CONNECT_RETRIED  
        - ZMQ_EVENT_LISTENING  
        - ZMQ_EVENT_BIND_FAILED  
        - ZMQ_EVENT_ACCEPTED  
        - ZMQ_EVENT_ACCEPT_FAILED  
        - ZMQ_EVENT_CLOSED  
        - ZMQ_EVENT_CLOSE_FAILED  
        - ZMQ_EVENT_DISCONNECTED  
        - ZMQ_EVENT_MONITOR_STOPPED  
        - ZMQ_EVENT_HANDSHAKE_FAILED  
        - ZMQ_EVENT_HANDSHAKE_SUCCEED  

### Input/output multiplexing

ZMQ provides a mechanism for applications to multiplex input/output events over a set containing both ZMQ sockets and standard sockets. This mechanism mirrors the standard poll() system call.

 - zmq_poll - input/output multiplexing  

## Transports

A ZMQ socket can use multiple different underlying transport mechanisms. Each transport mechanism is suited to a particular purpose and has its own advantages and drawbacks.

The following transport mechanisms are provided:
 - zmq_tcp - Unicast transport using TCP
 - zmq_pgm - Reliable multicast transport using PGM
 - zmq_ipc - Local inter-process communication transport
 - zmq_inproc - Local in-process (inter-thread) communication transport
 - zmq_vmci - Virtual Machine Communications Interface (VMC) transport
 - zmq_udp - Unreliable unicast and multicast using UDP
 - zmq_tipc - ZMQ unicast transport using TIPC  

## Proxies

ZMQ provides proxies to create fanout and fan-in topologies. A proxy connects a frontend socket to a backend socket and switches all messages between the two sockets, opaquely. A proxy may optionally capture all traffic to a third socket. To start a proxy in an application thread, use zmq_proxy(3). 

 - zmq_proxy - start built-in ZMQ proxy     
 - zmq_proxy_steerable - built-in ZMQ proxy with control flow 

## Security

A ZMQ socket can select a security mechanism. Both peers must use the same security mechanism.  
The following security mechanisms are provided for IPC and TCP connections.

 - zmq_null - Plain-text authentication using username and password
 - zmq_plain - Elliptic curve authentication and encryption
 - zmq_curve - Generate a CURVE keypair in armored text format   

        - zmq_curve_keypair - generate a new CURVE keypair
        - zmq_curve_public - derive the public key from a private key
 - zmq_gssapi - secure authentication and confidentiality 

Converting keys to/from armoured text strings  
 - zmq_z85_decode - decode a binary key from Z85 printable text  
 - zmq_z85_encode - encode a binary key as Z85 printable text

## Error handling

The ZMQ library functions handle errors using the standard conventions found on POSIX systems. Generally, this means that upon failure a ZMQ library function shall return either a NULL value (if returning a pointer) or a negative value (if returning an integer), and the actual error code shall be stored in the errno variable.

On non-POSIX systems some users may experience issues with retrieving the correct value of the errno variable. The zmq_errno() function is provided to assist in these cases; for details refer to zmq_errno(3).

The zmq_strerror() function is provided to translate ZMQ-specific error codes into error message strings; for details refer to zmq_strerror(3).

 - zmq_errno - retrieve value of errno for the calling thread 
 - zmq_strerror - get ZMQ error message string  

## Utility

### Atomic counters
You can use this in multithreaded applications to do, for example, reference counting of shared objects. The atomic counter is at least 32 bits large. This function uses platform specific atomic operations.

 - zmq_atomic_counter_new - create a new atomic counter
 - zmq_atomic_counter_destroy - destroy an atomic counter
 - zmq_atomic_counter_set - set atomic counter to new value
 - zmq_atomic_counter_value - return value of atomic counter
 - zmq_atomic_counter_inc - increment an atomic counter
 - zmq_atomic_counter_dec - decrement an atomic counter

## Miscellaneous

The following miscellaneous functions are provided:

Report ZMQ library version
 - zmq_version

Check a ZMQ capability
- zmq_has - check a ZMQ capability(protocol/security mechanism)

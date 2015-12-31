---
title: Python Socket
layout: post
tags:
  - python
  - socket

---

*Python Socket*

Create a new socket using the given address family, socket type and protocol number.
The address family should be AF_INET (the default), AF_INET6 or AF_UNIX.
The socket type should be SOCK_STREAM (the default), SOCK_DGRAM or perhaps one of the other SOCK_ constants.
The protocol number is usually zero and may be omitted in that case.

    import socket

    socket.socket([family[, type[, proto]]])


[官网socket echo 服务例子](https://docs.python.org/2/library/socket.html#example)

    # Echo server program
    import socket

    HOST = 'localhost'
    PORT = 50007
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind((HOST, PORT))
    s.listen(1)
    conn, addr = s.accept()
    print 'Connected by', addr
    while 1:
        data = conn.recv(1024)
        if not data: break
        conn.sendall(data)
    conn.close()

    # Echo client program
    import socket

    HOST = 'localhost'
    PORT = 50007
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((HOST, PORT))
    s.sendall('Hello, world')
    data = s.recv(1024)
    s.close()
    print 'Received', repr(data)


Family参数指定调用者期待返回的套接口地址结构的类型.

它的值包括：AF_INET[2], AF_INET6[10], AF_UNIX[1], AF_UNSPEC[0] 等. ('[]'中整型数字代表实际socket.AF_XXX代表的值)

如果指定AF_INET, 那么不能返回任何IPV6相关的地址信息; 如果仅指定了AF_INET6, 则就不能返回任何IPV4地址信息;
如果指定 AF_UNSPEC 则意味着函数返回的是适用于指定主机名和服务名且适合任何协议族的地址.

如果某个主机既有AAAA记录(IPV6)地址, 同时又有A记录(IPV4)地址, 那么AAAA记录将作为sockaddr_in6结构返回, 而A记录则作为sockaddr_in结构返回.

**为了避免指定某一个ipv4或者ipv6, 可以使用socket.getaddrinfo来获取5元组**

    socket.getaddrinfo(host, port[, family[, socktype[, proto[, flags]]]])

Translate the host/port argument into a sequence of 5-tuples that contain all the necessary arguments for creating a socket connected to that service.

* host is a domain name, a string representation of an IPv4/v6 address or None.

* port is a string service name such as 'http', a numeric port number or None.

By passing None as the value of host and port, you can pass NULL to the underlying C API.

* The family, socktype and proto arguments can be optionally specified in order to narrow the list of addresses returned.

By default, their value is 0, meaning that the full range of results is selected.

* The flags argument can be one or several of the AI_* constants, and will influence how results are computed and returned.

Its default value is 0. For example, AI_NUMERICHOST will disable domain name resolution and will raise an error if host is a domain name.

* The function returns a list of 5-tuples with the following structure: (family, socktype, proto, canonname, sockaddr)

In these tuples, family, socktype, proto are all integers and are meant to be passed to the socket() function.

canonname will be a string representing the canonical name of the host if AI_CANONNAME is part of the flags argument; else canonname will be empty.
sockaddr is a tuple describing a socket address, whose format depends on the returned family (a (address, port) 2-tuple for AF_INET, a (address, port, flow info, scope id) 4-tuple for AF_INET6), and is meant to be passed to the socket.connect() method.

The following example fetches address information for a hypothetical TCP connection to www.python.org on port 80 (results may differ on your system if IPv6 isn’t enabled):

    >>>
    >>> socket.getaddrinfo("www.python.org", 80, 0, 0, socket.SOL_TCP)
    [ (2, 1, 6, '', ('82.94.164.162', 80)),
      (10, 1, 6, '', ('2001:888:2000:d::a2', 80, 0, 0)) ]

在redis-py中获取一个socket实例：

    sock = self._connect()

    def _connect(self):
        err = None
        # TODO 使用 socket.getaddrinfo, socket.SOCK_STREAM: 表示使用TCP
        for res in socket.getaddrinfo(self.host, self.port, 0, socket.SOCK_STREAM):
            family, socktype, proto, canonname, socket_address = res
            sock = None
            try:
                sock = socket.socket(family, socktype, proto)
                # TCP_NODELAY TODO 禁止发送合并的Nagle算法.
                sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)

                # TCP_KEEPALIVE
                if self.socket_keepalive:
                    sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
                    for k, v in iteritems(self.socket_keepalice_options):
                        sock.setsockopt(socket.SOL_TCP, k, v)

                # TODO 为啥使用两次settimeout?
                # set the socket_connect_timeout before we connect
                sock.settimeout(self.socket_connection_timeout)

                # connect
                sock.connect(socket_address)

                # set the socket_timeout now that we're connected
                sock.settimeout(self.socket_timeout)
                return sock

            except socket.error as _:
                err = _
                if sock is not None:
                    sock.close()

        if err is not None:
            raise err
        raise socket.error("socket.getaddrinfo returned an empty list")


**上例中使用到了socket.setsockopt(level, optname, value)**

    socket.setsockopt(level, optname, value)

Set the value of the given socket option (see the Unix manual page setsockopt(2)).
The needed symbolic constants are defined in the socket module (SO_* etc.).
The value can be an integer or a string representing a buffer.
In the latter case it is up to the caller to ensure that the string contains the proper bits (see the optional built-in module struct for a way to encode C structures as strings).


用于任意类型, 任意状态套接口的设置选项值. 尽管在不同协议层上存在选项, 但本函数仅定义了最高的"套接口"层次上的选项. 选项影响套接口的操作, 诸如加急数据是否在普通数据流中接收, 广播数据是否可以从套接口发送等等.
有两种套接口的选项: 一种是布尔型选项, 允许或禁止一种特性; 另一种是整形或结构选项.

第一个参数为协议层参数, 指明了希望访问一个选项所在的协议栈. 通常我们需要使用下面中的一个：

    SOL_SOCKET      来访问套接口层选项
    SOL_TCP         来访问TCP层选项

第二个参数是与第一个参数相对应的. 第一个参数决定了协议层level, 第二个参数决定了该协议层下选项组合. SOL_SOCKET的选项组合如下：

    协议层           选项名字                  意义
    SOL_SOCKET      SO_REUSEADDR        允许套接口和一个已在使用中的地址捆绑（参见bind()）。
    SOL_SOCKET      SO_KEEPALIVE        发送“保持活动”包。
    SOL_SOCKET      SO_LINGER           如关闭时有未发送数据，则逗留。
    SOL_SOCKET      SO_BROADCAST        允许套接口传送广播信息。
    SOL_SOCKET      SO_OOBINLINE        在常规数据流中接收带外数据。
    SOL_SOCKET      SO_SNDBUF           指定发送缓冲区大小。
    SOL_SOCKET      SO_RCVBUF           为接收确定缓冲区大小。
    SOL_SOCKET      SO_TYPE             套接口类型。
    SOL_SOCKET      SO_ERROR            获取错误状态并清除。
                    SO_DEBUG            记录调试信息。
                    SO_DONTLINER        不要因为数据未发送就阻塞关闭操作。设置本选项相当于将SO_LINGER的l_onoff元素置为零。
                    SO_DONTROUTE        禁止选径；直接传送。
    IPPROTO_TCP     TCP_NODELAY         禁止发送合并的Nagle算法。
                    SO_RCVLOWAT         接收低级水印。
                    SO_RCVTIMEO         接收超时。
                    SO_SNDLOWAT         发送低级水印。
                    SO_SNDTIMEO         发送超时。
                    IP_OPTIONS          在IP头中设置选项。

常用的3个设置

    1. 缺省条件下, 一个套接口不能与一个已在使用中的本地地址捆绑（参见bind()）.但有时会需要“重用”地址.
    因为每一个连接都由本地地址和远端地址的组 合唯一确定, 所以只要远端地址不同, 两个套接口与一个地址捆绑并无大碍.
    为了通知套接口实现不要因为一个地址已被一个套接口使用就不让它与 另一个套接口捆绑, 应用程序可在bind()调用前先设置SO_REUSEADDR选项.
    请注意仅在bind()调用时该选项才被解释；故此无需（但也无 害）将一个不会共用地址的套接口设置该选项，或者在bind()对这个或其他套接口无影响情况下设置或清除这一选项。

    2. 一个应用程序可以通过打开SO_KEEPALIVE选项，使得套接口实现在TCP连接情况下允许使用“保持活动”包。
    一个套 接口实现并不是必需支持“保持活动”，但是如果支持的话，具体的语义将与实现有关，应遵守RFC1122“Internet主机要求－通讯层”中第 4.2.3.6节的规范。
    如果有关连接由于“保持活动”而失效，则进行中的任何对该套接口的调用都将以WSAENETRESET错误返回，后续的任何调用 将以WSAENOTCONN错误返回。

    3. TCP_NODELAY选项禁止Nagle算法。Nagle算法通过将未确认的数据存入缓冲区直到蓄足一个包一起发送的方法，来减少主机发送的零碎小数据 包的数目。
    但对于某些应用来说，这种算法将降低系统性能。所以TCP_NODELAY可用来将此算法关闭。应用程序编写者只有在确切了解它的效果并确实需 要的情况下，才设置TCP_NODELAY选项，因为设置后对网络性能有明显的负面影响。
    TCP_NODELAY是唯一使用IPPROTO_TCP层的选 项，其他所有选项都使用SOL_SOCKET层。

其他可能遇到的情况

    1.closesocket（一般不会立即关闭而经历TIME_WAIT的过程）后想继续重用该socket：
    BOOL bReuseaddr=TRUE;
    setsockopt(s,SOL_SOCKET ,SO_REUSEADDR,(const char*)&bReuseaddr,sizeof(BOOL));

    2. 如果要已经处于连接状态的soket在调用closesocket后强制关闭，不经历
    TIME_WAIT的过程：
    BOOL bDontLinger = FALSE;
    setsockopt(s,SOL_SOCKET,SO_DONTLINGER,(const char*)&bDontLinger,sizeof(BOOL));

    3.在send(),recv()过程中有时由于网络状况等原因，发收不能预期进行,而设置收发时限：
    int nNetTimeout=1000;//1秒
    //发送时限
    setsockopt(socket，SOL_S0CKET,SO_SNDTIMEO，(char *)&nNetTimeout,sizeof(int));
    //接收时限
    setsockopt(socket，SOL_S0CKET,SO_RCVTIMEO，(char *)&nNetTimeout,sizeof(int));

    4.在send()的时候，返回的是实际发送出去的字节(同步)或发送到socket缓冲区的字节
    (异步);系统默认的状态发送和接收一次为8688字节(约为8.5K)；在实际的过程中发送数据
    和接收数据量比较大，可以设置socket缓冲区，而避免了send(),recv()不断的循环收发：
    // 接收缓冲区
    int nRecvBuf=32*1024;//设置为32K
    setsockopt(s,SOL_SOCKET,SO_RCVBUF,(const char*)&nRecvBuf,sizeof(int));
    //发送缓冲区
    int nSendBuf=32*1024;//设置为32K
    setsockopt(s,SOL_SOCKET,SO_SNDBUF,(const char*)&nSendBuf,sizeof(int));

    5. 如果在发送数据的时，希望不经历由系统缓冲区到socket缓冲区的拷贝而影响
    程序的性能：
    int nZero=0;
    setsockopt(socket，SOL_S0CKET,SO_SNDBUF，(char *)&nZero,sizeof(nZero));

    6.同上在recv()完成上述功能(默认情况是将socket缓冲区的内容拷贝到系统缓冲区)：
    int nZero=0;
    setsockopt(socket，SOL_S0CKET,SO_RCVBUF，(char *)&nZero,sizeof(int));

    7.一般在发送UDP数据报的时候，希望该socket发送的数据具有广播特性：
    BOOL bBroadcast=TRUE;
    setsockopt(s,SOL_SOCKET,SO_BROADCAST,(const char*)&bBroadcast,sizeof(BOOL));

    8.在client连接服务器过程中，如果处于非阻塞模式下的socket在connect()的过程中可
    以设置connect()延时,直到accpet()被呼叫(本函数设置只有在非阻塞的过程中有显著的
    作用，在阻塞的函数调用中作用不大)
    BOOL bConditionalAccept=TRUE;
    setsockopt(s,SOL_SOCKET,SO_CONDITIONAL_ACCEPT,(const char*)&bConditionalAccept,sizeof(BOOL));

    9.如果在发送数据的过程中(send()没有完成，还有数据没发送)而调用了closesocket(),以前我们
    一般采取的措施是"从容关闭"shutdown(s,SD_BOTH),但是数据是肯定丢失了，如何设置让程序满足具体
    应用的要求(即让没发完的数据发送出去后在关闭socket)？
    struct linger {
    u_short l_onoff;
    u_short l_linger;
    };
    linger m_sLinger;
    m_sLinger.l_onoff=1;//(在closesocket()调用,但是还有数据没发送完毕的时候容许逗留)
    // 如果m_sLinger.l_onoff=0;则功能和2.)作用相同;
    m_sLinger.l_linger=5;//(容许逗留的时间为5秒)
    setsockopt(s,SOL_SOCKET,SO_LINGER,(const char*)&m_sLinger,sizeof(linger));


**摘抄**

一:

假如端口被socket使用过, 并且利用socket.close()来关闭连接, 但此时端口还没有释放, 要经过一个TIME_WAIT的过程之后才能使用.
为了实现端口的马上复用, 可以选择setsockopt()函数来达到目的.

    import socket

    tcp1 = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp1.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp1.bind('1.1.1.1',12345)

此为tcp的例子, udp一样

写Socket程序的时候经常会遇到这个问题: 如果自己的程序不小心崩溃了, 重新启动程序的时候往往会在bind调用上失败, 错误原因为Address Already In Use, 往往要等待两分钟才能再次绑定.
但是在很多的程序（比如nginx）中好像并不存在这个问题, 就算被KILL了也能立刻重启.
这个区别还是比较头痛的.
其实我猜Unix Socket编程这样的书上有讨论过这方面的问题, 不过我竟然没有这方面的书籍（完全靠man看来也是行不通啊）.
    # TODO SIGTERM信号..干嘛用的.
我曾经天真的以为, 在收到SIGTERM这样的信号的时候把所有套接字全部关闭可以解决问题.
后来才发现无济于事. Google了这方面的文章才知道, 解决这个问题理论上有三种办法.

1.SOCK_REUSEADDR:曾经在研究内网穿透的时候跟这个东西打过交道. 如果一个监听的套接字被设置为允许地址复用, 那么套接字绑定到的地址不会被独占, 所以必然不会存在Address already in use的问题.
而且如果有两个套接字绑定到同一个端口, 也只允许一个套接字进行监听, 后一个套接字在调用listen的时候会报错. 因此这个方案显然是最佳方案. 对于完全不知道我在说什么的童鞋，你们只要这么做
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
/* What you need to do is add the following two line to your code */
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
/* Then do other things */listen(s, SOMAXCONN);/* ... */

2.不过据说这样的做法容易产生安全性问题, 某些操作系统（没有指明Linux或是BSD或是Windows）允许分别属于两个进程的两个套接字绑定到两个地址的同一个端口. 大多数程序把套接字绑定到0.0.0.0这个地址上进行监听（也即监听所有网络设备）, 这种情形下另外一个进程可以绑定到127.0.0.1或者是其他网络设备的IP地址的同一个端口, 并进行监听, 就可能把外部的连接给拦截下来（因为127.0.0.1这样的IP是属于特定设备的, 比0.0.0.0这虚拟设备更specific).
而解决这个安全性的问题的方法其实也不难（其实换个没问题的操作系统就可以了不是？）, 只要把套接字绑定绑定到具体的网络设备的IP地址（比如绑定到127.0.0.1，或者a.b.c.d)即可, 大不了为每个网络设备建一个套接字. 如果实施起来有难度, 只能考虑后面的两种方法.

3.让客户端先关闭连接. 如果所有的连接关闭事件都在客户端首先发生, 那么也不会存在这个问题. 不过这种做法可能需要修改协议, 而且貌似很容易恶意连接攻击. 修改系统TCP超时时间, 这种方法很不推荐.

二:

SO_KEEPALIVE

是什么, 怎么用, 为啥这么用.

socket心跳机制so_keepalive的三个参数详解

SO_KEEPALIVE 保持连接检测对方主机是否崩溃，避免（服务器）永远阻塞于TCP连接的输入。
设置该选项后，如果2小时内在此套接口的任一方向都没有数据交换，TCP就自动给对方 发一个保持存活探测分节(keepalive probe)。这是一个对方必须响应的TCP分节.它会导致以下三种情况：
1、对方接收一切正常：以期望的ACK响应，2小时后，TCP将发出另一个探测分节。
2、对方已崩溃且已重新启动：以RST响应。套接口的待处理错误被置为ECONNRESET，套接 口本身则被关闭。
3、对方无任何响应：源自berkeley的TCP发送另外8个探测分节，相隔75秒一个，试图得到一个响应。在发出第一个探测分节11分钟15秒后若仍无响应就放弃。套接口的待处理错误被置为ETIMEOUT，套接口本身则被关闭。如ICMP错误是“host unreachable(主机不可达)”，说明对方主机并没有崩溃，但是不可达，这种情况下待处理错误被置为 EHOSTUNREACH。

有关SO_KEEPALIVE的三个参数详细解释如下:
（16）tcp_keepalive_intvl，保活探测消息的发送频率。默认值为75s。
发送频率tcp_keepalive_intvl乘以发送次数tcp_keepalive_probes，就得到了从开始探测直到放弃探测确定连接断开的时间，大约为11min。
（17）tcp_keepalive_probes，TCP发送保活探测消息以确定连接是否已断开的次数。默认值为9（次）。
注意：只有设置了SO_KEEPALIVE套接口选项后才会发送保活探测消息。
（18）tcp_keepalive_time，在TCP保活打开的情况下，最后一次数据交换到TCP发送第一个保活探测消息的时间，即允许的持续空闲时间。默认值为7200s（2h）。

http://blog.csdn.net/ctthuangcheng/article/details/8596818
http://blog.csdn.net/ctthuangcheng/article/details/9569265
http://blog.csdn.net/ctthuangcheng/article/details/9450087

三:

    # TODO 这个应该是服务器端设置的吧?, 非阻塞..
    socket.setblocking(0)


----

    # -*- coding: utf-8 -*-

    import socket

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(('localhost', 6379))

    # set j ts
    commands = ['*3\r\n$3\r\nset\r\n$1\r\nj\r\n$2\r\nts\r\n']
    for command in commands:
        sock.sendall(command)

    sock.close()

    # ----- ----- ----- ----- -----


    import socket
    import sys


    class BaseError(Exception):
        pass


    class Timeout(BaseError):
        pass


    class ConnectionError(BaseError):
        pass


    def connect(host, port):
        """
        创建并返回一个socket connect
        """
        socket_timeout = 5

        err = None
        for res in socket.getaddrinfo(host, port, socket.AF_UNSPEC, socket.SOCK_STREAM):
            family, socktype, proto, canonname, socket_address = res
            _sock = None
            try:
                _sock = socket.socket(family, socktype, proto)

                _sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)

                _sock.settimeout(socket_timeout)

                _sock.connect(socket_address)

                _sock.settimeout(socket_timeout)

                return _sock

            except socket.error as _:
                err = _
                if _sock is not None:
                    _sock.close()

        if err is not None:
            raise err

        raise socket.error("socket.getaddrinfo returned an empty list")


    def disconnect(_sock):
        """
        关闭socket connect
        """
        if _sock is not None:
            _sock.close()
            _sock = None
        if _sock is None:
            return
        try:
            _sock.shutdown(socket.SHUT_RDWR)
            _sock.close()
        except socket.error:
            pass

    sock = connect("localhost", 6379)

    commands = ['*3\r\n$3\r\nset\r\n$1\r\nj\r\n$2\r\nts\r\n']

    try:
        for command in commands:
            sock.sendall(command)
    except socket.timeout:
        disconnect(sock)
        raise Timeout("writing to socket")
    except socket.error:
        e = sys.exc_info()[1]
        disconnect(sock)

        if len(e.args) == 1:
            _errno, errmsg = 'UNKNOWN', e.args[0]
        else:
            _errno, errmsg = e.args
        raise ConnectionError("Error %s while writing to socket. %s." % (_errno, errmsg))
    except:
        disconnect(sock)
        raise
    finally:
        disconnect(sock)
        sock = None

    # ----- ----- ----- ----- -----

    协议

    sample server 接收以上socket发送数据, 并原本打印
    import socket
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(('localhost', 50007))
    s.listen(1)
    while 1:
        conn, addr = s.accept()
        print'Connected by', addr
        while 1:
            data = conn.recv(1024)
            conn.sendall('Done.')
            print "-%s-" % repr(data)
        conn.close()

    sample http server 使用浏览器查看

    import socket

    EOL1 = b'\n\n'
    EOL2 = b'\n\r\n'

    response = b'HTTP/1.0 200 OK\r\nDate: Mon, 1 Jan 1996 01:01:01 GMT\r\n'
    response += b'Content-Type: text/plain\r\nContent-Length: 13000\r\n\r\n'
    response += b'Hello, world!' * 1000

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind(('127.0.0.1', 9996))
    sock.listen(1)
    try:
        while True:
            conn, addr = sock.accept()
            request = b''
            while EOL1 not in request and EOL2 not in request:
                # TODO recv(1024) 边界不止读取\n\r\n会怎么样?
                request += conn.recv(1024)
            print repr(request.decode())
            conn.send(response)
            conn.close()
    finally:
        sock.close()

    import socket
    import select

    EOL1 = b'\n\n'
    EOL2 = b'\n\r\n'

    response = b'HTTP/1.0 200 OK\r\nDate: Mon, 1 Jan 1996 01:01:01 GMT\r\n'
    response += b'Content-Type: text/plain\r\nContent-Length: 13000\r\n\r\n'
    response += b'Hello, world!' * 1000

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind(('127.0.0.1', 9996))
    sock.listen(1)
    sock.setblocking(0)

    epoll = select.epoll()
    epoll.register(sock.fileno(), select.EPOLLIN)

    try:
        conns = {}
        requests = {}
        responses = {}
        while True:
            events = epoll.poll(1)
            for fileno, event in events:
                if fileno == sock.fileno():
                    conn, addr = sock.accept()
                    conn.setblocking(0)
                    epoll.register(conn.fileno(), select.EPOLLIN)
                    conns[conn.fileno()] = conn
                    requests[conn.fileno()] = b''
                    responses[conn.fileno()] = response
                elif event & select.EPOLLIN:
                    requests[fileno] += conns[fileno].recv(1024)
                    if EOL1 in requests[fileno] or EOL2 in requests[fileno]:
                        epoll.modify(fileno, select.EPOLLOUT)
                        print repr(requests[fileno].decode())
                elif event & select.EPOLLOUT:
                    byteswritten = conns[fileno].send(responses[fileno])
                    responses[fileno] = responses[fileno][byteswritten:]
                    if len(responses[fileno]) == 0:
                        epoll.modify(fileno, 0)
                        conns[fileno].shutdown(socket.SHUT_RDWR)
                elif event & select.EPOLLHUP:
                    epoll.unregister(fileno)
                    conns[fileno].close()
                    del conns[fileno]
    finally:
        epoll.unregister(sock.fileno())
        epoll.close()
        sock.close()

    import socket
    import select

    EOL1 = b'\n\n'
    EOL2 = b'\n\r\n'

    response = b'HTTP/1.0 200 OK\r\nDate: Mon, 1 Jan 1996 01:01:01 GMT\r\n'
    response += b'Content-Type: text/plain\r\nContent-Length: 13000\r\n\r\n'
    response += b'Hello, world!' * 1000

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind('127.0.0.1', 9996)
    sock.listen(1)
    sock.setblocking(0)

    epoll = select.epoll()
    epoll.register(sock.fileno(), select.EPOLLIN | select.EPOLLET)

    try:
        conns = {}
        requests = {}
        responses = {}
        while True:
            events = epoll.poll(1)
            for fileno, event in events:
                if fileno == sock.fileno():
                    try:
                        while True:
                            conn, addr = sock.accept()
                            conn.setblocking(0)
                            epoll.register(conn.fileno(), select.EPOLLIN | select.EPOLLET)
                            conns[conn.fileno()] = conn
                            requests[conn.fileno()] = b''
                            responses[conn.fileno()] = response
                    except socket.error:
                        pass
                elif event & select.EPOLLIN:
                    try:
                        while True:
                            requests[fileno] += conns[fileno].recv(1024)
                    except socket.error:
                        pass
                    if EOL1 in requests[fileno] or EOL2 in requests[fileno]:
                        epoll.modify(fileno, select.EPOLLOUT | select.EPOLLET)
                        print repr(requests[fileno].decode())
                elif event & select.EPOLLOUT:
                    try:
                        while len(responses[fileno]) > 0:
                            byteswritten = conns[fileno].send(responses[fileno])
                            responses[fileno] = responses[fileno][byteswritten:]
                    except socket.error:
                        pass
                    if len(responses[fileno]) == 0:
                        epoll.modify(fileno, select.EPOLLET)
                        conns[fileno].shutdown(socket.SHUT_RDWR)
                elif event & select.EPOLLHUP:
                    epoll.unregister(fileno)
                    conns[fileno].close()
                    del conns[fileno]
    finally:
        epoll.unregister(sock.fileno())
        epoll.close()
        sock.close()
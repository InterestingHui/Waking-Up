### TCP/IP协议族
<details><summary>TCP/IP协议族的体系结构自底向上有几层？</summary>
  
- 数据链路层、网络层、传输层、应用层
</details>

<details><summary>每层做了什么（略）？</summary>
  
- 数据链路层：实现了网卡接口的网络驱动程序在物理媒介上的传输，也就是封装了物理网络的电气细节。
- 网络层：网络层实现数据包的选路和转发，也就是封装了网络连接的细节。
- 传输层：传输层为两台主机上的应用程序提供端到端的通信。
- 应用层：为用户提供应用程序的相关服务。
- 各层之间的联系：上层协议使用下层协议提供的服务，通常是使用挨着的下一层，但也可以跨层应用层可以直接使用网络层的服务，比如ping程序、OSPF协议。
</details>

<details><summary>每层有哪些协议，有什么作用？</summary>
  
- 数据链路层：
    - ARP协议——地址解析协议：完成IP地址到物理地址，也就是MAC地址的映射
    - RARP协议——逆地址解析协议：完成物理地址到IP地址的映射
- 网络层：
    - IP协议——因特网协议：根据数据包的IP地址来决定如何投递数据包，因为网络采用逐跳通信的方式，所以需要IP协议来不断地选择合适的路由器也就是中间节点来决定数据包的交付、转发。
    - ICMP协议——因特网控制报文协议：主要用于检测网络连接和判断重定向类型。这个协议的报文是32位，前8位区分网络连接的类型，是目标可送达还是不可送达以及重定向，9-16位是进一步区分重定向的类型。剩下16位是报文校验和。
- 传输层：
    - TCP协议——传输控制协议
        - TCP为应用层提供可靠的、面向连接的和基于流的服务。
        - （TCP为什么是可靠的？）TCP使用超时重传、数据确认等方式来确保数据包被正确地发送至目的端，实现可靠性。
        - TCP的服务是基于流的，基于流的数据没有边界长度的限制，它源源不断地从通信的一端流到另一端，发送端逐字节地向数据流写入数据，接收端也逐字节地读数据。
    - UDP协议——用户数据报协议
        - 和TCP相反，UDP提供不可靠的、无连接的、和基于数据报的服务。
        - 不可靠指UDP无法保证数据报从通信的一端传到另一端
            - 如果数据丢失，UDP只是简单地通知应用程序发送失败，所以需要应用程序自己实现数据确认、超时重传等逻辑
        - 基于数据报的服务是指，每个UDP数据报都有一个长度，接收端要以这个长度为最小单位来一次性读取数据，否则数据将会被截断
    - SCTP协议——流控制传输协议
        - 为了在因特网上传输电话信号而设计的
- 应用层：
    - telnet：远程登录协议
    - OSPF：开放最短路径优先协议——是一种动态路由更新协议，用于路由器之间的通信
    - DNS：域名服务协议——提供机器域名到IP地址的转换
</details>

<details><summary>什么是路由器？</summary>
  
- 路由器是指通信过程中的中间节点。通信的两台主机不是直接相连的，而是通过各个中间节点连接，中间节点就是路由器。
</details>

<details><summary>上层是如何使用下层的协议提供的服务的？</summary>

- 通过“封装”实现：
    - 封装是指，应用程序数据在发送到物理网络之前，需要沿着协议栈从上往下依次传递
    - 在传递的过程中，每层协议都将在上层协议的基础上加上自己的头部信息，有时还包含尾部信息，用来实现该层的功能。
    - 那么这个传递的过程就是封装
- 各个协议在封装的过程中完成自己的封装并得到相应封装后的数据
    - TCP封装后的数据叫做TCP报文段，简称TCP段。
        - TCP段分为头部信息和内核缓冲区数据，内核缓冲区包括接受缓冲区和发送缓冲区
        - TCP段在封装的过程中会保存数据副本，在应用层应用程序使用send或者write函数向TCP连接写入数据时，内核中的TCP模块会复制这些数据到TCP内核缓冲区中，生成副本
    - UDP封装后的数据叫做UDP数据报
        - UDP数据报和TCP段的区别在于UDP不会保存数据副本，如果应用程序检测到该数据报未能正确接收，应用程序需要重新从用户空间将数据拷贝到UDP内核发送缓冲区中进行重发。
    - IP封装后的数据叫做IP数据报
        - IP数据报包括头部信息和数据部分，其中数据部分就是一个TCP段、UDP数据报或者是ICMP报文
    - 经过数据链路层封装的数据叫做帧。
        - 传输的媒介不同，帧的类型也不同：如果是以太网上传输的就是以太帧，是令牌环网络传输的就是令牌环帧
        - 帧就是最终在物理网络上传送的字节序列，至此，封装完成
</details>

<details><summary>应用程序是如何获取另一端发送的数据的？</summary>
  
- 通过“分用”实现；
    - 分用是指，目的主机在获取到
</details>

<details><summary></summary>
  
</details>

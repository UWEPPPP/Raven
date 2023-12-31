### 不同设备上的进程需要通信,所以需要一套通用的网络协议 然后协议是分层的

### 应用层

 专注于提供应用功能 如 HTTP FTP Telnet DNS SMTP等

### 传输层

![bagu/image/应用层/web](../image/%E5%BA%94%E7%94%A8%E5%B1%82.webp)

应用层会把数据包传给传输层 , 即 传输层是为应用层提供网络支持的。

####  TCP And UDP

  大部分应用使用的是tcp ，比如http应用层协议 。TCP相比UDP多了很多特性，比如流量控制，超时重传，拥塞控制， 旨在能够保证数据包坑可靠的传输。

  UDP相对来说就很简朴，只负责发送数据包，不保证成功，但是它的实时性更好，传输效率也更高。如果要UDP也能实现可靠传输，把TCP的特性在应用层上实现即可，单不容易。

##### MSS（TCP最大报文段长度）

  当应用传输的数据包大小超过MSS时，则需要将数据包分块，这样的话若中途有一个分块异常，则只需要重新发送一个分开，而不是整个数据包。 在tcp协议中，分块被成为TCP段

![./image/TCP段.webp](../image/TCP%E6%AE%B5.webp)

##### 端口

设备作为暑假接收方时，传输层则负责把数据包传给应用，因为很多应用在接收或传输数据，所以需要一个编号来将应用区分开，这个编号就是端口。

### 网络层

传输层只需要实现应用到应用的通信，真正的传输功能交给网络层。

![](../image/%E7%BD%91%E7%BB%9C%E5%B1%82.webp)

##### IP协议 Internet Protocol

网络层最常使用的是ip协议,ip协议将传输层的报文作为数据部分,再加上ip包头组装成ip报文，如果报文大小超过MTU 就会再次进行分片，得到一个即将发送到网络的IP报文。

![](../image/12.webp)

##### IP地址

设备通过 ip地址作为编号 来 找到要传输的另一个设备 

对于IPv4协议 IP地址共32位 ，分成了四段 比如（192.168.100.1），每段是8位(二进制)，但只有一个ip地址不方便寻址，

所以ip地址有两种意义

1. **网络号** 负责标识 ip的地址是属于哪个 **子网**

2. **主机号** 负责标识同一子网下的不同主机

   ##### 子网掩码将用于配合来算出ip地址的网络号和主机号

   ```solidity
   举个例子，比如 10.100.122.0/24，后面的/24表示就是 255.255.255.0 子网掩码，255.255.255.0 二进制是「11111111-11111111-11111111-00000000」，大家数数一共多少个1？不用数了，是 24 个1，为了简化子网掩码的表示，用/24代替255.255.255.0。
   
   知道了子网掩码，该怎么计算出网络地址和主机地址呢？
   
   将 10.100.122.2 和 255.255.255.0 进行按位与运算，就可以得到网络号，
   ```

IP协议的另一个重要能力就是路由，在数据包到达一个网络节点后，通过路由算法决定下一步走哪条路径。

![](../image/17.webp)

所以，**IP 协议的寻址作用是告诉我们去往下一个目的地该朝哪个方向走，路由则是根据「下一个目的地」选择路径。寻址更像在导航，路由更像在操作方向盘**。

### 网络接口层

在生成了IP头部后，接下来要交给网络接口层 在IP头部的签名加上MAC头部，然后封装成数据帧 (Data Frame)发送到网络上

![](../image/%E7%BD%91%E7%BB%9C%E6%8E%A5%E5%8F%A3%E5%B1%82.webp)

##### 以太网  

以太网就是一种在 局域网 内，把附近的设备连接起来，使它们之间可以进行通讯的技术。

以太网在判断网络包目的地时和 IP 的方式不同，因此必须采用相匹配的方式才能在以太网中将包发往目的地，而 MAC 头部就是干这个用的，所以，在以太网进行通讯要用到 MAC 地址。

MAC 头部是以太网使用的头部，它包含了接收方和发送方的 MAC 地址等信息，我们可以通过 ARP 协议获取对方的 MAC 地址。

所以说，网络接口层主要为网络层提供「链路级别」传输的服务，负责在以太网、WiFi 这样的底层网络上发送原始数据包，工作在网卡这个层次，使用 MAC 地址来标识网络上的设备。

# 总结

![](../image/tcpip%E5%8F%82%E8%80%83%E6%A8%A1%E5%9E%8B.drawio.webp)

每层数据的封装格式 

![](../image/%E5%B0%81%E8%A3%85.webp)

网络接口层的传输单位是帧（frame），IP 层的传输单位是包（packet），TCP 层的传输单位是段（segment），HTTP 的传输单位则是消息或报文（message）。但这些名词并没有什么本质的区分，可以统称为数据包。
# ARP：地址解析协议
> IP地址到对应的硬件地址之间提供动态映射
> 对I P地址数据链路如以太网或令牌环网都有自己的寻址机制（48bit地址）使用数据链路的任何网络层从一个网络如以太网可以同时被不同的网络层使用
> 一台主机把以太网数据帧发送到位于同一局域网上的另一台主机时根据 48 bit的以太网地址来确定目接口


*  ftp bsdi
  1. 应用程序FTP客户端调用函数gethostbyname把主机名（bsdi）转换成32 bit的IP地址。函数在DNS（域名系统）称作解析器使用DNS或者在较小网络中使用一个静态的主机文件（/e t c / h o s t s）。
  2. FTP客户端请求TCP用得到的iP地址建立连接
  3. TCP发送一个连接请求分段到远端的主机，即用上述IP地址发送一份 IP数据报
  4. 目的主机在本地网络上（如以太网、令牌环网或点对点链接的另一端）I P数据报可以直接送到目的主机上。
    * 如远程网络 就通过IP选路函数来确定位于本地网络上的下一站路由器地址，并转发 I P数据报将被送到位于本地网络上的一台主机或路由器
  5. 如果是以太网发送端主机必须把 32 bit的I P地址变换成48bit的地址



##  ARP：地址解析协议使用
> 从逻辑Internet地址到对应的物理硬件地址需要进行翻译 A R P本来是用于广播网络的 多主机或路由器连在同一个网络上

6. ARP发送一份称作A R P请求的以太网数据帧给以太网上的每个主机
7. 目的主机的A R P层收到这份广播报文后，识别出这是发送端在寻问它的 I P地址，于是发送一个A R P应答 应答包含I P地址及对应的硬件地址
8. 收到A R P应答后，使A R P进行请求—应答交换的I P数据报现在就可以传送了
9. 发送I P数据报到目的主机
  * A R P背后有一个基本概念，那就是网络接口有一个硬件地址（一个 48 bit的值，标识不同的以太网或令牌环网络接口）硬件层次上进行的数据帧交换必须有正确的接口地址 
  * TCP/IP有自己的地址：32 bit的I P地址内核（如以太网驱动程序）知道目的端的硬件地址才能发送数据 ARP的功能是在32 bit的IP地址和采用不同网络技术的硬件地址之间提供动态映射。


## ARP高速缓存
* 每个主机上都有一个 A R P高速缓存存放了最近Internet地址到硬件地址之间的映射记录每一项的生存时间一般为 20分钟起创建开始算起
* 48bit的以太网地址用6个十六进制的数来表示，中间以冒号隔开

## ARP的分组格式

* 以太网上解析I P地址A R P请求和应答分组的格式（A R P可以用于其他类型的网络解析IP地址以外的地址帧类型字段的前四个字段指定了最后四个字段的类型和长度）以太网报头中的前两个字段是以太网的源地址和目的地址目的地址为全 1的特殊地址是广播地址
  * 电缆上的所有以太网接口都要接收广播的数据帧 两个字节长的以太网帧类型表示后面数据的类型 
    * 例如，一个 A R P请求分组询问协议地址（这里是I P地址）对应的硬件地址（这里是以太网地址）。硬件类型字段表示硬件地址的类型。它的值为 1即表示以太网地址。协议类型字段表示要映射的协议地址类型。它的值为 0 x 0 8 0 0即表示I P地址。它的值与包含 I P数据报的以太网数据
  * 接下来的四个字段是发送端的硬件地址（在本例中是以太网地址）、发送端的协议地址（I P地址）、目的端的硬件地址和目的端的协议地址。注意，这里有一些重复信息：在以太网


## 不存在主机的ARP请求
> 查询的主机已关机或不存在会发生什么情况呢？为此我们指定一个并不存在的Internet地址 — 根据网络号和子网号所对应的网络确实存在多次进行 A R P请求

* 第1次请求发生后5 . 5秒进行第2次请求，在2 4秒之后又进行第3次请求t c p d u m p命令输出的超时限制为29.5秒。telnet命令使用前后分别用date命令检查时间 Telnet客户端的连接请求在大约 75秒后才放弃多数的B S D实现把完成T C P连接请求的时间限制设置为75秒

* 注意: 在线路上始终看不到TCP的报文段ARP请求直到ARP回答返回TCP报文段才可以被发送，因为硬件地址到这时才可能知道。如果我们用过滤模式运行t c p d u m p命令 只查看T C P数据，那么将没有任何输出。


## ARP代理
> A R P请求是从一个网络的主机发往另一个网络上的主机，连接这两个网络的路由器就可以回答该请求该过程称作委托 A R P或A R P代理(Proxy ARP)
以欺骗发起A R P请求的发送端使它误以为路由器就是目的主机，而事实上目的主机是在路由器的另一边路由器的功能相当于目的主机的代理，把分组从其他主机转发给它

* 系统s u n与两个以太网相连。但是，我们也指出过，事实上并不是这样，请把它与封内图 1进行比较。在s u n和子网1 4 0 . 2 5 2 . 1之间实
际存在一个路由器，就是这个具有A R P代理功能的路由器使得s u n就好像在子网1 4 0 . 2 5 2 . 1上一样
* 子网1 4 0 . 2 5 2 . 1（称作g e m i n i）上的其他主机有一份I P数据报要传给地址为1 4 0 . 2 5 2 . 1 . 2 9的s u n时，g e m i n i比较网络号（1 4 0 . 2 5 2）和子网号

* A R P代理可以把数据报传送到路由器 s u n上，但是子网140.252.13上的其他主机是如何处理的？
  * 选路必须使数据报能到达其他主机。这里需要特殊处理，选路表中的表项必须在网络140.252的某个地方制定，使所有数据报的目的端要么是子网 1 4 0 . 2 5 2 . 1 3，要么是子网上的
  * 某个主机，这样都指向路由器netb而路由器 netb知道如何把数据报传到最终的目的端，
即通过路由器sun
 * ARP代理也称作混合ARP(promiscuousARP）或ARP 出租(ARP hack)。这些名字来自于A R P代理的其他用途：通过两个物理网络之间的路由器可以互相隐藏物理网络
 * 两个物理网络可以使用相同的网络号，只要把中间的路由器设置成一个 A R P代理，以响应一个网络到另一个网络主机的 A R P请求。这种技术在过去用来隐藏一组在不同物理电缆上运行旧版T C P / I P的主机。分开这些旧主机有两个共同的理由，其一是它们不能处理子网划分，其二是它们使用旧的广播地址（所有比特值为 0的主机号，而不是目前使用的所有比特值为 1


* arp命令
  * 命令及参数- a来显示A R P高速缓存中的所有内容。这里介绍其他参数
的功能。
  * 超级用户可以用选项- d来删除A R P高速缓存中的某一项内容（这个命令格式可以在运行可以通过选项- s来增加高速缓存中的内容。这个参数需要主机名和以太网地址：
  * 对应于主机名的I P地址和以太网地址被增加到高速缓存中。新增加的内容是永久性的（比如，它没有超时值 除非在命令行的末尾附上关键字 t e m p
位于命令行末尾的关键字 p u b和- s选项一起，可以使系统起着主机 A R P代理的作用
  * 系统将回答与主机名对应的IP地址的ARP请求，并以指定的以太网地址作为应答 如果广播的地址是系统本身系统就为指定的主机名起着委托ARP代理的作用

### 小结
* 多数的T C P / I P实现中，A R P是一个基础协议，但是它的运行对于应用程序或系统管理员来说一般是透明的。 A R P高速缓存在它的运行过程中非常关键，可以用 a r p命令对高速缓存进行检查和操作。
* 高速缓存中的每一项内容都有一个定时器，根据它来删除不完整和完整的表项。a r p命令可以显示和修改A R P高速缓存中的内容。A R P的一般操作，同时也介绍了一些特殊的功能：委托 A R P（当路由器对来自于另一个路由器接口的 A R P请求进行应答时）和免费 A R P（发送自己 I P地址的A R P请求，一般发生在引导过程中）。



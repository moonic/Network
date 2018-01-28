# DHCP（Dynamic Host Configuration Protocol，动态主机配置协议）
> 使用UDP协议工作， 主要有两个用途：给内部网络或网络服务供应商自动分配IP地址，给用户或者内部网络管理员作为对所有计算机作中央管理的手段，在RFC 2131中有详细的描述。DHCP有3个端口，其中UDP67和UDP68为正常的DHCP服务端口，分别作为DHCP Server和DHCP Client的服务端口；546号端口用于DHCPv6 Client，而不用于DHCPv4，是为DHCP failover服务，这是需要特别开启的服务，DHCP failover是用来做“双机热备”的。
集中的管理、分配IP地址，使网络环境中的主机动态的获得IP地址、Gateway地址、DNS服务器地址等信息，并能够提升地址的使用率。

* DHCP协议采用客户端/服务器模型，主机地址的动态分配任务由网络主机驱动。当DHCP服务器接收到来自网络主机申请地址的信息时，才会向网络主机发送相关的地址配置等信息，以实现网络主机地址信息的动态配置。DHCP具有以下功能：
1. 保证任何IP地址在同一时刻只能由一台DHCP客户机所使用。
2. DHCP应当可以给用户分配永久固定的IP地址。
3. DHCP应当可以同用其他方法获得IP地址的主机共存（如手工配置IP地址的主机）。
4. DHCP服务器应当向现有的BOOTP客户端提供服务。

* DHCP有三种机制分配IP地址：
  1. 自动分配方式（Automatic Allocation），DHCP服务器为主机指定一个永久性的IP地址，一旦DHCP客户端第一次成功从DHCP服务器端租用到IP地址后，就可以永久性的使用该地址。
  2. 动态分配方式（Dynamic Allocation），DHCP服务器给主机指定一个具有时间限制的IP地址，时间到期或主机明确表示放弃该地址时，该地址可以被其他主机使用。
  3. 手工分配方式（Manual Allocation），客户端的IP地址是由网络管理员指定的，DHCP服务器只是将指定的IP地址告诉客户端主机。
只有动态分配可以重复使用客户端不再需要的地址。
DHCP消息的格式是基于BOOTP（Bootstrap Protocol）消息格式的，这就要求设备具有BOOTP中继代理的功能，并能够与BOOTP客户端和DHCP服务器实现交互。BOOTP中继代理的功能，使得没有必要在每个物理网络都部署一个DHCP服务器。

*  DHCP协议
  * DHCP协议采用UDP作为传输协议，主机发送请求消息到DHCP服务器的67号端口，DHCP服务器回应应答消息给主机的68号端口。
  * DHCP Client以广播的方式发出DHCP Discover报文。所有的DHCP Server都能够接收到DHCP Client发送的DHCP Discover报文，所有的DHCP Server都会给出响应，向DHCP Client发送一个DHCP Offer报文。
  * DHCP Offer报文中“Your(Client) IP Address”字段就是DHCP Server能够提供给DHCP Client使用的IP地址，且DHCP Server会将自己的IP地址放在“option”字段中以便DHCP Client区分不同的DHCP Server。DHCP Server在发出此报文后会存在一个已分配IP地址的纪录。
  * DHCP Client只能处理其中的一个DHCP Offer报文，一般的原则是DHCP Client处理最先收到的DHCP Offer报文。
  * DHCP Client会发出一个广播的DHCP Request报文，在选项字段中会加入选中的DHCP Server的IP地址和需要的IP地址。
  * DHCP Server收到DHCP Request报文后，判断选项字段中的IP地址是否与自己的地址相同。如果不相同，DHCP Server不做任何处理只清除相应IP地址分配记录；如果相同，DHCP Server就会向DHCP Client响应一个DHCP ACK报文，并在选项字段中增加IP地址的使用租期信息。
  * DHCP Client接收到DHCP ACK报文后，检查DHCP Server分配的IP地址是否能够使用。如果可以使用，则DHCP Client成功获得IP地址并根据IP地址使用租期自动启动续延过程；如果DHCP Client发现分配的IP地址已经被使用，则DHCP Client向DHCPServer发出DHCP Decline报文，通知DHCP Server禁用这个IP地址，然后DHCP Client开始新的地址申请过程。DHCP Client在成功获取IP地址后，随时可以通过发送DHCP Release报文释放自己的IP地址，DHCP Server收到DHCP Release报文后，会回收相应的IP地址并重新分配。
  * 在使用租期超过50%时刻处，DHCP Client会以单播形式向DHCP Server发送DHCPRequest报文来续租IP地址。如果DHCP Client成功收到DHCP Server发送的DHCP ACK报文，则按相应时间延长IP地址租期；如果没有收到DHCP Server发送的DHCP ACK报文，则DHCP Client继续使用这个IP地址。
在使用租期超过87.5%时刻处，DHCP Client会以广播形式向DHCP Server发送DHCPRequest报文来续租IP地址。如果DHCP Client成功收到DHCP Server发送的DHCP ACK报文，则按相应时间延长IP地址租期；如果没有收到DHCP Server发送的DHCP ACK报文，则DHCP Client继续使用这个IP地址，直到IP地址使用租期到期时，DHCP Client才会向DHCP Server发送DHCP Release报文来释放这个IP地址，并开始新的IP地址申请过程。
> DHCP客户端可以接收到多个DHCP服务器的DHCPOFFER数据包，然后可能接受任何一个DHCPOFFER数据包，但客户端通常只接受收到的第一个DHCPOFFER数据包。另外，DHCP服务器DHCPOFFER中指定[1]  的地址不一定为最终分配的地址，通常情况下，DHCP服务器会保留该地址直到客户端发出正式请求。
正式请求DHCP服务器分配地址DHCPREQUEST采用广播包，是为了让其它所有发送DHCPOFFER数据包的DHCP服务器也能够接收到该数据包，然后释放已经OFFER（预分配）给客户端的IP地址。

* 如果发送给DHCP客户端的地址已经被其他DHCP客户端使用，客户端会向服务器发送DHCPDECLINE信息包拒绝接受已经分配的地址信息。
  * 在协商过程中，如果DHCP客户端发送的REQUEST消息中的地址信息不正确，如客户端已经迁移到新的子网或者租约已经过期，DHCP服务器会发送DHCPNAK消息给DHCP客户 端，让客户端重新发起地址请求过程。




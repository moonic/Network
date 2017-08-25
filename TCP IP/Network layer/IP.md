# IP选路
选路是IP最重要的功能之一。图 9 - 1是I P层处理过程的简单流程。需要进行选路的数据报可以由本地主机产生，也可以由其他主机产生。在后一种情况下，主机必须配置成一个路由器，否则通过网络接口接收到的数据报，如果目的地址不是本机就要被丢弃（例如，悄无声息地被丢弃）。

在图9 - 1中，我们还描述了一个路由守护程序（ d a e m o n），通常这是一个用户进程。在
U n i x系统中，大多数普通的守护程序都是路由程序和网关程序（术语 d a e m o n指的是运行在后台的进程，它代表整个系统执行某些操作。 d a e m o n一般在系统引导时启动，在系统运行期间一直存在）。在某个给定主机上运行何种路由协议，如何在相邻路由器上交换选路信息，以及选路协议是如何工作的，所有这些问题都是非常复杂的,了解单个I P层如何作出路由决策。

图9 - 1所示的路由表经常被IP访问（繁忙的主机上一秒钟内可能要访问几百次）它被路由守护程序更新的频度却要低得多（可能大约 3 0秒种一次）。当接收到I C M P重定向，报文时，路由表也要被更新，这一点我们将在 9 . 5节讨论r o u t e命令时加以介绍。还将用n e t s t a t命令来显示路由表。


## 选路的原理
> 了解内核是如何维护路由表的。路由表中包含的信息决定了I P层所做的所有决策

* IP搜索路由表的几个步骤：
  1. 搜索匹配的主机地址；
  2. 搜索匹配的网络地址；
  3. 搜索默认表项（默认表项一般在路由表中被指定为一个网络表项，其网络号为 0）匹配主机地址步骤始终发生在匹配网络地址步骤之前。

* I P层进行的选路实际上是一种选路机制，它搜索路由表并决定向哪个网络接口发送分组。这区别于选路策略，它只是一组决定把哪些路由放入路由表的规则。 I P执行选路机制，而路由守护程序则一般提供选路策略。

* 简单路由表
在主机s v r 4上，我们先执行带-r选项的n e t s t a t命令
列出路由表，然后以-n选项再次执行该命令，以数字格式打印出I P地址（我们这样做是因为路由
表中的一些表项是网络地址，而不是主机地址。如果没有- n选项，n e t s t a t命令将搜索文件
/ e t c / n e t w o r k s并列出其中的网络名。这样会与另一种形式的名字 — 网络名加主机名相混淆）。

主机路由表的复杂性取决于主机所在网络的拓扑结构。
1) 最简单的（也是最不令人感兴趣的）情况是主机根本没有与任何网络相连。 T C P / I P协
议仍然能用于这样的主机，但是只能与自己本身通信！这种情况下的路由表只包含环回接口
一项。
2) 接下来的情况是主机连在一个局域网上，只能访问局域网上的主机。这时路由表包含
两项：一项是环回接口，另一项是局域网（如以太网）。
3) 如果主机能够通过单个路由器访问其他网络（如 I n t e r n e t）时，那么就要进行下一步。一般情况下增加一个默认表项指向该路由器。
4) 如果要新增其他的特定主机或网络路由，那么就要进行最后一步。在我们的例子中，
到主机s l i p的路由要通过路由器b s d i就是这样的例子。


我们根据上述I P操作的步骤使用这个路由表为主机 s v r 4上的一些分组例子选择路由。
1) 假定目的地址是主机 s u n，1 4 0 . 2 5 2 . 1 3 . 3 3。首先进行主机地址的匹配。路由表中的两
个主机地址表项（ s l i p和l o c a l h o s t）均不匹配，接着进行网络地址匹配。这一次匹配成
功，找到表项1 4 0 . 2 5 2 . 1 3 . 3 2（网络号和子网号都相同），因此使用e m d 0接口。这是一个直接
路由，因此链路层地址将是目的端的地址。
2) 假定目的地址是主机s l i p，1 4 0 . 2 5 2 . 1 3 . 6 5。首先在路由表搜索主机地址，并找到一个
匹配地址。这是一个间接路由，因此目的端的 I P地址仍然是1 4 0 . 2 5 2 . 1 3 . 6 5，但是链路层地址
必须是网关1 4 0 . 2 5 2 . 1 3 . 6 5的链路层地址，其接口名为e m d 0。
3) 这一次我们通过I n t e r n e t给主机a w . c o m（1 9 2 . 2 0 7 . 11 7 . 2）发送一份数据报。首先在路
由表中搜索主机地址，失败后进行网络地址匹配。最后成功地找到默认表项。该路由是一个
间接路由，通过网关1 4 0 . 2 5 2 . 1 3 . 3 3，并使用接口名为e m d 0。
4) 在我们最后一个例子中，我们给本机发送一份数据报。有四种方法可以完成这件事，
如用主机名、主机I P地址、环回名或者环回I P地址：


* 初始化路由表
每当初始化一个接口时（通常是用
i f c o n f i g命令设置接口地址），就为接口自动创建一个直接路由。对于点对点链路和环回接口
来说，路由是到达主机（例如，设置 H标志）。对于广播接口来说，如以太网，路由是到达网
络。
到达主机或网络的路由如果不是直接相连的，那么就必须加入路由表。一个常用的方法
是在系统引导时显式地在初始化文件中运行 r o u t e命令。在主机s v r 4上，我们运行下面两个命
令来添加路由表中的表项：
route add default sun 1
route add slip bsdi 1
第3个参数（d e f a u l t和s l i p）代表目的端，第4个参数代表网关（路由器），最后一个
参数代表路由的度量( m e t r i c )。r o u t e命令在度量值大于0时要为该路由设置G标志，否则，当
耗费值为0时就不设置G标志。


* 复杂的路由表
路由表中的目的地址就是点对点链路的另一端 (即路由器n e t b), 网关地址为外出
接口的本地I P地址( 1 4 0 . 2 5 2 . 1 . 2 9 ) (前面已经说过, n e t s t a t为直接路由打印出来的网关地址就
是本地接口所用的I P地址)。
默认的路由表项是一个到达网络的间接路由 (设置了G标志，但没有设置 H标志)，这正是
我们所希望的。网关地址是路由器的地址 ( 1 4 0 . 2 5 2 . 1 . 1 8 3，S L I P链路的另一端), 而不是S L I P链
路的本地I P地址( 1 4 0 . 2 5 2 . 1 . 2 9 )。其原因还是因为是间接路由，不是直接路由。
还应该指出的是，n e t s t a t输出的第3和第4行(接口名为s l 0)由S L I P软件在启动时创建，
并在关闭时删除.

* 没有到达目的地的路由
假定对路由表的搜索能找到匹配的表项，即使匹配的是默认项。如果
路由表中没有默认项，而又没有找到匹配项，这时会发生什么情况呢？
结果取决于该I P数据报是由主机产生的还是被转发的（例如，我们就充当一个路由器）。
如果数据报是由本地主机产生的，那么就给发送该数据报的应用程序返回一个差错，或者是
“主机不可达差错”或者是“网络不可达差错”。如果是被转发的数据报
那么就给原始发送端发送一份I C M P主机不可达的差错报文。下一节将讨论这种差错。

## ICMP重定向差错
当I P数据报应该被发送到另一
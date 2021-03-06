# Complex Network
> 具有自组织、自相似、吸引子、小世界、无标度中部分或全部性质的网络称之为复杂网络
言外之意，复杂网络就是指一种呈现高度复杂性的网络

## Small world theory (小世界特性)
> Stanley Milgram信件传递研究 心理学家将160封信随机分发给身边的人，让他们寄给他们的朋友，到达最终收件人平均经过6个人后就达到最终收件人
> 六度空间的概念由此而来

  * 特征路径长度（characteristic path length）：
     * 网络的全局特征
       * 两个节点的路径长度
        * 网络中 任选两个节点，连通这两个节点的最少边数
       * 络的特征路径长度
        * 网络中所有节点对的路径长度的平均值，定义为网

  * 聚合系数(clustering coefficient)：
    * 假设某个节点有kk条边则这kk条边连接的节点（kk个）之间最多可能存在的边的条数为k(k−1)/2k(k−1)/2
      * 节点的聚合系数
        * 实际存在的边数除以最多可能存在的边数得到的分值
      * 所有节点的聚合系数的均值为网络的聚合系数
        * 聚合系数是网络的局部特征，反映了相邻两个人之间朋友圈子的重合度

* 度及度分布  [1]k
  * 一个节点的度是指和该节点相关联的边的条数
  * 对有向网络
    * 节点的度分为入度和出度，节点的入度是指进入该节点的边的条数
    * 节点的出度是指从该节点出发的边的条数
  * 对无向网络
    * 一个节点的度是入度和出度之和。网络中所有节点的度的平均值为网络的平均度，一般记为 <K>

* 路径长度  
  * 网络统计特性中非常重要的一个特性
    * 路径pij是指从节点i出发到达节点j之间经过的节点序列（路径是没有环路的，节点不允许重复）
    * 节点之间可能存在多条路径，将这些路径中经过跳数最少的那条路径的长度定义为i和j之间的距离，记为dij
    * 网络平均路径长度是所有节点对之间距离的平均值，这是网络的全局特征，记为L，定义为公式2-1

* 节点中心性  
  * 网络统计特性中比较重要的
  * 社交网络中利用节点中心性来评价节点重要性的情况比较常,可以分为
    * 接近中心性
    * 介数中心性
    * 聚类系数以及核数

* 规则网络 任意两个点（个体）之间的特征路径长度长（通过多少个体联系在一起），但聚合系数高（你是朋友的朋友的朋友的几率高）。
    * 对于随机网络
      * 任意两个点之间的特征路径长度短，但聚合系数低。
    * 而小世界网络
      * 点之间特征路径长度小，接近随机网络，而聚合系数依旧相当高，接近规则网络。
  
* 无标度特性
  * 现实世界的网络大部分都不是随机网络，少数的节点往往拥有大量的连接，大部分节点却很少，节点的度数分布符合幂率分布
  * 这就被称为是网络的无标度特性（Scale-free） 
   * 将度分布符合幂律分布的复杂网络称为无标度网络。
  * 反映了复杂网络具有严重的异质性，其各节点之间的连接状况（度数）具有严重的不均匀分布性
   * 网络中少数称之为Hub点的节点拥有极其多的连接，而大多数节点只有很少量的连接。
   * 少数Hub点对无标度网络的运行起着主导的作用 从广义上说，无标度网络的无标度性是描述大量复杂系统整体上严重不均匀分布的一种内在性质。


* 其实复杂网络的无标度特性与网络的鲁棒性分析具有密切的关系。
  * 无标度网络中幂律分布特性的存在极大地提高了高度数节点存在的可能性
    * 无标度网络同时显现出针对随机故障的鲁棒性和针对蓄意攻击的脆弱性。
  * 无标度网络具有很强的容错性
    * 对基于节点度值的选择性攻击而言，其抗攻击能力相当差，高度数节点的存在极大地削弱了网络的鲁棒性，一个恶意攻击者只需选择攻击网络很少的一部分高度数节点，就能使网络迅速瘫痪。
  

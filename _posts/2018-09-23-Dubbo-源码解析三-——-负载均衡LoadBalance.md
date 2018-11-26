---
layout:     post
title:     Dubbo 源码解析四 —— 负载均衡LoadBalance
subtitle:    "\"  dubbo源码解析四 --- 负载均衡LoadBalance\""
date:       2018-11-21
author:     wangshouchang
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Dubbo
    - GitHub
    - Blog
    - Java


---

## 欢迎来我的 [Star](https://github.com/wsccoder/incubator-dubbo) [Followers](https://github.com/wsccoder) 后期后继续更新Dubbo别的文章

##### [Dubbo 源码分析系列之一环境搭建   博客园](https://www.cnblogs.com/wangshouchang/p/9694197.html)

##### [Dubbo 入门之二 ——- 项目结构解析 博客园](https://www.cnblogs.com/wangshouchang/p/9800659.html)

##### [Dubbo 源码分析系列之三 —— 架构原理  博客园](https://www.cnblogs.com/wangshouchang/p/9812757.html)

##### [Dubbo 源码解析四 —— 负载均衡LoadBalance    博客园](https://www.cnblogs.com/wangshouchang/p/10018141.html)



**下面是个人博客地址，页面比博客园美观一些其他都是一样的**

[Dubbo 源码分析系列之一环境搭建" Dubbo 源码分析系列之一环境搭建      个人博客地址"](http://wsccoder.top/2018/09/14/Dubbo-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E7%B3%BB%E5%88%97%E4%B9%8B%E4%B8%80%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/)

[Dubbo 入门之二 ——- 项目结构解析"Dubbo 项目结构解析         个人博客地址"](http://wsccoder.top/2018/10/15/Dubbo-%E5%85%A5%E9%97%A8%E4%B9%8B%E4%BA%8C-%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90/)

[Dubbo 源码分析系列之三 —— 架构原理" Dubbo 源码分析系列之三---架构原理        个人博客地址"](http://wsccoder.top/2018/10/17/Dubbo-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E7%B3%BB%E5%88%97%E4%B9%8B%E4%B8%89-%E6%9E%B6%E6%9E%84%E5%8E%9F%E7%90%86/)

[Dubbo 源码解析四 —— 负载均衡LoadBalance" dubbo源码解析四 --- 负载均衡LoadBalance   个人博客地址"](http://wsccoder.top/2018/11/21/Dubbo-%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%E4%B8%89-%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1LoadBalance/)

[Dubbo 源码解析五 —— 集群容错" dubbo源码解析五 --- 集群容错架构设计与原理分析       个人博客地址"](http://wsccoder.top/2018/11/23/Dubbo-%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%E4%BA%94-%E9%9B%86%E7%BE%A4%E5%AE%B9%E9%94%99/)





## 技术点

 - 面试中Dubbo负载均衡常问的几点
 - 常见负载均衡算法简介
 - Dubbo 官方文档介绍
 - Dubbo 负载均衡的策略
 - Dubbo 负载均衡源码解析



## 面试中Dubbo负载均衡常问的几点

1. 谈谈dubbo中的`负载均衡算法`及特点
2. `最小活跃数`算法中是如何统计这个活跃数的
3. 简单谈谈你对`一致性哈希算法`的认识
4. Dubbo默认的负载均衡策略是什么， 为什么使用 RandomLoadBalance 随机负载均衡算法
5. 谈谈几种负载均衡的优缺点
6. 如果让你设计负载均衡你将如何设计
7. 源码负载均衡你学到了什么
8. 有没有将Dubbo的负载均衡的原理使用在实际的项目中



## 常见负载均衡算法简介

*首先引出一点 负载均衡的目的是什么？*

> 当一台服务器的承受能力达到上限时，那么就需要多台服务器来组成集群，提升应用整体的吞吐量，那么这个时候就涉及到如何合理分配客户端请求到集群中不同的机器，这个过程就叫做负载均衡

**下面简单介绍几种负载均衡算法，有利于理解源码中为什么这样设计**

### 权重随机算法

>策略就是根据权重占比随机。算法很简单，就是一根数轴。然后利用伪随机数产生点，**看点落在了哪个区域从而选择对应的*服务器* 



![](https://user-gold-cdn.xitu.io/2018/6/3/163c5bfda5fe150f?imageslim)



### 权重轮询算法

>轮询算法是指依次访问可用服务器列表，其和随机本质是一样的处理，在无权重因素下，轮询只是在选数轴上的点时采取自增对长度取余方式。有权重因素下依然自增取余，再看选取的点落在了哪个区域。

### 一致性Hash负载均衡算法

利用Hash算法定位相同的服务器

- **普通的Hash**：当客户端请求到达是则使用 hash(client) % N,其中N是服务器数量，利用这个表达式计算出该客户端对应的Server处理
- **一致性Hash**：一致性Hash是把服务器分布变成一个环形，每一个hash(clinet)的结果会在该环上顺时针寻找第一个与其邻的`Server`节点

[一致性Hash算法](https://link.juejin.im/?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F34969168)

—————————— 下面这部分是来源于dubbo 官方文档 ------------------------------------

## Dubbo 官方文档介绍

### 负载均衡

在集群负载均衡时，Dubbo 提供了多种均衡策略，缺省为 Random LoadBalance 随机调用

### 负载均衡策略

### Random LoadBalance

- **随机**，按权重设置随机概率。
- 在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。

### RoundRobin LoadBalance

- **轮询**，按公约后的权重设置轮询比率。
- 存在慢的提供者累积请求的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上。

### LeastActive LoadBalance

- **最少活跃调用数**，相同活跃数的随机，活跃数指调用前后计数差。
- 使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。

### ConsistentHash LoadBalance

- **一致性 Hash**，相同参数的请求总是发到同一提供者。
- 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。
- 算法参见：<http://en.wikipedia.org/wiki/Consistent_hashing>
- 缺省只对第一个参数 Hash，如果要修改，请配置 `<dubbo:parameter key="hash.arguments" value="0,1" />`
- 缺省用 160 份虚拟节点，如果要修改，请配置 `<dubbo:parameter key="hash.nodes" value="320" />`

### 配置

### 服务端服务级别

```xml
<dubbo:service interface="..." loadbalance="roundrobin" />
```

### 客户端服务级别

```xml
<dubbo:reference interface="..." loadbalance="roundrobin" />
```

### 服务端方法级别

```xml
<dubbo:service interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:service>
```

### 客户端方法级别

```xml
<dubbo:reference interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:reference>
```

———————————————— Dubbo 官方文档已结束 ------------------------------------------



## Dubbo 负载均衡的策略

>上面官网文档已经说明 Dubbo 的负载均衡算法总共有4种
>
>- 随机算法 [**RandomLoadBalance**](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/RandomLoadBalance.java)（默认）
>- 轮训算法 **[RoundRobinLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/test/java/org/apache/dubbo/rpc/cluster/loadbalance/RoundRobinLoadBalanceTest.java)**
>- 最小活跃数算法 [**LeastActiveLoadBalance**](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/LeastActiveLoadBalance.java)
>- 一致性hash算法 **[ConsistentHashLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/test/java/org/apache/dubbo/rpc/cluster/loadbalance/ConsistentHashLoadBalanceTest.java)** 

**我们先看下接口的继承图**

![类继承结构](https://upload-images.jianshu.io/upload_images/1041678-2a60b5d43333fa71.png?imageMogr2/auto-orient/)

### LoadBalance

首先查看 LoadBalance 接口

> Invoker select(List<Invoker> invokers, URL url, Invocation invocation) throws RpcException;

[LoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-config/dubbo-config-api/src/test/java/org/apache/dubbo/config/mock/MockLoadBalance.java) 定义了一个方法就是从 invokers 列表中选取一个

### [AbstractLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/AbstractLoadBalance.java)

[AbstractLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/AbstractLoadBalance.java) 抽象类是所有负载均衡策略实现类的父类，实现了LoadBalance接口 的方法，同时提供抽象方法交由子类实现，

```java
 public <T> Invoker<T> select(List<Invoker<T>> invokers, URL url, Invocation invocation) {
        if (invokers == null || invokers.size() == 0)
            return null;
        if (invokers.size() == 1)
            return invokers.get(0);
        return doSelect(invokers, url, invocation);
    }

    protected abstract <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation);
```



**下面对四种均衡策略依次解析**

### [RandomLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/RandomLoadBalance.java)(随机)

>- **随机**，按权重设置随机概率。
>- 在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。

 [RandomLoadBalance#doSelect()](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/RandomLoadBalance.java)

```java
@Override
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
    //先获得invoker 集合大小
    int length = invokers.size(); // Number of invokers
    //总权重
    int totalWeight = 0; // The sum of weights
    //每个invoker是否有相同的权重
    boolean sameWeight = true; // Every invoker has the same weight?
    // 计算总权重
    for (int i = 0; i < length; i++) {
        //获得单个invoker 的权重
        int weight = getWeight(invokers.get(i), invocation);
        //累加
        totalWeight += weight; // Sum
        if (sameWeight && i > 0 && weight != getWeight(invokers.get(i - 1), invocation)) {
            sameWeight = false;
        }
    }
    // 权重不相等，随机后，判断在哪个 Invoker 的权重区间中
    if (totalWeight > 0 && !sameWeight) {
        // 随机
        // If (not every invoker has the same weight & at least one invoker's weight>0), select randomly based on totalWeight.
        int offset = ThreadLocalRandom.current().nextInt(totalWeight);
        // 区间判断
        // Return a invoker based on the random value.
        for (int i = 0; i < length; i++) {
            offset -= getWeight(invokers.get(i), invocation);
            if (offset < 0) {
                return invokers.get(i);
            }
        }
    }
    // 权重相等，平均随机
    // If all invokers have the same weight value or totalWeight=0, return evenly.
    return invokers.get(ThreadLocalRandom.current().nextInt(length));
}
```

**算法分析**

> 假定有3台dubbo provider:
> 10.0.0.1:20884, weight=2
> 10.0.0.1:20886, weight=3
> 10.0.0.1:20888, weight=4
> 随机算法的实现：
> totalWeight=9;
> 假设offset=1（即random.nextInt(9)=1）
> 1-2=-1<0？是，所以选中 10.0.0.1:20884, weight=2
>
> 假设offset=4（即random.nextInt(9)=4）
> 4-2=2<0？否，这时候offset=2， 2-3<0？是，所以选中 10.0.0.1:20886, weight=3
>
> 假设offset=7（即random.nextInt(9)=7）
> 7-2=5<0？否，这时候offset=5， 5-3=2<0？否，这时候offset=2， 2-4<0？是，所以选中 10.0.0.1:20888, weight=4

**流程图**

![流程图](https://upload-images.jianshu.io/upload_images/1041678-d69d4e6bb1acf146.png?imageMogr2/auto-orient/)



**[RoundRobinLoadBalance#doSelect()(轮询)](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/test/java/org/apache/dubbo/rpc/cluster/loadbalance/RoundRobinLoadBalanceTest.java)**

>- **轮询**，按公约后的权重设置轮询比率。
>- 存在慢的提供者累积请求的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上。



```java
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
    String key = invokers.get(0).getUrl().getServiceKey() + "." + invocation.getMethodName();
    int length = invokers.size(); // Number of invokers
    int maxWeight = 0; // The maximum weight
    int minWeight = Integer.MAX_VALUE; // The minimum weight
    final LinkedHashMap<Invoker<T>, IntegerWrapper> invokerToWeightMap = new LinkedHashMap<Invoker<T>, IntegerWrapper>();
    int weightSum = 0;
    // 计算最小、最大权重，总的权重和。
    for (int i = 0; i < length; i++) {
        int weight = getWeight(invokers.get(i), invocation);
        maxWeight = Math.max(maxWeight, weight); // Choose the maximum weight
        minWeight = Math.min(minWeight, weight); // Choose the minimum weight
        if (weight > 0) {
            invokerToWeightMap.put(invokers.get(i), new IntegerWrapper(weight));
            weightSum += weight;
        }
    }
    // 计算最小、最大权重，总的权重和。
    AtomicPositiveInteger sequence = sequences.get(key);
    if (sequence == null) {
        sequences.putIfAbsent(key, new AtomicPositiveInteger());
        sequence = sequences.get(key);
    }
    // 获得当前顺序号，并递增 + 1
    int currentSequence = sequence.getAndIncrement();
    // 权重不相等，顺序根据权重分配
    if (maxWeight > 0 && minWeight < maxWeight) {
        int mod = currentSequence % weightSum;// 剩余权重
        for (int i = 0; i < maxWeight; i++) {// 循环最大权重
            for (Map.Entry<Invoker<T>, IntegerWrapper> each : invokerToWeightMap.entrySet()) {
                final Invoker<T> k = each.getKey();
                final IntegerWrapper v = each.getValue();
                // 剩余权重归 0 ，当前 Invoker 还有剩余权重，返回该 Invoker 对象
                if (mod == 0 && v.getValue() > 0) {
                    return k;
                }
                // 若 Invoker 还有权重值，扣除它( value )和剩余权重( mod )。
                if (v.getValue() > 0) {
                    v.decrement();
                    mod--;
                }
            }
        }
    }
    // 权重相等，平均顺序获得
    // Round robin
    return invokers.get(currentSequence % length);
}
```



**算法说明**

>假定有3台权重都一样的dubbo provider:
> 10.0.0.1:20884, weight=100
> 10.0.0.1:20886, weight=100
> 10.0.0.1:20888, weight=100
> 轮询算法的实现：
> 其调用方法某个方法(key)的sequence从0开始：
> sequence=0时，选择invokers.get(0%3)=10.0.0.1:20884
> sequence=1时，选择invokers.get(1%3)=10.0.0.1:20886
> sequence=2时，选择invokers.get(2%3)=10.0.0.1:20888
> sequence=3时，选择invokers.get(3%3)=10.0.0.1:20884
> sequence=4时，选择invokers.get(4%3)=10.0.0.1:20886
> sequence=5时，选择invokers.get(5%3)=10.0.0.1:20888
>
> 



### [LeastActiveLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/LeastActiveLoadBalance.java)(最少活跃数)

>- **最少活跃调用数**，相同活跃数的随机，活跃数指调用前后计数差。
>- 使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。



```java
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
    // 总个数
    int length = invokers.size(); // Number of invokers
    // 最少的活跃数
    int leastActive = -1; // The least active value of all invokers
    // 相同最小活跃数的个数
    int leastCount = 0; // The number of invokers having the same least active value (leastActive)
    // 相同最小活跃数的下标
    int[] leastIndexs = new int[length]; // The index of invokers having the same least active value (leastActive)
    //总权重
    int totalWeight = 0; // The sum of weights
    // 第一个权重，用于于计算是否相同
    int firstWeight = 0; // Initial value, used for comparision
    // 是否所有权重相同
    boolean sameWeight = true; // Every invoker has the same weight value?
    // 计算获得相同最小活跃数的数组和个数
    for (int i = 0; i < length; i++) {
        Invoker<T> invoker = invokers.get(i);
        // 活跃数
        int active = RpcStatus.getStatus(invoker.getUrl(), invocation.getMethodName()).getActive(); // Active number
        // 权重
        int weight = invoker.getUrl().getMethodParameter(invocation.getMethodName(), Constants.WEIGHT_KEY, Constants.DEFAULT_WEIGHT); // Weight
        // 发现更小的活跃数，重新开始
        if (leastActive == -1 || active < leastActive) { // Restart, when find a invoker having smaller least active value.
            // 记录最小活跃数
            leastActive = active; // Record the current least active value
            // 重新统计相同最小活跃数的个数
            leastCount = 1; // Reset leastCount, count again based on current leastCount
            // 重新记录最小活跃数下标
            leastIndexs[0] = i; // Reset
            // 重新统计总权重
            totalWeight = weight; // Reset
            // 记录第一个权重
            firstWeight = weight; // Record the weight the first invoker
            // 还原权重标识
            sameWeight = true; // Reset, every invoker has the same weight value?
        // 累计相同最小的活跃数
        } else if (active == leastActive) { // If current invoker's active value equals with leaseActive, then accumulating.
            // 累计相同最小活跃数下标
            leastIndexs[leastCount++] = i; // Record index number of this invoker
            // 累计总权重
            totalWeight += weight; // Add this invoker's weight to totalWeight.
            // 判断所有权重是否一样
            // If every invoker has the same weight?
            if (sameWeight && i > 0
                    && weight != firstWeight) {
                sameWeight = false;
            }
        }
    }
    // assert(leastCount > 0)
    if (leastCount == 1) {
        // 如果只有一个最小则直接返回
        // If we got exactly one invoker having the least active value, return this invoker directly.
        return invokers.get(leastIndexs[0]);
    }
    if (!sameWeight && totalWeight > 0) {
        // 如果权重不相同且权重大于0则按总权重数随机
        // If (not every invoker has the same weight & at least one invoker's weight>0), select randomly based on totalWeight.
        int offsetWeight = ThreadLocalRandom.current().nextInt(totalWeight);
        // 并确定随机值落在哪个片断上
        // Return a invoker based on the random value.
        for (int i = 0; i < leastCount; i++) {
            int leastIndex = leastIndexs[i];
            offsetWeight -= getWeight(invokers.get(leastIndex), invocation);
            if (offsetWeight <= 0)
                return invokers.get(leastIndex);
        }
    }
    // 如果权重相同或权重为0则均等随机
    // If all invokers have the same weight value or totalWeight=0, return evenly.
    return invokers.get(leastIndexs[ThreadLocalRandom.current().nextInt(leastCount)]);
}
```



简单思路介绍

概括起来就两部分,一部分是`活跃数`和`权重`的统计,另一部分是选择`invoker`.也就是他把最小活跃数的`invoker`统计到`leastIndexs`数组中,如果权重一致(这个一致的规则参考上面的随机算法)或者总权重为0,则均等随机调用,如果不同,则从`leastIndexs`数组中按照权重比例调用

**算法说明**

>最小活跃数算法实现：
> 假定有3台dubbo provider:
> 10.0.0.1:20884, weight=2，active=2
> 10.0.0.1:20886, weight=3，active=4
> 10.0.0.1:20888, weight=4，active=3
> active=2最小，且只有一个2，所以选择10.0.0.1:20884
>
>假定有3台dubbo provider:
> 10.0.0.1:20884, weight=2，active=2
> 10.0.0.1:20886, weight=3，active=2
> 10.0.0.1:20888, weight=4，active=3
> active=2最小，且有2个，所以从[10.0.0.1:20884,10.0.0.1:20886 ]中选择；
> 接下来的算法与随机算法类似：
> 假设offset=1（即random.nextInt(5)=1）
> 1-2=-1<0？是，所以选中 10.0.0.1:20884, weight=2
> 假设offset=4（即random.nextInt(5)=4）
> 4-2=2<0？否，这时候offset=2， 2-3<0？是，所以选中 10.0.0.1:20886, weight=3



**流程图**

![流程图](https://upload-images.jianshu.io/upload_images/1041678-974214615c58dce8.png?imageMogr2/auto-orient/)



### [ConsistentHashLoadBalance](https://github.com/wsccoder/incubator-dubbo/blob/master/dubbo-cluster/src/main/java/org/apache/dubbo/rpc/cluster/loadbalance/LeastActiveLoadBalance.java)(一致性哈希)

>- **一致性 Hash**，相同参数的请求总是发到同一提供者。
>- 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。



源码其实分为四个步骤

1. 定义全局一致性hash选择器的`ConcurrentMap<String, ConsistentHashSelector<?>> selectors`，key为方法名称，例如com.alibaba.dubbo.demo.TestService.getRandomNumber
2. 如果一致性hash选择器不存在或者与以前保存的一致性hash选择器不一样（即dubbo服务provider有变化，通过System.identityHashCode(invokers)计算一个identityHashCode值） 则需要重新构造一个一致性hash选择器
3. 构造一个一致性hash选择器ConsistentHashSelector的源码如下，通过参数i和h打散Invoker在TreeMap上的位置，replicaNumber默认值为160，所以最终virtualInvokers这个TreeMap的size为`invokers.size()*replicaNumber`
4. 选择Invoker的步骤
   1. 根据Invocation中的参数invocation.getArguments()转成key
   2. 算出这个key的md5值
   3. 根据md5值的hash值从TreeMap中选择一个Invoker

**下面源码解析+注释**

```java
public class ConsistentHashLoadBalance extends AbstractLoadBalance {
    public static final String NAME = "consistenthash";

    /**
     * 服务方法与一致性哈希选择器的映射
     *
     * KEY：serviceKey + "." + methodName
     */
    private final ConcurrentMap<String, ConsistentHashSelector<?>> selectors = new ConcurrentHashMap<String, ConsistentHashSelector<?>>();

    @SuppressWarnings("unchecked")
    @Override
    protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
        String methodName = RpcUtils.getMethodName(invocation);
        // 基于 invokers 集合，根据对象内存地址来计算定义哈希值
        String key = invokers.get(0).getUrl().getServiceKey() + "." + methodName;
        int identityHashCode = System.identityHashCode(invokers);
        // 获得 ConsistentHashSelector 对象。若为空，或者定义哈希值变更（说明 invokers 集合发生变化），
        // 进行创建新的 ConsistentHashSelector 对象
        ConsistentHashSelector<T> selector = (ConsistentHashSelector<T>) selectors.get(key);
        if (selector == null || selector.identityHashCode != identityHashCode) {
            selectors.put(key, new ConsistentHashSelector<T>(invokers, methodName, identityHashCode));
            selector = (ConsistentHashSelector<T>) selectors.get(key);
        }
        return selector.select(invocation);
    }

    private static final class ConsistentHashSelector<T> {

        /**
         * 虚拟节点与 Invoker 的映射关系
         */
        private final TreeMap<Long, Invoker<T>> virtualInvokers;

        /**
         * 每个Invoker 对应的虚拟节点数
         */
        private final int replicaNumber;

        /**
         * 定义哈希值
         */
        private final int identityHashCode;

        /**
         * 取值参数位置数组
         */
        private final int[] argumentIndex;

        ConsistentHashSelector(List<Invoker<T>> invokers, String methodName, int identityHashCode) {
            this.virtualInvokers = new TreeMap<Long, Invoker<T>>();
            // 设置 identityHashCode
            this.identityHashCode = identityHashCode;
            URL url = invokers.get(0).getUrl();
            // 初始化 replicaNumber
            this.replicaNumber = url.getMethodParameter(methodName, "hash.nodes", 160);
            // 初始化 argumentIndex
            String[] index = Constants.COMMA_SPLIT_PATTERN.split(url.getMethodParameter(methodName, "hash.arguments", "0"));
            argumentIndex = new int[index.length];
            for (int i = 0; i < index.length; i++) {
                argumentIndex[i] = Integer.parseInt(index[i]);
            }
            // 初始化 virtualInvokers
            for (Invoker<T> invoker : invokers) {
                String address = invoker.getUrl().getAddress();
                // 每四个虚拟结点为一组，为什么这样？下面会说到
                for (int i = 0; i < replicaNumber / 4; i++) {
                    // 这组虚拟结点得到惟一名称
                    byte[] digest = md5(address + i);
                    // Md5是一个16字节长度的数组，将16字节的数组每四个字节一组，
                    // 分别对应一个虚拟结点，这就是为什么上面把虚拟结点四个划分一组的原因
                    for (int h = 0; h < 4; h++) {
                        // 对于每四个字节，组成一个long值数值，做为这个虚拟节点的在环中的惟一key
                        long m = hash(digest, h);
                        virtualInvokers.put(m, invoker);
                    }
                }
            }
        }

        public Invoker<T> select(Invocation invocation) {
            // 基于方法参数，获得 KEY
            String key = toKey(invocation.getArguments());
            // 计算 MD5 值
            byte[] digest = md5(key);
            // 计算 KEY 值
            return selectForKey(hash(digest, 0));
        }

        /**
         * 基于方法参数，获得 KEY
         * @param args
         * @return
         */
        private String toKey(Object[] args) {
            StringBuilder buf = new StringBuilder();
            for (int i : argumentIndex) {
                if (i >= 0 && i < args.length) {
                    buf.append(args[i]);
                }
            }
            return buf.toString();
        }

        /**
         * 选一个 Invoker 对象
         * @param hash
         * @return
         */
        private Invoker<T> selectForKey(long hash) {
            // 得到大于当前 key 的那个子 Map ，然后从中取出第一个 key ，就是大于且离它最近的那个 key
            Map.Entry<Long, Invoker<T>> entry = virtualInvokers.ceilingEntry(hash);
            // 不存在，则取 virtualInvokers 第一个
            if (entry == null) {
                entry = virtualInvokers.firstEntry();
            }
            // 存在，则返回
            return entry.getValue();
        }

        /**
         * 对于每四个字节，组成一个 Long 值数值，做为这个虚拟节点的在环中的惟一 KEY
         * @param digest
         * @param number
         * @return
         */
        private long hash(byte[] digest, int number) {
            return (((long) (digest[3 + number * 4] & 0xFF) << 24)
                    | ((long) (digest[2 + number * 4] & 0xFF) << 16)
                    | ((long) (digest[1 + number * 4] & 0xFF) << 8)
                    | (digest[number * 4] & 0xFF))
                    & 0xFFFFFFFFL;
        }

        /**
         * MD5 是一个 16 字节长度的数组，将 16 字节的数组每四个字节一组，
         * 分别对应一个虚拟结点，这就是为什么上面把虚拟结点四个划分一组的原因
         * @param value
         * @return
         */
        private byte[] md5(String value) {
            MessageDigest md5;
            try {
                md5 = MessageDigest.getInstance("MD5");
            } catch (NoSuchAlgorithmException e) {
                throw new IllegalStateException(e.getMessage(), e);
            }
            md5.reset();
            byte[] bytes;
            try {
                bytes = value.getBytes("UTF-8");
            } catch (UnsupportedEncodingException e) {
                throw new IllegalStateException(e.getMessage(), e);
            }
            md5.update(bytes);
            return md5.digest();
        }

    }

}
```



一致性哈希算法的三个关键点 **原理**， **down机影响面**， **虚拟节点**

**原理**

[一致性Hash(Consistent Hashing)原理剖析](https://blog.csdn.net/lihao21/article/details/54193868)

**down 机影响**

在某个节点挂机的时候，会根据虚拟节点选择下一个节点。只影响到一个节点，其他的节点不受到影响

**虚拟节点**

根据一致性Hash算法将生成很多的虚拟节点，这些节点落在圆环中。当某个节点down掉，则压力会给到指定的虚拟节点





>**参考文章**
>
>[常见负载均衡算法分析](https://juejin.im/entry/5b13e712e51d4506da5a039b)
>
>[Dubbo 官方文档 负载均衡](https://dubbo.incubator.apache.org/zh-cn/docs/user/demos/loadbalance.html)
>
>[阿飞的博客 14.dubbo源码-负载均衡](https://www.jianshu.com/p/10c30d7b8b6a)
>
>[dubbo源码解析-LoadBalance](https://www.jianshu.com/p/53feb7f5f5d9)
>
>[Dubbo 负载均衡策略与实现](https://www.cnblogs.com/javanoob/p/dubbo_loadbalance.html)
>
>[精尽 Dubbo 源码解析 —— 集群容错（四）之 LoadBalance 实现](http://svip.iocoder.cn/Dubbo/cluster-4-impl-loadbalance/)
>
>








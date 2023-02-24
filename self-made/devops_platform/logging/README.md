## 日志平台


### 背景介绍
服务架构，从传统的单体服务已经进化到今天的微服务，以及下一代的无服务器架构。  
但是在日志的查询中，大部分的公司还是继承了单体服务的查询思路，使用elasticsearch作为日志引擎，es在使用上往往有以下几个问题，

1. 根据关键字搜索问题，在互联网下，每天服务的请求是巨量的，根据关键字搜索到的内容往往包含了太多的无关内容
2. 微服务架构下，一个请求往往需要经过几个服务，如果要复原此次请求下所有服务在处理此次请求所产生的日志，需要逐个定位具体服务，然后在根据关键字去搜索，繁琐且效率低
3. elasticsearch是基于内容搜索，资源消耗量大

下图展示了传统elasticsearch下的跨服务日志搜索的架构:
<p align="center">
   <img src="tradition_flow.jpg">
</p>
    服务间相互隔离，无法串联数据

### 方案调研

#### [loki](https://grafana.com/oss/loki/)
    loki是grafana公司开源的日志平台

与elasticsearch相比，loki是基于标签的日志平台，由于日志在持久化时已经被贴上了标签，因此在资源消耗上有了很大的节省，  
但是其在架构方面还是有很多问题
1. [架构复杂](https://grafana.com/docs/loki/latest/fundamentals/architecture/components/)，组件过于复杂，其在架构端考虑了底层设施的不可用性，所以在架构中融入了太多的组件，维护成本大
2. 日志量巨大时，需要综合调优众多组件，此操作将带来风险性，一定程度上也加大了平台的开销
3. 处理海量标签时，特别是为每一条日志添加微服务架构下为每一次服务请求所产生的traceid作为标签，[loki是不建议添加此种无共性的标签](https://grafana.com/docs/loki/latest/fundamentals/labels/)，若加上该标签，将极大的添加日志的分片数量，降低性能

#### 半自研方案

##### 日志采集客户端
基于的[promtail](https://grafana.com/docs/loki/latest/clients/promtail/)方案，简化其架构，优化数据结构

##### 日志架构
<p align="center">
   <img src="hh-logging.jpg">
</p>


##### 持久化端的数据库设设计
<p align="center">
   <img src="tracedLogging.jpg">
</p>

1. 存入到Cassandra的日志，将数据存放至**loggings**表，两个物化视图，**search_logging**与**trace_logging**将根据base表，logging自动更新其中的内容  
2. **search_logging**适用于搜索特定服务特定版本下在某一时间区间内的日志  
3. **trace_logging**适用于根据traceid与spanid精确定位到与traceid以及spanid关联的日志  
<p align="center">
   <img src="tables.png">
</p>

### 日志平台功能概览
<p align="center">
   <img src="overview.png">
</p>

下面我们将重点介绍某些图表
#### 报表统计，从多个角度统计日志的产生量，如pod产生的日志行数排行，以项目分组哪个项目产生的日志量多等等
<p align="center">
   <img src="logging_qty_group.png">
</p>

#### 采集客户端 - 日志产生的时间，全链路日志端到端监控，日志被采集的时间，日志被消费的时间以及他们之间的时间差
<p align="center">
   <img src="logging_consumer_gap.png">
</p>

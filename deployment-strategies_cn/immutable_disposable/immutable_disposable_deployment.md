<p align="center">
   <img width="200" src="immutable.png">
</p>

### 不可变/用完及丢弃 部署
    不可变/用完及丢弃 部署，是一种可以更新应用依赖的部署策略

#### 注：
    改方案主要用于代码与虚拟机分离的情况，因为如果AMI中包含了代码，则在下次打包编译时，使用新版本的基础AMI即可
    现实中，一般很少用到，却是必须掌握的一种策略
    更适用于IaaS层，比如虚拟机，
    可以保证虚拟机能平滑升级，服务不中断，用户无感知

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: |  :sun_with_face: :sun_with_face:| :x: | :sun_with_face: :sun_with_face: :sun_with_face:| :x: | :x: |

#### 说明：
1. 部署流程复杂，前期需要准备工作
2. 对公司的自动化设施要求高
3. 服务要平滑过度，不能影响用户
4. 允许产生临时账单

#### 实际场景：
随着时间的推移与某些特殊情况，虚拟机可能要进行一次大版本升级，此时，已经超过了常见部署模式的范畴
公司的系统镜像是基于版本V1，此时发现漏洞，或者公司要升级过旧的虚拟机镜像版本。

#### 实现方案：

##### AWS
基于不同的边界，有多种方案，详细说明请参考AWS_Blue_Green_Deployments的[whitepaper](https://github.com/liubq919/AWS_Blue_Green_Deployments_cn)。
- DNS - 部署一套使用新基础AMI的集群，部署服务，然后切换DNS解析
- 新autoscalling - 在同一个ELB下，新创建一个autoscalling group，然后再通过伸缩更新旧autoscalling group
- 更新autoscalling的AMI - 更新autoscalling的AMI，扩大autoscalling group容量，在新创建的EC2上部署服务，再收缩容量，移除旧版的EC2，直到完全替换

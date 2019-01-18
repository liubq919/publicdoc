<p align="center">
   <img width="200" src="immutable.png">
</p>

#### 不可变/用完及丢弃 部署
    不可变/用完及丢弃 部署，是一种可以更新应用依赖的部署策略

##### 注：
    现实中，一般很少用到，却是必须掌握的一种策略
    更适用于IaaS层，比如虚拟机，
    类似于滚动部署
    随着时间的推移与某些特殊情况，虚拟机可能要进行一次大版本升级，此时，已经超过了常见部署模式的范畴
    可以保证虚拟机能平滑升级，服务不中断，用户无感知

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: |  :sun_with_face: :sun_with_face:| :x: | :sun_with_face: :sun_with_face: :sun_with_face:| :x: | :x: |

##### 说明：
1. 部署流程复杂，前期需要准备工作
2. 对公司的自动化设施要求高
3. 服务要平滑过度，不能影响用户
4. 允许产生临时账单

##### 场景：
1. IaaS层的虚拟机版本过低
2. 服务不中断
3. 回滚时间长
   
##### 实际场景：
公司的系统镜像是基于版本A-1，此时发现安全漏洞，或者公司要升级过旧的虚拟机镜像版本。
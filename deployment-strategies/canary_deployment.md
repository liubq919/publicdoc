<p align="center">
   <img width="200" src="canary.png">
</p>

#### 金丝雀部署
    金丝雀部署，在初期让部分用户访问新版本，然后逐渐提高能访问新版本用户的比例，最后实现版本的完全升级

##### 典故：
>在以前，工人挖矿作业中，可能会有一些有毒气体产生。由于人对有毒或者能引起爆炸的气体不是很敏感，无法立刻感 知到。在条件有限的情况下，有工人将金丝雀放在鸟笼中，带入作业现场。危险气体会先让金丝雀毙命，所以工人能通过关注金丝雀的状态而感知有毒气体。

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face:  |  :sun_with_face: :sun_with_face: :sun_with_face:   | :x:   | :sun_with_face: | :x: | :x: |

##### 注：
金丝雀部署与[蓝绿部署](blue_green_deployment.md)可以说是相同的。
具体的使用场景与说明，请查[蓝绿部署](blue_green_deployment.md)
<p align="center">
   <img width="200" src="blue-green.png">
</p>

#### 蓝绿部署
    蓝绿部署是一种可以同时测试多版本，降低风险，并且对发布进度完全可控的部署方案。

##### 注：
>  稍后将提供[AWS_Blue_Green_Deployments](https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf)的中文翻译版

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face:  |  :sun_with_face: :sun_with_face: :sun_with_face:   | :x:   | :sun_with_face: | :x: | :x: |

##### 说明：
1. 部署速度慢，实现细节比较复杂
2. 需要创建新资源，同时为了保证服务可用性，可能创建一个与现有集群相类似的集群环境
3. 不存在dowmtime
4. 回滚迅速，只需要修改Route53或者service的配置信息
5. 用户无感知
6. 可以精确控制部署进度，实时控制版本之间的切换
   
##### 场景：
1. 对服务的连续性有很高的要求，
2. 新版本部署后，一旦发现问题，可以及时控制
3. 对版本的升级完全可控，可升可将
4. 允许消耗更多的资源

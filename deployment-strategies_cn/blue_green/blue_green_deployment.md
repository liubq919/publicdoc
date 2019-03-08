<p align="center">
   <img width="200" src="blue-green.png">
</p>

#### 蓝绿部署
    蓝绿部署是一种可以同时测试多版本，降低风险，并且对发布进度完全可控的部署方案。

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face:  |  :sun_with_face: :sun_with_face: | :x:   | :sun_with_face: | :x: | :x: |

##### 说明：
1. 部署速度慢，实现细节比较复杂
2. 需要创建新资源，同时为了保证服务可用性，可能创建一个与现有集群相类似的集群环境
3. 不存在dowmtime
4. 回滚迅速，只需要修改Route53，service或者DNS的配置信息
5. 用户无感知
6. 可以精确控制部署进度，实时控制版本之间的切换
   
##### 实际场景：
- 核心服务升级过程不能出现任何错误
- 服务迁移
- 在新环境中，对部分用户进行测试

##### 实现方案

###### AWS
请参考AWS_Blue_Green_Deployments的[whitepaper](https://github.com/liubq919/AWS_Blue_Green_Deployments_cn)。

###### kubernetes
基于不同的边界，都可以实现蓝/绿部署，我们介绍主流的且易于实现的几种

- DNS解析 - 在新kubernetes集群中部署绿版本的服务，通过Ingress（或者istio中的Gateway）对外提供服务，在DNS处，添加新环境的ingress解析，至此，服务就可以被路由到两个kubernetes集群
- 部署绿版本的deployment - 部署包含绿版本的deployment，包含该服务的service也可以同样过滤的label，通过控制deploymeng的replicas，调整蓝绿两个环境的容量。

以第二个为例：

1. 执行 [blue.yaml](blue.yaml)
   ```
   kubectl apply -f blue.yaml
   ```

2. 执行 [green.yaml](green.yaml)
   ```
   kubectl apply -f green.yaml
   ```

3. 查看pod的状态
   ```
   kubectl get service kubia
   kubectl get pods --show-labels | grep kubia
   ```
4. 访问kubia服务，可以看到v1与v2会间隔出现

5. green版本如果稳定且没有问题，则可以删除blue版本
   ```
   kubectl delete -f blue.yaml
   ```

注明
- 修改相应的镜像路径
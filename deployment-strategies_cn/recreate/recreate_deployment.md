<p align="center">
   <img width="200" src="recreate.png">
</p>

### 重建部署
    先删除所有承载服务的载体，然后再部署新版本的服务

#### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face: :sun_with_face: :sun_with_face: |  :sun_with_face: | :o:  | :sun_with_face: :sun_with_face: | :o: | :x: |

#### 说明：
1. 不会产生更多的账单，承载服务载体的数量与性能一般情况下不会有变化
2. 存在dowmtine，在升级服务时，有一段时间内，没有任何载体去运行服务
3. 回滚较慢，需要再删除所有载体，再次部署服务
4. 用户在某一时间段内无法访问服务

#### 实际场景:
不需要考虑服务可用性，需要快速部署。


#### 实现方案

###### AWS
1. 更新资源的Cloud Formation，更新其AMI的版本
2. 将AutoScalling的容量设置为0，清空所有的EC2
3. 清空完成后，再调整AutoScalling的容量

注明：
此模式中，应用代码需要与基础AMI共同构建成包含代码的AMI，如果代码与AMI分离，则需要借鉴[就地更新](../in_place/in_place_deployment.md)。

###### kubernetes
移除当前的pod，再创建使用新版本的pod

1. 执行 [app-v1.yaml](app-v1.yaml)
   ```
   kubectl apply -f app-v1.yaml
   ```

2. 在步骤一的pod资源为Running状态后，执行 [app-v2.yaml](app-v2.yaml)
   ```
   kubectl apply -f app-v2.yaml
   ```

3. 查看pod的状态
   ```
   kubectl get pods | grep kubia
   ```
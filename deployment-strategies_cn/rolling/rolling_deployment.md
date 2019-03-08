<p align="center">
   <img width="200" src="rolling.png">
</p>

#### 滚动部署
    在提供服务的载体中，逐渐升级或者替换服务的版本

##### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face:  |  :sun_with_face:   | :x:   | :sun_with_face: :sun_with_face: :sun_with_face: | :x: | :x:|

##### 说明：
1. 部署速度一般，因为需要逐个升级与替换
2. 可能需要创建新的资源，AWS平台与kubernetes下会有部分不同
3. 不存在dowmtime，因为是逐个升级，
4. 回滚时间长，需要再次逐个降级
5. 用户无感知，对用户访问没有影响

##### 实际场景：
在基于gitflow或者某种分支开发流程中，develop分支，访问量大，此分支的可用性要求，可能仅次于生产环境，是内部开发人员都依赖的环境。所以，每一次发布，都不能影响用户的使用。

##### 实现方案

###### AWS（基于EC2的创建与启动进行考量，可有以下两种方案）
- 不创建新EC2
    在当前服务器上，通过ansible逐个（或者按照一定比例）连接EC2，在升级服务之前，通知负载均衡（ELB）将该EC2下线，升级服务，通过健康检查，再通知负载均衡再将该EC2上线。以此步骤逐步完成所有服务器上应用的更新。
- 创建新的EC2
  - 通过AWS Elastic Beanstalk - AWS Elastic Beanstalk提供了全面支持，可以[参考文档](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html)。
  - 通过修改Cloud Formation - 如果EC2集群的创建，是使用了Cloud Formation，并包含AutoScalling，ELB等，可以修改EC2的AMI，将其更新为包含最新代码的AMI，删除当前运行旧版本的EC2，或者扩容（保障负载量），逐步替换当前运行旧版本的EC2。

注明：
该方案的两种方式，要求了不同的代码编译打包方式，也对当前集群的负载能力有不同的影响。
- 不创建新EC2 - 代码编译成独立的包并发送到中央仓库，再通过ansible部署到EC2；部署过程中，由于某些EC2已经无法提供服务，所以这会影响当前集群的负载能力
- 创建新的EC2 - 代码编译后的文件需要与基础AMI一同组成新版本的AMI；部署中，可以设置扩大集群容量或者保持原有容量

###### kubernetes

1. 执行 [deployment-v1.yaml](deployment-v1.yaml)
   ```
   kubectl apply -f deployment-v1.yaml
   ```

2. 在步骤一的pod资源为Running状态后，再执行 [deployment-v2.yaml](deployment-v2.yaml)
   ```
   kubectl apply -f deployment-v2.yaml
   ```

3. 查看pod的更新状态
   ```
   kubectl get deploy kubia
   kubectl get pods | grep kubia
   ```


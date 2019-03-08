<p align="center">
   <img width="200" src="a_b.png">
</p>

### 滚动部署
    基于用户数据，将请求路由到特定版本的服务

#### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face:  |  :sun_with_face: :sun_with_face:  | :x:   | :sun_with_face:  | :x: | :o: |

#### 说明：
1. 部署速度一般，前期准备工作较多
2. 需要创建新的资源，
3. 不存在dowmtime，
4. 回滚时间段，只需要修改策略
5. 用户无感知，对用户访问没有影响
6. 能准确定位到用户

#### 实际场景：
- A/B部署，可以目标性的发布公司的产品，而且能将风险降到最低
- 在非生产环境下，无法测试所有的情况，可以通过A/B部署，准确定位用户，与生产环境的用户一起完成测试

#### 实现方案
##### AWS
AWS在EC2，需要用户自己通过nginx等工具完成A/B部署，因此，本篇幅不做介绍

##### kubernetes
架构层做到A/B部署，需要使用第三方框架[istio](https://istio.io/)。

1. 执行 [a_b.yaml](a_b.yaml)
   ```
   kubectl apply -f <(istioctl kube-inject -f a_b.yaml)
   ```

2. 验证请求，非admin用户，将访问到v1
   ```
   curl -X GET http://kubia.com 
   ```

3. admin用户，将访问到v2
   ```
   curl -X GET http://kubia.com -H 'role: admin'
   ```
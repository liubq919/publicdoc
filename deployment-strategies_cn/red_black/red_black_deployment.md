<p align="center">
   <img width="200" src="red-black.png">
</p>

### 红黑部署
    红黑部署，最早由Netfilx提出与实践，是一种将通过测试后的新版本做到立刻上线的部署方案

#### 注：
    红黑部署与蓝绿部署很相似，只不过红黑部署在切换版本的时候，不会像蓝绿部署那样平滑
    红，提供服务的版本；黑，未提供服务的版本
    提供服务的版本，并不是说新版本为红，旧版本为黑，
    在升级服务中，哪个版本提供服务，就是红；未提供服务，处于预备模式的，就是黑

#### 概况：

| 部署速度 |  资源消耗  | Downtime  | 回滚时间  | 对用户的负面影响| 目标用户|
| -------| ---------| --------| --------| --------|--------|
| :sun_with_face: :sun_with_face: :sun_with_face:  |  :sun_with_face: :sun_with_face: :sun_with_face:   | :x:   | :sun_with_face: | :x: | :x: |

#### 说明：
1. 部署速度慢，每个步骤都要保证
2. 需要创建新资源，在一段时间内，可能存在一定的闲置，会产生更多的账单
3. 不存在dowmtime
4. 回滚迅速，无缝切换红黑，一旦发现问题，可以立刻切换
5. 用户无感知
   
#### 实际场景：
在很多核心业务场景中，一般不需要同时对外提供多个版本，新版本上线前，就已经经过完备的测试。由于是重要的业务，部署完成后一旦出现问题，需要立即处理。

#### 实现方案

##### AWS
由于蓝绿部署与红黑部署最大的差别就是平滑过度，所以用户可以参考AWS_Blue_Green_Deployments的[whitepaper](https://github.com/liubq919/AWS_Blue_Green_Deployments_cn)中所描述的方案，当升级版本时，做到整体切换，而非平滑过度即可。

##### kubernetes
基于不同的边界，可以有以下方案

- DNS - 在另一个kubernetes环境中，完整部署新版本的服务，当服务经过测试都可用时，修改DNS入口，将服务地址解析到该kubernetes的入口地址
- service - 由于service可以通过label过滤pod，所以可以部署新版本的deployment，然后通过更新service中的label，将service的请求导向新版本的deployment

1. 执行 [v1.yaml](v1.yaml)
   ```
   kubectl apply -f v1.yaml
   ```

2. 执行 [v2.yaml](v2.yaml)
   ```
   kubectl apply -f v2.yaml
   ```

3. 查看pod的状态
   ```
   kubectl get deploy kubia kubia-v2
   ```
4. 切换版本
   ```
   kubectl apply -f service-v2.yaml
   ```    
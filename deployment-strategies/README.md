<p align="center">
   <img width="200" src="deployment.png">
</p>


部署策略，一直是一个很重要的课题。从web时代，就一直存在。近十年，由于公有云的普及与微服务架构模式的流行，部署策略诞生了很多新的概念。

传统的IDC时代，管理员可能更关注业务的可用性。

到了公有云时代，这个时候，只考虑业务可用性，可能已经不能符合业务场景的完全需求，每一种发布模式所消耗的资源，对目标用户的影响，回滚时间，服务不可用时间等，都需要管理员综合考虑才能做出最符合当前场景下的方案。

同样，应用运行的载体，有虚拟机，也有容器。如何在每一种载体下，实现相应的部署策略，都需要熟悉掌握。

[DevOps Engineering on AWS](https://aws.amazon.com/training/course-descriptions/devops-engineering/)，其整个课程体系对于在公有云下的Devops做出了详细的介绍与实践。同样，[AWS Certified DevOps Engineer - Professional](https://aws.amazon.com/certification/certified-devops-engineer-professional/)的考试中，对于[发布策略与自动化实现](https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS_certified_devops_engineer_professional_blueprint.pdf)的考核，也占据了55%，看来是一个合格的Devops人员必须要掌握的技能了。

|           Domain             |             % of Examination            |
| --------------------------| ---------------------------- |
| Domain 1: Continuous Delivery and Process Automation     |  55%|
| Domain 2: Monitoring, Metrics, and Logging      |  20%|
| Domain 3: Security, Governance, and Validation      |  10%|
| Domain 4: High Availability and Elasticity     |  15%|
| TOTAL      |  100%|

不仅仅是发布策略的实现，在不同载体下的具体实现方案，如自动化，每一个方案所产生的账单，架构及代码的落地等，管理员都需要关注。本系列，将在每一个策略下，阐述每一个细节的实现方案，以及综合维度下的各种对比，给读者一个综合的参考。

注：本系列所使用的平台  

|   运行载体  |     平台  |
| -----------| ----------|
| 虚拟机 |   AWS   |
| 容器 |  Kubernetes |

##### [1: 就地部署(In-Place Deployment)](in_place_deployment.md)
##### [2: 滚动部署(Rolling Deployment)](rolling_deployment.md)
##### [3: 蓝绿部署(Blue/Green Deployment)](blue_green_deployment.md)
##### [4: 重建部署(Recreate Deployment)](recreate_deployment.md)
##### [5: 红黑部署(Red/Black Deployment)](red_black_deployment.md)
##### [6: 金丝雀部署(Canary Deployment)](canary_deployment.md)
##### [7: 不可变/用完及丢弃部署(Immutable/Disposable Deployment)](immutable_disposable_deployment.md)

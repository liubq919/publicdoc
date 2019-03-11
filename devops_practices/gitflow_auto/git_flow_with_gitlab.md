<p align="center">
   <img width="200" src="GitflowAuto.png">
</p>

#### Gitflow自动化实现
在前面的章节中，我们介绍了[Gitflow](../../gitflow-workflow-cn/README.md)流程。掌握了规范并不是最终的目的，我们希望能通过统一的规范，能加速为客户提供公司产品的价值。
在本章节中，我们将基于[Gitlab](https://about.gitlab.com/)的[gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/)标准，基于Gitflow实现自动化测试，编译，打包，发布等流程。

#### 需求确认
在Gitflow流程中，我们讨论了不同分支的适用场景，所以我们也希望Gitlab可以基于不同的分支，按照Gitlab的规范，执行相应的操作。
因此，我们对Gitlab提出了以下几点需求：
- 能识别我们提交的分支
- 对公司现有的一些组件，如SonarQube，Nexus等可以做到无缝衔接
- 支持灵活的添加与更新操作
- 做到一切皆代码，便于维护与管理
- 

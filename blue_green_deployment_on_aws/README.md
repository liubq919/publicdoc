# AWS的蓝/绿部署

## 目录
- [摘要](abstract.md)
- [介绍](introduction.md)
  * 蓝绿部署方法论
  * 蓝/绿的优点
  * 定义环境边界
- [AWS中保障蓝/绿部署的工具与服务](AWS_tools_and_services_enabling_blue_green_deployments.md)
  * Amazon Route 53
  * Elastic Load Balancing
  * Auto Scaling
  * AWS Elastic Beanstalk
  * AWS OpsWorks
  * AWS CloudFormation
  * Amazon CloudWatch
- [方案](techniques.md)
  * 使用Amazon Route 53更新DNS路由
  * 在弹性负载均衡器下游交换Auto Scaling组
  * 更新Auto Scaling组启动配置
  * 交换Elastic Beanstalk应用环境
  * 在AWS OpsWorks中克隆堆栈并更新DNS
- [数据同步管理和schema更改的最佳实践](best_practices_for_managing_data_sync_and_schema_changes.md)
  * schema更改与代码更改间的解耦
- [何时不推荐蓝/绿部署](when_blue_green_are_not_recommended.md)
- [总结](conclusion.md)
- [贡献者](contributors.md)
- [附录](appendix.md)

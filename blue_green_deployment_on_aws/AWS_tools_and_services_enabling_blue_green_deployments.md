## AWS中保障蓝/绿部署的工具与服务
AWS提供了许多工具和服务，可帮助你通过AWS API自动化和简化部署和基础架构，可以使用Web控制台，[CLI工具，SDK和IDE](https://aws.amazon.com/tools/)来利用这些API。由于AWS生态系统中提供了许多服务，因此以下内容并非完整列表。相反，此列表仅提供我们在本文中将讨论的服务的概览。你可以在AWS之外找到软件解决方案来帮助自动化和监控基础架构和部署，但本文主要关注AWS服务。

### Amazon Route 53
[Amazon Route 53](https://aws.amazon.com/route53/)是一种高可用且可扩展的权威DNS服务，可将用户对基于Internet资源的请求路由到适当的目标。Amazon Route 53运行在一个由DNS服务器组成的全球网络上，为客户提供了附加的功能，比如基于健康检查、地理位置和延迟的路由。DNS是蓝/绿部署的经典方法，允许管理员通过简单地更新托管区域中的DNS记录来引导流量。此外，存活时间（TTL）可以调整资源记录; 这对于有效的DNS模式很重要，因为较短的TTL允许更改的记录更快地传播到客户端。

### Elastic Load Balancing
为蓝/绿部署路由流量的另一种常见方法是使用负载均衡技术。[Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/)在指定的[Amazon Elastic Compute Cloud（Amazon EC2）](https://aws.amazon.com/ec2/)实例之间分配进入的应用流量。Elastic Load Balancing基于传入的请求进行伸缩，针对Amazon EC2资源执行健康检查，并自然地与其他AWS工具集成，例如Auto Scaling。对于希望提高应用容错性的客户来说，这是一个很好的选择。

### Auto Scaling
[Auto Scaling](https://aws.amazon.com/autoscaling/)有助于维护应用可用性，并允许客户根据定义的条件自动伸缩EC2容量。在Auto Scaling组中用于启动EC2实例的模板被称为*启动配置*。可以将不同版本的启动配置附加到Auto Scaling组以启用蓝/绿部署。还可以配置Auto Scaling以与Elastic Load Balancing负载均衡器配合使用。在此配置中，Elastic Load Balancing会平衡Auto Scaling组中运行的EC2实例的流量。可以在Auto Scaling组中定义终止策略，以确定在伸缩操作期间要删除的EC2实例。如“[Auto Scaling开发人员指南](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-enter-exit-standby.html)”中所述，Auto Scaling还允许将实例置于待机状态，而不是终止，这有助于在需要时快速回滚。Auto Scaling的终止策略和待机状态都支持蓝/绿部署。

### AWS Elastic Beanstalk
[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)是一种在AWS上启动和运行应用的快速而简单的方法。对于希望部署代码而不必担心管理底层基础设施的开发人员来说，它是完美的。Elastic Beanstalk支持Auto Scaling和Elastic Load Balancing，两者都支持蓝/绿部署。Elastic Beanstalk可以轻松运行多个版本的应用，并提供交换环境URL的功能，从而促进蓝/绿部署。

### AWS OpsWorks
[AWS OpsWorks](https://aws.amazon.com/opsworks/)是一种基于Chef的配置管理服务，允许用户在AWS上部署和管理应用堆。用户可以指定资源和应用配置，以及部署和监视运行资源。在准备蓝/绿环境时，OpsWorks简化了克隆整个堆栈的过程。

### AWS CloudFormation
[AWS CloudFormation](https://aws.amazon.com/cloudformation/)使客户能够通过JSON格式的模板描述他们所需的AWS资源。此服务提供非常强大的自动化功能，用于配置蓝/绿环境并促进交换流量的更新，无论是通过Route 53 DNS，还是Elastic Load Balancing等。服务可以作为更庞大的基础架构中的一部分作为代码策略使用，该基础架构使用代码和软件开发技术(如版本控制和持续集成)，类似于处理应用代码的方式以分配和管理。

### Amazon CloudWatch
[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)是AWS云资源和运行在AWS上的应用的监控服务。CloudWatch可以收集和跟踪指标，收集和监控日志文件以及设置警报。它提供了系统范围内对资源利用率，应用性能和运行状况的可视性，这对于蓝/绿部署中的应用运行状况的早期检测至关重要。
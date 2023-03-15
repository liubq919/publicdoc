<p align="center">
   <img width="200" src="avatar.png">
</p>

## 说说我理解的DevOps

过去的五年，在国内，DevOps可能第一次被这么重视起来，很大一部分因为云原生在国内的普及，甚至可以说是因为kubernetes被大规模采用。  
当我们提到DevOps，很多朋友脑海里想到的是自动化，ansible，容器，kubernetes，云，云原生等等，这些只能说是DevOps实践道路上的工具（器），远远没有达到组织架构与文化层（道）。  
在14年，很荣幸在一家优秀的外资企业开始了自己的第一份DevOps工作。多次在公司重要业务节点上，团队都很好的完成了任务要求，期间的功能迭代，高效沟通以及服务保障性，优秀的组织管理与高效的协作方式使我深刻的认识到，DevOps体系想要落地，绝不仅仅靠的是那些所谓的工具。  
同样，在这几年，DevOps在国内被普及的过程中，出现了各种其他的概念，如GitOps，DevSecOps，AiOps，FinOps等等，我也想跟大家聊聊我对这些概念的理解。  

### 历史
让我们回到过去看看DevOps的由来。

事件

    2007 年，Patrick参与了比利时一个政府下属部门的大型数据中心迁移的项目。
    在这个项目中，他负责测试和验证工作。所以他不光要和开发团队（Dev）一起工作，也要和运维团队（Ops）一起工作。
    他第一天在开发团队跟随敏捷的节奏，第二天又要以传统的方式像消防队员那样维护这些系统，
    这种在两种工作氛围的切换令他十分沮丧。
    
    他意识到开发团队和运维团队的工作方式和思维方式有巨大的差异：
    开发团队和运维团队生活在两个不同的世界，而彼此又坚守着各自的利益，所以在这两者之间工作到处都是冲突。
    作为一个敏捷的簇拥者，他渐渐的明白如何在这种状况下改进自己的工作。

术语的由来
    
    2009年10月
    Patrick 就想通过 Twitter 召集开发工程师和运维工程师在比利时举办一个类似于 Velocity 的大会。
    但如果要召开一个会议，就得有一个名字。Patrick 首先就想到了 Dev 和 Ops，
    由于这个会议会持续两天，所以他加上了 Days，于是就有了 DevOpsDays。
    由于 Twitter 140个字符的限制，大家在 Twitter 上去掉了 DevOps 中的 Days，保留了 DevOps。
    于是， DevOps 这个称谓正式诞生。

从上述的历史回顾中，我们可以看出以下几点：

- 开发团队（Dev）与运维团队（Ops）有着共同的目标，但两者却有不同的节奏与管理体系，因此为后续的管理冲突埋下了伏笔
- DevOps字面上代表的是开发与运维团队，但从广义上来讲，DevOps包含了对公司业务负责的所有团队或部门

### 行业优秀公司的定义
在这里，我们选取了AWS（Amazon Web Service）与Azure对DevOps的定义，仅供参考。

#### [AWS（Amazon Web Service）](https://aws.amazon.com/devops/what-is-devops/?nc1=h_ls)
    
    DevOps is the combination of cultural philosophies, practices, and tools that 
    increases an organization’s ability to deliver applications and services at high velocity: 
    evolving and improving products at a faster pace than 
    organizations using traditional software development and infrastructure management processes. 
    This speed enables organizations to better serve their customers and compete more effectively in the market.

    DevOps 集文化理念、实践和工具于一身，可以提高组织高速交付应用程序和服务的能力，与使用传统软件开发和基础设施管理流程相比，
    能够帮助组织更快地发展和改进产品。这种速度使组织能够更好地服务其客户，并在市场上更高效地参与竞争。

#### [Azure](https://azure.microsoft.com/zh-cn/resources/cloud-computing-dictionary/what-is-devops/)
    A compound of development (Dev) and operations (Ops), 
    DevOps is the union of people, process, and technology to continually provide value( of product) to customers.(P3)

    DevOps 是开发 (Dev) 和运营 (Ops) 的复合词，它将人、流程和技术结合起来，不断地为客户提供价值。


    What does DevOps mean for teams? 
    DevOps enables formerly siloed roles—development, IT operations, quality engineering, and security—to coordinate and collaborate to produce better, more reliable products. 
    By adopting a DevOps culture along with DevOps practices and tools, 
    teams gain the ability to better respond to customer needs, increase confidence in the applications they build, and achieve business goals faster.

    DevOps 对团队意味着什么？ 
    DevOps 使以前孤立的角色（开发、IT 运营、质量工程和安全）可以协调和协作，以生产更好、更可靠的产品。
    通过采用 DevOps 文化、做法和工具，团队能够更好地响应客户需求，增强对所构建应用程序的信心，更快地实现业务目标。

我们从上述两大公司的定义中可以总结出以下几点：

- DevOps的核心思想，也就是最终目标是成就客户，服务好客户
- DevOps的成功落地，需要工具，实践与文化的一整套体系

### 实践

下面我们想通过传统公司与采用DevOps体系公司两者间的案例做下比较，使大家直观的感受两者间的差别。

#### 传统公司
在二十年以前，无论是服务访问量还是服务上线节奏，都是远远低于当下，服务器可能只有几百台甚至几十台，上线频率一般都是按月来算，一个月可能才一个版本。因此在这种情况下，大家通过流程审批一次发布或者一次更新。  
以一次简单的上线为例，传统公司的部门组织方式：
<div align="center">
<img src=images/tradition-dep.jpg width="80%"/>
</div>

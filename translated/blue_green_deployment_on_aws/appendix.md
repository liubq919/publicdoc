## 附录
### 蓝/绿部署方案对比
下表提供了本文中讨论的不同蓝/绿部署技术的概述和比较。潜在风险从理想的低风险（![circle](black_circle.png)）到不太理想的高风险（![circle](black_circle.png) ![circle](black_circle.png) ![circle](black_circle.png)）进行评估。

<table>
    <thead>
        <tr>
            <th rowspan="3">方案</th>
            <th>风险类别</th>
            <th>潜在风险</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="6">使用Amazon Route 53更新DNS路由</td>
            <td>应用问题</td>
            <td><img src="black_circle.png"/></td>
            <td>促进金丝雀分析</td>
        </tr>
        <tr>
            <td>应用性能</td>
            <td><img src="black_circle.png"/></td>
            <td>逐步切换，流量分流管理</td>
        </tr>
        <tr>
            <td>人为/流程错误</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>依赖于自动化框架，整体流程简单</td>
        </tr>
        <tr>
            <td>基础设施故障</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>依赖于自动化框架</td>
        </tr>
        <tr>
            <td>回滚</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>DNS TTL复杂性（反应时间，flip/flop）</td>
        </tr>
        <tr>
            <td>成本</td>
            <td><img src="black_circle.png"></td>
            <td>通过Auto Scaling优化</td>
        </tr>
        <tr>
            <td rowspan="6">在弹性负载均衡器下游交换Auto Scaling组</td>
            <td>应用问题</td>
            <td><img src="black_circle.png"/></td>
            <td>促进金丝雀分析</td>
        </tr>
        <tr>
            <td>应用性能</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>较低细粒度流量分割管理，已预热的负载均衡器</td>
        </tr>
        <tr>
            <td>人为/流程错误</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>依赖于自动化框架</td>
        </tr>
        <tr>
            <td>基础设施故障</td>
            <td><img src="black_circle.png"/></td>
            <td>Auto Scaling</td>
        </tr>
        <tr>
            <td>回滚</td>
            <td><img src="black_circle.png"/></td>
            <td>没有DNS的复杂性</td>
        </tr>
        <tr>
            <td>成本</td>
            <td><img src="black_circle.png"></td>
            <td>通过Auto Scaling优化</td>
        </tr>
        <tr>
            <td rowspan="6">更新Auto Scaling组启动配置</td>
            <td>应用问题</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>在不均匀设备中的错误/问题检查比较复杂</td>
        </tr>
        <tr>
            <td>应用性能</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>较低细粒度流量分割管理，初始流量负载</td>
        </tr>
        <tr>
            <td>人为/流程错误</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>依赖于自动化框架</td>
        </tr>
        <tr>
            <td>基础设施故障</td>
            <td><img src="black_circle.png"/></td>
            <td>Auto Scaling</td>
        </tr>
        <tr>
            <td>回滚</td>
            <td><img src="black_circle.png"/></td>
            <td>没有DNS的复杂性</td>
        </tr>
        <tr>
            <td>成本</td>
            <td><img src="black_circle.png"> <img src="black_circle.png"/></td>
            <td>通过Auto Scaling进行优化，但初始扩展超出需求</td>
        </tr>
        <tr>
            <td rowspan="6">交换Elastic Beanstalk应用环境</td>
            <td>应用问题</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> </td>
            <td>能够在切换之前进行金丝雀分析，但不能用于生产流量</td>
        </tr>
        <tr>
            <td>应用性能</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>完整切换</td>
        </tr>
        <tr>
            <td>人为/流程错误</td>
            <td><img src="black_circle.png"/></td>
            <td>流程简单，自动化</td>
        </tr>
        <tr>
            <td>基础设施故障</td>
            <td><img src="black_circle.png"/></td>
            <td>Auto Scaling, CloudWatch monitoring, Elastic Beanstalk健康报告</td>
        </tr>
        <tr>
            <td>回滚</td>
            <td><img src="black_circle.png"/> <img src="black_circle.png"/> <img src="black_circle.png"/></td>
            <td>DNS TTL复杂性</td>
        </tr>
        <tr>
            <td>成本</td>
            <td><img src="black_circle.png"> <img src="black_circle.png"/></td>
            <td>通过Auto Scaling进行优化，但初始扩展超出需求</td>
        </tr>
        <tr>
            <td rowspan="6">在AWS OpsWorks中克隆堆栈并更新DNS</td>
            <td>应用问题</td>
            <td><img src="black_circle.png"/></td>
            <td>促进金丝雀分析</td>
        </tr>
        <tr>
            <td>应用性能</td>
            <td><img src="black_circle.png"/></td>
            <td>逐步切换，流量分流管理</td>
        </tr>
        <tr>
            <td>人为/流程错误</td>
            <td><img src="black_circle.png"></td>
            <td>高度自动化</td>
        </tr>
        <tr>
            <td>基础设施故障</td>
            <td><img src="black_circle.png"></td>
            <td>Auto-healing（自愈）能力</td>
        </tr>
        <tr>
            <td>回滚</td>
            <td><img src="black_circle.png"> <img src="black_circle.png"> <img src="black_circle.png"></td>
            <td>DNS TTL复杂性</td>
        </tr>
        <tr>
            <td>成本</td>
            <td><img src="black_circle.png"> <img src="black_circle.png"> <img src="black_circle.png"></td>
            <td>双重资源栈</td>
        </tr>
    </tbody>
</table>
#### 测试

花时间开发代码的全面测试是确保无服务器体系结构中的质量的最佳方法。但是，无服务器架构将执行适当的单元测试实践，可能比你习惯的更多。许多开发人员使用单元测试工具和框架来编写测试，从而使他们的代码也测试其依赖。这是一个结合了单元测试和集成测试的单一测试，但是这两种测试完成地都不是很好。

将所有单元测试用例范围缩小到单个逻辑函数中的单个代码路径，模拟来自上游的所有输入和来自下游的输出，这一点非常重要。这允许将测试用例仅与你拥有的代码隔离。在编写单元测试时，基于代码与api、库等与之间的契约，你可以也应该假设依赖能正确工作。

对于集成测试来说，在模拟现实环境的环境中测试代码与其依赖的集成也同样重要。在开发人员笔记本或者构建服务器上测试与下游依赖的集成，并不能完全测试代码是否能够在现实环境中成功集成一次。这在Lambda环境中尤其如此，在这个环境中，代码没有将由事件源传递的事件的所有权，并且你无法在Lambda之外创建Lambda运行时环境。

###### 单元测试

根据我们之前的说法，我们建议你对Lambda函数代码进行彻底的单元测试，主要关注处理程序函数之外的业务逻辑。还应该单元测试解析函数事件源的样本/模拟对象的能力。但是，大部分逻辑和测试应该使用在代码中你能完全控制的模拟对象和函数。如果觉得处理程序函数中有一些重要的东西需要进行单元测试，那么这可能是你应该在处理程序函数中进一步封装和具体化逻辑的标志。另外，为了补充你编写的单元测试，应该使用AWS SAM Local创建本地测试自动化，它可以作为函数代码的本地端到端测试(注意，这不是单元测试的替代品)。

###### 集成测试

对于集成测试，我们建议你创建Lambda函数的较低生命周期版本，在这个版本中，通过CI/CD管道可以触发和检查的示例事件部署和调用代码包（实现取决于应用和架构）。

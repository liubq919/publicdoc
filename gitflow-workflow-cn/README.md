## Gitflow 工作流

Gitflow工作流是一种Git工作流设计，最初是由[nive的Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)发布并流行的。Gitflow工作流定义了一个围绕项目发布的严格分支模型。这为管理大型项目提供了一个健壮的框架。

Gitflow非常适合具有发布计划周期的项目。此工作流程不会添加超出[Feature Branch 工作流](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)所需的任何新概念或命令。相反，它将非常特定的角色分配给不同的分支，并定义它们应该如何以及何时进行交互。除了feature分支之外，它还使用单独的分支来准备、维护和记录发布。当然，你还可以利用Feature Branch 工作流的所有优点:拉请求、独立实验和更有效的协作。

#### 开始
Gitflow实际上只是一个Git工作流的抽象概念。这意味着它决定了要创建的分支类型以及如何将它们合并在一起。下面我们将讨论这些分支的用途。git-flow工具集是一个具有安装过程的实际命令行工具。git-flow的安装过程很简单。git-flow的软件包可在多个操作系统上使用。在OSX系统上，您可以执行brew install git-flow。在Windows上，您需要[下载并安装git-flow](https://git-scm.com/download/win)。安装git-flow后，可以通过执行git flow init以在项目中使用它。Git-flow是Git的包装器。git flow init命令是默认[git init](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init)命令的扩展，除了创建分支之外，不会更改仓库中的任何内容。

#### 原理

![1](images/image1.svg)）

##### Develop与Master分支

此工作流使用两个分支来记录项目的历史记录，而不是单个*master*分支。*master*分支存储正式发布历史，而*develop*分支作为*feature*的集成分支。使用版本号标记*master*分支中的所有提交也很方便。

第一步是使用*develop*分支补充默认*master*。一种简单的方法是让一个开发人员在本地创建一个空的*develop*分支并将其推送到服务器：
```shell
git branch develop
git push -u origin develop
```

该分支将包含项目的完整历史记录，而master将包含删节版本。其他开发人员现在应该克隆中央存储库并为*develop*创建跟踪分支。

使用*git-flow*扩展库时，在现有*repo*上执行*git flow init*将创建*develop*分支：
```shell
$ git flow init
Initialized empty Git repository in ~/project/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []

$ git branch
* develop
 master
```

#### Feature分支

每个新功能都应驻留在自己的分支中，可以将其推送到[中央存储库](https://www.atlassian.com/git/tutorials/syncing/git-push)以进行备份/协作。但是，*feature*不基于*master*来创建分支，而是将*develop*作为其父分支。 *feature*完成后，它将合并回*develop*。*Feature*不应该直接与*master*交互。

![2](images/image2.svg)）

注意，结合*develop*分支的*feature*分支实际上是Feature Branch工作流。但是，Gitflow工作流并不止于此。

通常*feature*分支的创建基于最新的*develop*分支。

##### 创建feature分支
没有git-flow扩展：
```shell
git checkout develop
git checkout -b feature_branch
```
使用git-flow拓展：
```shell
git flow feature start feature_branch
```

继续工作，像往常一样使用Git。

##### 完成一个feature分支

完成*feature*的开发工作后，下一步是将*feature*分支合并到*develop*中。
没有git-flow扩展：
```shell
git checkout develop
git merge feature_branch
```
使用git-flow拓展：
```shell
git flow feature finish feature_branch
```

#### Release分支

![3](images/image3.svg)）

一旦*develop*分支为发布获得了足够多的功能（或预定的发布日期即将到来），就可以从*develop*分支派生一个*release*分支。创建此分支将启动下一个发布周期，因此在此之后不能添加任何新特性，只能有错误修复，文档生成和其他以发布为中心的任务才能进入该分支。一旦准备好发布，*release*分支将合并到*master*分支并标记版本号。 此外，它应该合并回到*develop*分支中，自发布以来可能（develop分支）已经取得了进展。

使用专门的分支来准备发行版，使得一个团队可以完善当前的发行版，而另一个团队可以继续为下一个发行版开发特性。它还创建了定义良好的开发阶段(例如，很容易说，这周我们将准备4.0版本，并在仓库结构中实际看到它)。

生成*release*分支是另一个简单的分支操作。与*feature*分支一样，*release*分支基于*develop*分支。可以使用以下方法创建一个新的*release*分支。

没有git-flow扩展：

```shell
git checkout develop
git checkout -b release/0.1.0
```
使用git-flow拓展：
```shell
$ git flow release start 0.1.0
Switched to a new branch 'release/0.1.0'
```

一旦*release*分支准备好发布，它将被合并到*master*和*develop*中，然后*release*分支将被删除。将它们合并回*develop*中是很重要的，因为重要的更新可能已经添加到*release*分支中，并且它们需要能够被新特性访问。如果你的组织强调代码审查，那么这将是一个理想的pull请求的地方。

要完成*release*分支，请使用以下方法：

没有git-flow扩展：
```shell
git checkout develop
git merge release/0.1.0
```

使用git-flow拓展：
```shell
git checkout master
git checkout merge release/0.1.0
git flow release finish '0.1.0'
```

#### Hotfix分支
![3](images/image4.svg)）

维护或*hotfix*分支用于快速修补生产版本。*hotfix*分支很像*release*分支和*feature*分支，只不过它们是基于*master*而不是*develop*。这是唯一应该直接从*master*上fork的分支。一旦修复完成，就应该将其合并到*master*和*develop*(或当前*release*分支)中，并且应该用更新的版本号标记*master*分支。

拥有专门的bug修复开发线可以让团队在不中断工作流中其余部分或无需等待下一个发布周期的情况下解决问题。你可以将维护分支看作是直接与*master*一起工作的临时*release*分支。可以使用以下方法创建*hotfix*分支：

没有git-flow扩展：
```shell
git checkout master
git checkout -b hotfix_branch
```

使用git-flow拓展：
```shell
$ git flow hotfix start hotfix_branch
```

与完成*release*分支类似，*hotfix*分支将合并到*master*分支和*release*分支中。
没有git-flow扩展：
```shell
git checkout master
git merge hotfix_branch
git checkout develop
git merge hotfix_branch
git branch -D hotfix_branch
```

使用git-flow拓展：
```shell
$ git flow hotfix finish hotfix_branch
```

#### 总结

这里我们讨论了Gitflow工作流。Gitflow是你和你的团队可以使用的众多Git工作流样式之一。

了解Gitflow的一些关键要点是：
- 工作流对于基于发布的软件工作流来说是非常好的
- Gitflow为生产环境提供了一个专用的热修复通道

Gitflow的总体流程是：
- 从*master*创建一个*develop*分支
- 从*develop*创建一个*release*分支
- *feature*分支基于*develop*分支创建
- *feature*分支完成后，它将合并到*develop*分支
- 当*release*分支完成时，它被合并到*develop*和*master*中
- 如果*master*中检测到问题，则从*master*创建一个*hotfix*分支
- *hotfix*分支完成后，就会将其合并到*develop*和*master*中


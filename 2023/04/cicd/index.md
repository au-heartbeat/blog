# CI/CD


## 使用 Git Action 实现CI/CD
有关于CI/CD的概念，想必大家都非常熟悉。今天跟大家分享这篇文章，主要是想分享记录一下，在HeartBeat内部项目中，我们怎么实现代码的CI/CD的。我主要从以下几方面来跟大家分享。
1. CI/CD的简单概述
2. 部署CI/CD的工具
3. Git Action以及Why use it?
4. How to use Git Action?
### CI/CD
CI指持续集成（Continuous Integration）
CD指持续交付/持续部署（Continuous Delivery/Continuous Deployment）的缩写。
持续集成是指在开发过程中，团队成员经常地提交代码到一个共享代码仓库中，然后自动地进行构建、测试、代码质量检查等流程，以尽早地发现和解决潜在的问题。
持续交付/持续部署则是指将已经通过了所有自动化测试的代码，自动地部署到生产环境中，以提高软件交付的速度和质量，并且可以快速地响应客户需求和反馈。

### 部署CI/CD的工具
部署 CI/CD 的工具有很多，下面我就简单列举一些主流的工具及其优缺点.

| 工具名 | 优点 | 缺点 |
| ------ | ------ | ------ |
| Jenkins | 开源、免费、功能强大、插件丰富，可与各种工具集成，支持多个操作系统。 | 需要自己维护和扩展。 |
| Travis CI | 适用于开源项目，易于设置和使用，集成度高，支持多种编程语言和框架。 | 需要付费才能获得更高级别的功能和支持。 |
| CircleCI | 基于云服务，易于设置和使用，与多个工具集成，支持多个操作系统和编程语言。 | 需要付费才能获得更高级别的功能和支持。 |
| GitLab CI/CD | GitLab 自带的 CI/CD 工具，与 GitLab 无缝集成，易于设置和使用，支持多种编程语言和框架，可自定义 CI/CD 流程。 | 可能需要额外的硬件资源来支持 CI/CD 流程。 |
| Bamboo | 易于设置和使用，支持多种操作系统和编程语言，与 JIRA 等 Atlassian 产品集成，可扩展性强。 | 需要付费才能获得更高级别的功能和支持。 |
| GitLab CI/CD | GitLab 自带的 CI/CD 工具，与 GitLab 无缝集成，易于设置和使用，支持多种编程语言和框架，可自定义 CI/CD 流程。 | 可能需要付费才能获得更高级别的功能和支持。 |
### Git Action
Git Actions是一个持续集成和持续部署（CI/CD）平台，由GitHub提供。使用Git Actions，您可以在代码存储库中配置和自动化软件开发生命周期中的工作流程，包括测试、构建和部署。
HeartBeat是我司的内部项目，其是一种了解项目交付情况的工具，可帮助团队确定绩效指标，从而推动持续改进并提高团队生产力和效率。因此团队还不是完全获取一些付费资源的支持。基于此，我们选择Git Action部署项目CI/CD，并且免费的资源完全支持项目当前运行。
### 如何使用Git Action部署CI/CD
1. 在你的GitHub上的存储库中创建一个目录.github/workflows
2. 在该.github/workflows目录中，创建一个名为buildAndDeploy.yml的文件
3. 将当前内容commit并push至仓库，创建pull request。工作流提交到存储库中的分支会触发事件push并运行工作流。 至此，一个简单的CI/CD就部署成功啦。

那如何快速查看工作流程结果呢？ 这里将以HeartBeat CI/CD部署为例，查看工作流程结果。
  - 进入GitHub仓库主页，单击 Actions
  ![alt 属性文本](GitActionImages/GitActionIcon.png "GitActionIcon")
  - 在左侧边栏中，单击要显示的工作流.
  ![alt 属性文本](GitActionImages/GitActionJobs.png "GitActionJobs")
  - 从工作流程运行列表中，单击要查看的运行名称，以为“frontend checked举例，正在测试 GitHub Actions”。
  ![alt 属性文本](GitActionImages/DetailSteps.png "DetailSteps")
  - 日志显示每个步骤是如何处理的。展开任何步骤以查看其详细信息。
   ![alt 属性文本](GitActionImages/DetailInfoAboutEachStep.png "DetailInfoAboutEachStep")
  - 以下是HeartBeat完整CI/CD workFlows
    ![alt 属性文本](GitActionImages/BuildAndDeploy.png "BuildAndDeploy")
Deadline 2023-05-10



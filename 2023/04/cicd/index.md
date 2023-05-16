# CI/CD


## 使用 GitHub Actions 实现Heartbeat CI/CD
有关于CI/CD的概念，想必大家都非常熟悉。今天跟大家分享这篇文章，主要是想分享记录一下，在HeartBeat内部项目中，我们怎么实现代码的CI/CD的。我主要从以下几方面来跟大家分享。
1. CI/CD的简单概述
2. 部署CI/CD的工具
3. GitHub Actions以及Why use it?
4. How to use GitHub Action?
### CI/CD
CI指持续集成（Continuous Integration）
CD指持续交付/持续部署（Continuous Delivery/Continuous Deployment）的缩写。
持续集成是指在开发过程中，团队成员经常地提交代码到一个共享代码仓库中，然后自动地进行构建、测试、代码质量检查等流程，以尽早地发现和解决潜在的问题。
持续交付/持续部署则是指将已经通过了所有自动化测试的代码，自动地部署到生产环境中，以提高软件交付的速度和质量，并且可以快速地响应客户需求和反馈。

### 部署CI/CD的工具
部署 CI/CD 的工具有很多，下面我就简单列举一些主流的工具及其优缺点.

| 工具名 | 优点 | 缺点 |
| ------ | ------ | ------ |
| Jenkins | 开源、免费、功能强大、插件丰富，可与各种工具集成，支持多个操作系统。 | 需要自己维护和扩展。|
| Travis CI | 适用于开源项目，易于设置和使用，集成度高，支持多种编程语言和框架。 | 需要付费才能获得更高级别的功能和支持。|
| GitHub Actions | 与GitHub 版本控制系统无缝集成，可以轻松配置和触发 CI/CD 流程 | 如果需要更高的并发能力，则需要升级到付费版本；相比一些专业的 CI/CD 工具，GitHub Actions 在某些高级功能和自定义配置方面可能受到一定的限制。|
| BuildKite |简单易用，可扩展性，可视化界面 | 需要自行设置和维护服务器资源，需要编写配置文件，可能会受限于插件和集成的可用性。|
### GitHub Actions
GitHub Actions是一个持续集成和持续部署（CI/CD）平台，由GitHub提供。使用GitHub Actions，可以在代码存储库中配置和自动化软件开发生命周期中的工作流程，包括测试、构建和部署。同时它与GitHub无缝集成：GitHub Actions与GitHub代码仓库紧密集成，可以直接在代码仓库中编写和管理工作流程，方便使用和管理。
HeartBeat是我司的内部项目，其是一种帮助团队了解项目交付情况的度量工具，包括可以计算团队一段时间内的代码部署频率，需求卡的完成情况等，通过分析这些指标并作出相应的改进措施从而推动并提高团队生产力和效率。但团队还没获取一些付费资源的支持。基于此，我们选择GitHub Actions部署项目CI/CD，并且免费的资源完全支持项目当前运行。
### HeartBeat code structure
HeartBeat 项目中的所有代码在一个总的Heartbeat目录下,其中包括了前端代码，后端代码，stub(mock server)等。
而.github目录下主要存了GitHub actions的流水线配置信息;docs目录存的主要是Heartbeat的官网信息文档,且不断在更新当中;ops下存了与基础设施相关的配置信息，包括管理AWS资源的cloudformation.yml文件,还有前端，后端，mock server(stub)的Dockerfile。讲项目结构的原因是我们的流水线的各个job是与其紧密相连的。
- Heartbeat
    - .github
        - workflows
           - BuildAndDeploy.yml
           - Docs.yml
           - Welcome.yml
    - backend
        - gradle
        - src
           - main
               - java
                   - client
                   - config
                   - controller
                   - exception
                   - service
           - test
    - docs
        - src
            - components
            - layouts
            - pages
    - frontend
        - test
        - cypress
        - src
            - assets
            - clients
            - components
            - hooks
            - layouts
            - pages
            - utils
    - ops
      - infra
        - cloudformation.yml
        - docker-compose.yml
        - Dockerfile.backend
        - Dockerfile.frontend
        - Dockerfile.stub
    - stubs
        - backend
          - buildkite
          - github
          - jira
        - frontend
            - config
                - board.json
                - pipeline.json
                - sourceControl.json
            - exportPage

### 如何使用GitHub Actions部署CI/CD
1. 在你的GitHub上的存储库中创建一个目录.github/workflows
2. 在该.github/workflows目录中，创建一个名为buildAndDeploy.yml的文件,该文件就是设计组织pipeline结构的主要文件。其主干代码如下：
其中jobs下的每一项都是我们pipeline运行过程的一个步骤。
``` yaml
name: Build and Deploy

on:
  push:
    branches: [ "*" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  shellcheck:
  
  security-check:
  
  backend-check:
  
  backend-license-check:
  
  frontend-check:
  
  frontend-license-check:

  deploy-infra:
    if: ${{ github.ref == 'refs/heads/main' }}
    needs:
      - frontend-check
      - backend-check
      - security-check
      - shellcheck
      - frontend-license-check
      - backend-license-check

  build-backend:
    if: ${{ github.ref == 'refs/heads/main' }}
    needs:
      - deploy-infra

  build-frontend:
    if: ${{ github.ref == 'refs/heads/main' }}
    needs:
      - deploy-infra

  build-mock-server:
    if: ${{ github.ref == 'refs/heads/main' }}
    needs:
      - deploy-infra

  deploy-e2e:
    runs-on: ubuntu-latest
    needs:
      - build-backend
      - build-frontend

  deploy-stub:
    runs-on: ubuntu-latest
    needs:
      - build-mock-server
      
  e2e:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./frontend
    needs:
      - deploy-e2e
      - deploy-stub

  deploy:
    runs-on: ubuntu-latest
    needs:
      - e2e

```
用该pipeline跑完的流水线如下图所示
![alt 属性文本](./GitActionImages/BuildAndDeploy.png "BuildAndDeploy")
3. 触发流水线  
那流水线的触发条件是什么呢？主要有两种：1）feature branch的任意提交； 2）将feature branch上的pr合并到main分支；
第一种只会触发一些检查工作，包括代码安全性检查，许可证检查，shell脚本检查。不会触发前后端代码的构建镜像和部署。第二种情况，也就是当将pr
合并到main后才会触发整个流水线，包括镜像构建和最终部署到AWS EC2。
``` yaml
name: Build and Deploy

on:
  push:
    branches: [ "*" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:
```
而只在main分支有pr合并时才触发相应job的条件就是buildAndDeploy.yml代码中的
```yaml
 if: ${{ github.ref == 'refs/heads/main' }}
```
4. 不同条件下触发的job不同.[stub] [infra] [docs]  
跑一遍流水线是需要花费一定的时间的,有的时候，流水线中不是所有job都应该跑的，比如，当我们没有对mock server(stub)做任何改变的时候，可以不触发它；
还有对于infra相关的配置，如果没有任何更改，也不需要触发，这样就可以节省流水线总的运行时间，那如何控制呢，其实很简单，就是在对应的job里加一个
条件判断.
```yaml
- name: Build, tag for MockServer
        if: ${{ contains(github.event.head_commit.message, '[stub]') }}
        env:
          REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          REPOSITORY: heartbeat_stub
          IMAGE_TAG: "hb${{ github.run_number }}"
        run: |
          docker build -t $REGISTRY/$REPOSITORY:latest ./ -f ./ops/infra/Dockerfile.stub
          docker build -t $REGISTRY/$REPOSITORY:$IMAGE_TAG ./ -f ./ops/infra/Dockerfile.stub
```
上述代码是build-mock-server job中的一个构建mock server镜像的step,其中有行代码是`if: ${{ contains(github.event.head_commit.message, '[stub]') }}`
这行代码的作用就是只有当触发流水线的最新一条提交信息里有[stub]标记时，才会跑这个step。所以当我们对stub文件下的内容进行了更改后，我们应该在提交信息里加[stub]标记，这样才会触发mock server的构建和部署，否则就不会触发。
5. 不同jobs里边的依赖关系
流水线的各个job肯定是有依赖关系的，比如部署一定依赖于镜像构建，这个其实也很好实现。比如下面代码中deploy-infra是要依赖之前的6个job的，这在我们完整的流水线图中也可以体现。
```yaml
deploy-infra:
    if: ${{ github.ref == 'refs/heads/main' }}
    needs:
      - frontend-check
      - backend-check
      - security-check
      - shellcheck
      - frontend-license-check
      - backend-license-check
```
6. 至此，用GitHub Actions搭建的Heartbeat pipeline就成功啦。

那如何快速查看工作流程结果呢？ 这里将以HeartBeat CI/CD部署为例，查看工作流程结果。
  - 进入GitHub仓库主页，单击 Actions
  ![alt 属性文本](./GitActionImages/GitActionIcon.png "GitActionIcon")
  - 在左侧边栏中，单击要显示的工作流.
  ![alt 属性文本](./GitActionImages/GitActionJobs.png "GitActionJobs")
  - 从工作流程运行列表中，单击要查看的运行名称，以为“frontend checked举例，正在测试 GitHub Actions”。
  ![alt 属性文本](./GitActionImages/DetailSteps.png "DetailSteps")
  - 日志显示每个步骤是如何处理的。展开任何步骤以查看其详细信息。
   ![alt 属性文本](./GitActionImages/DetailInfoAboutEachStep.png "DetailInfoAboutEachStep")
  - 以下是HeartBeat完整CI/CD workFlows
    ![alt 属性文本](./GitActionImages/BuildAndDeploy.png "BuildAndDeploy")



---
layout:     post
title:      Gitlab+Runner持续集成
subtitle:   
date:       2019-06-11
author:     YQ
header-img: 
catalog: true
sidebar: true
tags:
    - docker
---

## 什么是GitLab CI/CD

根据[Gitlab官方文档](https://about.gitlab.com/product/continuous-integration/)介绍，gitlab内置了持续集成以及持续交付功能。

`持续集成`(Continuous Integration，简称CI)的工作是将团队中的代码集成到共享存储库中。开发人员在Merge (Pull)请求中共享他们的新代码，这会触发一个管道来构建、测试和验证新代码，然后再合并存储库中的更改。

`持续交付`(Continuous Delivery，或CD)向应用程序交付经过CI验证的代码。

CI和CD一起加速了您的团队为客户和涉众交付结果的速度。CI可以帮助您在开发周期的早期捕获和减少bug，并且CD可以更快地转移到您的应用程序中。

## CI/CD的优点

1. `集成`：GitLab CI/CD是GitLab的一部分，支持从计划到部署(以及以后)的单个对话
2. `开源`：CI/CD是开源GitLab社区版和专有GitLab企业版的一部分
3. `容易学习`：看我们的快速入门指南
4. `无缝`：一个GitLab应用程序的一部分，具有一个伟大的用户体验
5. `可伸缩`：测试分布在不同的机器上，您可以添加任意数量的测试
6. `更快的结果`：每个构建可以在多台机器上并行运行的多个作业中进行分割
7. `针对交付进行优化`：多个阶段、手动部署门、环境和变量


## GitLab-Runner与GitLab-CI的关系图

![GitLab-Runner&GitLab-CI](https://raw.githubusercontent.com/yangqi1789/yangqi1789.github.io/master/img/GitLab-Runner&GitLab-CI.jpg)

## GitLab-Runner的安装

1.Simply download one of the binaries for your system:

```bash
# Linux x86-64
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Linux x86
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-386

# Linux arm
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm

```

2.Give it permissions to execute:

```bash
sudo chmod +x /usr/local/bin/gitlab-runner
````

3.Optionally, if you want to use Docker, install Docker with:

```bash
curl -sSL https://get.docker.com/ | sh
```

4.Create a GitLab CI user:

```bash
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

5.Install and run as service:

```bash
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

6.[Register the Runner](https://docs.gitlab.com/runner/register/index.html)

# Istio

[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/1395/badge)](https://bestpractices.coreinfrastructure.org/projects/1395)
[![Go Report Card](https://goreportcard.com/badge/github.com/istio/istio)](https://goreportcard.com/report/github.com/istio/istio)
[![GoDoc](https://godoc.org/istio.io/istio?status.svg)](https://godoc.org/istio.io/istio)

<a href="https://istio.io/">
    <img src="https://github.com/istio/istio/raw/master/logo/istio-bluelogo-whitebackground-unframed.svg"
         alt="Istio logo" title="Istio" height="100" width="100" />
</a>

---

Istio 是一个开源的服务网格，它可以透明地分层到现有的分布式应用程序上。Istio 的强大功能提供了一种统一且更高效的方式来保护、连接和监控服务。Istio 是实现负载均衡、服务间认证和监控的途径 - 几乎不需要或完全不需要修改服务代码。

- 如需深入了解如何使用 Istio，请访问 [istio.io](https://istio.io)
- 如需提问和获取社区帮助，请访问 [Github Discussions](https://github.com/istio/istio/discussions)
- 如需了解如何参与我们的整体社区，请访问 [我们的社区页面](https://istio.io/about/community)

在本 README 中：

- [Istio](#istio)
  - [简介](#简介)
  - [代码仓库](#代码仓库)
  - [问题管理](#问题管理)

此外，这里还有一些您可能想要阅读的其他文档：

- [Istio 社区](https://github.com/istio/community#istio-community) - 描述如何参与和贡献 Istio 项目
- [Istio 开发者指南](https://github.com/istio/istio/wiki/Preparing-for-Development) - 解释如何设置和使用 Istio 开发环境
- [项目规范](https://github.com/istio/istio/wiki/Development-Conventions) - 描述我们在代码库中使用的规范
- [创建快速和精简的代码](https://github.com/istio/istio/wiki/Writing-Fast-and-Lean-Code) - 针对代码库的性能导向建议和指南

您可以在我们的 [Wiki](https://github.com/istio/istio/wiki) 上找到许多其他有用的文档。

## 简介

[Istio](https://istio.io/latest/docs/concepts/what-is-istio/) 是一个开放平台，用于提供统一的方式来[集成微服务](https://istio.io/latest/docs/examples/microservices-istio/)、管理[微服务间的流量](https://istio.io/latest/docs/concepts/traffic-management/)、执行策略和聚合遥测数据。Istio 的控制平面为底层集群管理平台（如 Kubernetes）提供了一个抽象层。

Istio 由以下组件组成：

- **Envoy** - 每个微服务的边车代理，用于处理集群内服务之间以及服务与外部服务之间的入站/出站流量。这些代理形成了一个_安全的微服务网格_，提供丰富的功能，如服务发现、丰富的 7 层路由、断路器、策略执行和遥测记录/报告功能。

  > 注意：服务网格不是覆盖网络。它简化和增强了应用程序中的微服务如何通过底层平台提供的网络进行通信。

- **Istiod** - Istio 控制平面。它提供服务发现、配置和证书管理。它由以下子组件组成：

    - **Pilot** - 负责在运行时配置代理。

    - **Citadel** - 负责证书的颁发和轮换。

    - **Galley** - 负责验证、接收、聚合、转换和分发 Istio 内的配置。

- **Operator** - 该组件为用户提供了友好的选项来操作 Istio 服务网格。

## 代码仓库

Istio 项目分为几个 GitHub 仓库：

- [istio/api](https://github.com/istio/api)。这个仓库定义了 Istio 平台的组件级 API 和通用配置格式。

- [istio/community](https://github.com/istio/community)。这个仓库包含有关 Istio 社区的信息，包括管理 Istio 开源项目的各种文档。

- [istio/istio](README.md)。这是主要的代码仓库。它托管 Istio 的核心组件、安装构件和示例程序。它包括：

    - [istioctl](istioctl/)。这个目录包含 [_istioctl_](https://istio.io/latest/docs/reference/commands/istioctl/) 命令行工具代码。

    - [pilot](pilot/)。这个目录包含平台特定的代码，用于填充[抽象服务模型](https://istio.io/docs/concepts/traffic-management/#pilot)，在应用程序拓扑发生变化时动态重新配置代理，以及将[路由规则](https://istio.io/latest/docs/reference/config/networking/)转换为代理特定配置。

    - [security](security/)。这个目录包含[安全](https://istio.io/latest/docs/concepts/security/)相关代码，包括 Citadel（作为证书颁发机构）、citadel agent 等。

- [istio/proxy](https://github.com/istio/proxy)。Istio 代理包含 [Envoy 代理](https://github.com/envoyproxy/envoy) 的扩展（以 Envoy 过滤器的形式），支持认证、授权和遥测收集。

- [istio/ztunnel](https://github.com/istio/ztunnel)。该仓库包含 Ambient mesh 的 ztunnel 组件的 Rust 实现。

- [istio/client-go](https://github.com/istio/client-go)。这个仓库定义了用于以编程方式与 Istio 资源交互的自动生成的 Kubernetes 客户端。

> [!NOTE]
> 只有 `istio/api` 和 `istio/client-go` 仓库暴露了稳定的接口，旨在作为库直接使用。

## 问题管理

我们使用 GitHub 来跟踪所有的错误和功能请求。我们跟踪的每个问题都有各种元数据：

- **Epic（史诗）**。一个 epic 代表 Istio 整体的一个功能领域。Epic 的范围相当广泛，基本上是产品级别的事情。每个问题最终都是 epic 的一部分。

- **Milestone（里程碑）**。每个问题都被分配一个里程碑。这是 0.1、0.2、...，或 'Nebulous Future'。里程碑表明我们认为问题应该在什么时候得到解决。

- **Priority（优先级）**。每个问题都有一个优先级，由 [Prioritization](https://github.com/orgs/istio/projects/6) 项目中的列表示。优先级可以是 P0、P1、P2 或 >P2。优先级表示在里程碑内解决问题的重要性。P0 表示如果问题未解决，就不能认为里程碑已经实现。

---

<div align="center">
    <img src="https://raw.githubusercontent.com/cncf/artwork/master/other/cncf/horizontal/color/cncf-color.svg" width="300" alt="Cloud Native Computing Foundation logo"/>
    <p>Istio 是一个 <a href="https://cncf.io">云原生计算基金会</a> 项目。</p>
</div>

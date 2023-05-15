---
title: "Mock Server and Stub Server"
date: 2023-04-24T09:37:52+08:00
draft: false
author: "Heartbeat"
authorLink: "https://au-heartbeat.github.io/blog/"
description: ""
license: <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/80x15.png" /></a>

tags: [""]
categories: [""]
featuredImage: "https://images.unsplash.com/photo-1671227498016-93aa927686f8?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1074&q=80"
images: [""]
---

## 概述

在软件开发和测试过程中，为了提高效率并减少对实际服务的依赖，我们常常使用存根服务器（Stub Server）和模拟服务器（Mock Server）这两种工具。存根服务
器和模拟服务器都是用于模拟外部依赖项的行为，但它们有不同的实现方式和用途。  

本文将介绍存根服务器和模拟服务器的概念、为什么要使用存根服务器和模拟服务器，它们的优势和用途，以及如何在开发和测试中使用它们来提高效率和可靠性。  

## Mock Server

### 什么是Mock Server？

Mock Server是一种模拟服务器，用于模仿API的真实行为。它通过模拟真实的服务，为来自客户端的请求提供真实响应。在开发和测试中，Mock Server发挥着重要的
作用。 它可以在本地计算机或云服务器上运行，拦截应用程序发出的请求，并返回预定义的响应。

### 为什么要使用Mock Server？

1. 提高开发效率  
   在开发过程中，某些依赖的服务可能尚未准备就绪或不可访问。使用Mock server可以模拟这些服务的行为，使开发人员能够继续工作而不会受到阻碍。有助于前端
   和后端开发人员在服务尚未准备好或不可用的情况下独立进行开发。
   前端团队可以利用Mock Server模拟API响应，无需等待实际后端服务的就绪，从而节省时间和资源，加快开发和测试周期。
   同时，Mock Server的使用使前端和后端开发人员能够并行开发，因为它们可以依据预定义的API规范和模拟响应进行工作，而无需相互依赖。
   这种独立且并行的开发方式提高了团队的整体效率，并促进了协作的灵活性和敏捷性。

2. 模拟各种场景  
   Mock Server可以模拟各种请求和响应场景，包括成功响应、错误响应、边界条件和异常情况。这样可以更全面地测试应用程序的行为，提高应用程序的质量和可靠性。

3. 隔离和独立性
   Mock server允许开发人员将应用程序与外部依赖项（如第三方API或服务）隔离开来，从而使开发和测试更加独立和可控。这样可以避免对实际服务产生不必要的
   依赖，提高开发效率和测试可靠性。

4. 性能测试和负载测试
   Mock server可以用于模拟高负载条件，从而进行性能测试和负载测试，以评估应用程序在真实环境中的表现。 

   总的来说，Mock Server的使用可以提高开发效率，支持并行开发，模拟各种场景，隔离依赖服务，并用于性能测试和负载测试。它是一个强大的工具，帮助开发人
员构建和测试应用程序，同时降低对实际服务的依赖性。

### 如何搭建Mock server？
搭Mock Server的方式有很多种，以下是一些常见的方式：

1. 使用Mock Server框架：有许多专门用于搭建Mock Server的框架可供选择，例如MockServer、Prism、WireMock等。这些框架提供了简单易用的接口和配置
选项，可以快速搭建和配置Mock Server。  
   **常用框架**  
   - xx
   - xx
   - xx

2. 自定义代码：可以使用编写程序语言（如Java、Python、Node.js等）来编写自定义的模拟服务器，处理来自客户端的请求，并返回预定义的响应。

3. 使用API开发工具：一些API开发工具（如Postman、Swagger等）提供了Mock Server的功能。可以使用这些工具创建和管理Mock API，并根据需要配备设置请
求和响应。

## Stub Server

### 什么是Stub Server

服务存根是对实际服务的模拟，可用来在功能上替换测试环境中的服务。 存根服务器用于替换实际应用程序服务器。  
从客户机应用程序的角度来看，服务存根看起来与其模拟的实际服务相同。 要使用服务存根来替换实际服务，必须能够将客户机应用程序中原始服务的  
URL 替换为存根服务器的 URL。

### 为什么要使用Stub Server？

### 如何搭建Stub Server

## HeartBeat使用Stubby4j搭建Stub Server

HeartBeat使用Stubby4j作为存根服务器来构建我们的服务。所有第三方服务都存根在一个存根服务器中。

### 为何选择Stubby4j？

### 构建示例



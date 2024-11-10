# bkdevops-sample

本项目是 [devops-framework](https://github.com/bkdevops-projects/devops-framework) `starter-schedule` 组件 demo

## starter-schedule 介绍

[devops-framework](https://github.com/bkdevops-projects/devops-framework) 是提炼自腾讯 DevOps 团队内部多个项目，使用约定优于配置的设计理念，帮助我们专注于DevOps业务快速开发，它具有以下优势:

- **简单** ：几乎零配置快速开发微服务，低成本上手
- **易用** ：采用`Spring Boot`组件化思想，易于学习理解
- **统一** ：目前已集成了微服务开发常用组件和统一配置
- **扩展** ：组件之间低耦合，高内聚，扩展十分方便

`starter-schedule` 组件提供分布式调度功能，帮助开发者快速接入分布式调度。

借鉴`xxl-job`的思想，将调度中心和执行器完全解耦，保持`xxl-job`的轻量化特性的同时，还具有

- 可插拔设计
- 与Spring Cloud无缝集成
- 可扩展设计
- 支持多种数据源，JDBC、MongoDB...

## 项目启动

* 数据源。`starter-schedule` 提供了 mongodb 数据源实现。启动时需提供 mongodb 服务器
  * 启动 mongodb：`cd tools/docker/mongodb && docker compose up -d`
  * 连接信息：`mongodb://root:example@localhost:27017/`
  * mongo web：http://localhost:8081
* 启动服务端。启动 `bkdevops-schedule-server` 模块下的 `cn.sliew.carp.ScheduleServerApplication` 类
* 启动客户端。启动 `bkdevops-schedule-worker` 模块下的 `cn.sliew.carp.ScheduleWorkerApplication` 类
* 启动 web。
  * clone [devops-framework](https://github.com/bkdevops-projects/devops-framework)
  * 进入 前端项目路径。`cd devops-framework/devops-boot-project/devops-boot-core/devops-schedule/devops-schedule-server/src/main/resources/frontend`
  * 安装依赖。`npm install`
  * 启动。`npm run dev`
  * 打开 http://localhost:9528
* 测试任务
  * 创建执行组
  * 创建任务
  * 查看任务执行结果

## 项目说明

* 数据源。
  * `starter-schedule` 目前只提供了 mongodb 实现。必须用 mongodb
  * 数据源配置。`bkdevops-schedule-server` 是在 `MongoConfig` 里面配置的，在配置文件中配置的 `spring.data.mongodb` 不生效
* 前端页面。
  * demo 中没有搞定这个，所以前端项目仍然是另外启动的。`devops-schedule-server` 包内包含了前端项目，并且在 [ScheduleServerWebUiConfigurer](https://github.com/bkdevops-projects/devops-framework/blob/master/devops-boot-project/devops-boot-core/devops-schedule/devops-schedule-server/src/main/kotlin/com/tencent/devops/schedule/config/ScheduleServerWebUiConfigurer.kt) 中配置了路由。而且也有 swagger ui，同样是无法访问
  * 从 `http://localhost:8082/actuator/mappings` 查看到是有 ui 的路由的，但是实际不生效
* 注册方式。
  * [boot-schedule-worker-cloud-sample](https://github.com/bkdevops-projects/devops-framework/tree/master/devops-boot-sample/boot-schedule-worker-cloud-sample) 注册方式采用 `DISCOVERY`，demo 项目使用 `AUTO`，配置略有不同
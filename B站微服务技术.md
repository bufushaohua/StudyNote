## B站微服务技术

### 微服务技术栈有哪些？

|                微服务条目                |                           落地技术                           |
| :--------------------------------------: | :----------------------------------------------------------: |
|                 服务开发                 |                SpringBoot、Spring、SpringMVC                 |
|              服务配置与管理              |            Netfix公司的Archaius、阿里的Diamond等             |
|             服务的注册与发现             |                 Eureka、Consul、Zookeeper等                  |
|                 服务调用                 |                       Rest、RPC、gRPC                        |
|                服务熔断器                |                       Hystrix、Envoy等                       |
|                 负载均衡                 |                       Ribbon、Nignx等                        |
| 服务接口调用（客户端调用服务的简化工具） |                           Feign等                            |
|                 消息队列                 |                 Kafka、RabbitMQ、ActiveMQ等                  |
|             服务配置中心管理             |                  SpringCloudConfig、Chef等                   |
|           服务路由（API网关）            |                            Zuul等                            |
|                 服务监控                 |             Zabbix、Nagios、Metrics、Spectator等             |
|                全链路追踪                |                   Zipkin、Brave、Dapper等                    |
|                 服务部署                 |               Docker、OpenStack、Kubernetes等                |
|             数据流操作开发包             | SpringCloud Stream（封装与Redis、Rabbit、Kafka等发送接收消息） |
|               事件消息总线               |                       SpringCloud Bus                        |
|                    ……                    |                              ……                              |

### SpringCloud和SpringBoot是什么关系？

- SpringBoot专注于快速方便的开发的单个个体微服务
- SpringCloud是关注全局的微服务协调整理治理框架，他将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等待继承服务
- SpringBoot可以离开SpringCloud独立使用开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系
- SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架

### SpringCloud与Dubbo之间区别对比

|              | Dubbo         | SpringCloud                 |
| ------------ | ------------- | --------------------------- |
| 服务注册中心 | Zookeeper     | SpringCloud Netflix Eureka  |
| 服务调用方式 | RPC           | Rest API                    |
| 服务监控     | Dubbo-monitor | SpringBoot Admin            |
| 断路器       | 不完善        | SpringCloud Netflix Hystrix |
| 服务网关     | 无            | SpringCloud Netflix  Zuul   |
| 分布式配置   | 无            | SpringCloud Config          |
| 服务跟踪     | 无            | SpringCloud Sleuth          |
| 消息总线     | 无            | SpringCloud Bus             |
| 数据流       | 无            | SpringCloud Stream          |
| 批量任务     | 无            | SpringCloud Task            |
| ……           | ……            | ……                          |

微服务模块：

1. 建module
2. 改pom
3. 写YML
4. 主启动
5. 业务类
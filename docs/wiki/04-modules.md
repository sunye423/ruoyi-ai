# 模块说明

## 顶层模块

| 模块 | 职责 |
| --- | --- |
| `ruoyi-admin` | Spring Boot 启动模块，聚合领域依赖，提供认证、验证码和首页入口 |
| `ruoyi-common` | BOM 与通用 Starter/基础设施实现 |
| `ruoyi-modules` | 系统、AI、工作流与代码生成等业务模块 |
| `ruoyi-extend` | 监控中心与 SnailJob 服务，可按需独立部署 |

## 领域模块

| 模块 | 主要内容 | 常用入口 |
| --- | --- | --- |
| `ruoyi-system` | 用户、角色、菜单、租户、字典、OSS、系统配置 | `org.ruoyi.system.controller` |
| `ruoyi-chat` | 模型、对话、知识库、向量检索、MCP、图像生成 | `ChatController`、`KnowledgeInfoController` |
| `ruoyi-aiflow` | AI 工作流定义、组件、节点、边与运行时 | `WorkflowController`、`WorkflowRuntimeController` |
| `ruoyi-workflow` | Warm-Flow 流程定义、实例、任务和审批 | `FlwDefinitionController`、`FlwTaskController` |
| `ruoyi-generator` | 数据表导入与 CRUD 代码生成 | `GenController` |

## 通用模块分组

`ruoyi-common` 中的子模块按用途可分为：

- Web 与安全：`core`、`web`、`satoken`、`security`、`sensitive`、`encrypt`。
- 数据：`mybatis`、`redis`、`tenant`、`translation`。
- 通信：`sse`、`websocket`、`mail`、`sms`、`social`。
- 文件：`oss`、`excel`、`doc`。
- 运维治理：`log`、`job`、`ratelimiter`、`idempotent`。
- AI 公共契约：`chat`，包含聊天 DTO、服务接口、线程上下文和工厂。

## 扩展模块

- `ruoyi-monitor-admin`：Spring Boot Admin 监控服务。主应用中的客户端默认关闭。
- `ruoyi-snailjob-server`：SnailJob 调度服务。主应用中的客户端默认关闭。

这些模块不是主应用成功启动的必需条件，启用前应同步修改对应配置并初始化其依赖数据。

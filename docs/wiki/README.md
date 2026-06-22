# RuoYi AI Repo Wiki

这套 Wiki 面向第一次接触仓库的开发者，描述当前代码库的结构、运行方式和扩展入口。内容以仓库中的 `pom.xml`、配置文件和源码为准。

## 阅读导航

1. [项目概览](./01-overview.md)：能力范围、技术栈和请求链路
2. [本地启动](./02-getting-started.md)：准备依赖、初始化数据、构建与运行
3. [系统架构](./03-architecture.md)：分层、模块依赖和关键运行链路
4. [模块说明](./04-modules.md)：各 Maven 模块的职责与源码入口
5. [配置指南](./05-configuration.md)：环境、数据库、Redis、AI 与安全配置
6. [开发指南](./06-development.md)：新增接口、业务模块和 AI Provider 的约定
7. [部署与排障](./07-deployment-troubleshooting.md)：Docker、生产检查和常见问题

## 快速定位

| 目标 | 入口 |
| --- | --- |
| 应用启动类 | `ruoyi-admin/src/main/java/org/ruoyi/RuoYiAIApplication.java` |
| 主配置 | `ruoyi-admin/src/main/resources/application.yml` |
| 开发配置 | `ruoyi-admin/src/main/resources/application-dev.yml` |
| 数据库初始化 | `docs/script/sql/ruoyi-ai-v3_mysql8.sql` |
| AI 对话与知识库 | `ruoyi-modules/ruoyi-chat` |
| AI 工作流编排 | `ruoyi-modules/ruoyi-aiflow` |
| 审批工作流 | `ruoyi-modules/ruoyi-workflow` |
| 系统管理 | `ruoyi-modules/ruoyi-system` |
| 一键 Docker 编排 | `docs/docker/ruoyi-ai/docker-compose-all.yaml` |

> 用户前端和管理前端不在本仓库，分别维护在 `ruoyi-web` 与 `ruoyi-admin` 独立仓库中。

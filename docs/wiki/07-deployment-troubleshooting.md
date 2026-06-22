# 部署与排障

## Docker 一键部署

本仓库的编排文件位于 `docs/docker/ruoyi-ai/`，不是仓库根目录。启动全部服务：

```bash
docker compose -f docs/docker/ruoyi-ai/docker-compose-all.yaml up -d
docker compose -f docs/docker/ruoyi-ai/docker-compose-all.yaml ps
```

默认映射端口：

| 服务 | 端口 |
| --- | --- |
| 管理端 | `25666` |
| 用户端 | `25137` |
| 后端 API | `26039` |
| MySQL | `23306` |
| Redis | `26379` |
| Weaviate | `28080` |
| MinIO API / Console | `29000` / `29090` |

编排中的数据库名是 `ruoyi-ai-agent`，而本地开发配置默认使用 `ruoyi-ai`。从容器配置切换到本地运行时，要同步检查库名。

## 生产上线检查

- 使用 `prod` profile，并从外部注入机密。
- 更换默认数据库、MinIO、监控、调度和 JWT 凭据。
- 不暴露 MySQL、Redis、向量库和 MinIO 管理端口到公网。
- 为上传目录、日志和数据库配置持久卷与备份。
- 为 SSE 关闭代理缓冲并适当延长读取超时。
- 按需关闭 OpenAPI，收紧 `security.excludes`。
- 配置健康检查、资源限制、日志轮转和告警。

## 常见问题

### 数据源连接失败

检查数据库名是否为当前环境使用的 `ruoyi-ai` 或 `ruoyi-ai-agent`，确认 MySQL 8 驱动、时区和 `allowPublicKeyRetrieval` 参数，并验证容器内不能用 `localhost` 访问另一个容器。

### Redis 启动报错

确认 Redis 地址、端口和密码在 Spring Data Redis 与 Redisson 两处语义一致。容器环境应使用服务名 `redis`。

### 应用启动时意外连接 Neo4j

通用配置已排除 `Neo4jAutoConfiguration`。如果外部配置覆盖了 `spring.autoconfigure.exclude`，需保留该排除项，除非确实要启用 Neo4j。

### 对话没有流式输出

先直接请求后端排除前端问题，再检查 Nginx/网关是否缓冲响应、连接超时是否过短、客户端是否按 SSE 消费，以及 Provider 是否真正返回流式事件。

### 知识库检索为空

确认附件解析成功、片段已生成、Embedding 维度与集合一致、租户 ID 一致、向量库连接正常，并检查召回阈值和重排结果。

### `-Plocal` 启动失败

仓库没有内置 `application-local.yml`。请新增本机配置或改用默认的 `-Pdev`。

## 日志定位顺序

1. 启动日志：profile、端口、自动配置和 Bean 创建失败。
2. 数据源/Redis 日志：连接、认证、库名与超时。
3. HTTP 请求日志：状态码、权限和租户上下文。
4. AI 调用日志：Provider、模型名、耗时和经过脱敏的错误。
5. 外部服务日志：向量库、对象存储、模型网关与反向代理。

# 配置指南

## 配置层次

| 文件 | 用途 |
| --- | --- |
| `application.yml` | 通用配置与 Maven profile 占位符 |
| `application-dev.yml` | 默认开发环境的数据源、Redis 和集成配置 |
| `application-prod.yml` | 生产环境覆盖项 |

根 `pom.xml` 定义 `dev`、`prod` 和 `local` Maven profile，但仓库当前只提供 `application-dev.yml` 与 `application-prod.yml`。使用 `-Plocal` 前需要自行提供 `application-local.yml` 或其他外部配置源。

## 关键配置域

- `server`：默认端口 `6039`，使用 Undertow。
- `spring.datasource.dynamic`：动态数据源；默认数据源名为 `master`。
- `spring.data.redis` / `redisson`：缓存、会话及分布式能力。
- `sa-token`：认证 Header、并发登录和 JWT 密钥。
- `tenant`：多租户开关与排除表。
- `security.excludes`：匿名访问白名单，新增条目需谨慎审查。
- `springdoc`：OpenAPI 文档开关与扫描包。
- `mcp`：MCP SSE 客户端开关与地址。
- `mail`、`sms`、`justauth`：外部通知和第三方登录。
- `snail-job`、`spring.boot.admin.client`：可选运维服务。

## 环境变量覆盖

Spring Boot 支持用环境变量覆盖配置，例如：

```bash
export SPRING_DATASOURCE_DYNAMIC_DATASOURCE_MASTER_URL='jdbc:mysql://db:3306/ruoyi-ai'
export SPRING_DATASOURCE_DYNAMIC_DATASOURCE_MASTER_USERNAME='ruoyi'
export SPRING_DATASOURCE_DYNAMIC_DATASOURCE_MASTER_PASSWORD='change-me'
export SPRING_DATA_REDIS_HOST='redis'
export SA_TOKEN_JWT_SECRET_KEY='replace-with-a-long-random-secret'
```

生产环境至少要更换数据库密码、Redis 密码、JWT 密钥、第三方登录密钥、OSS 凭据和所有模型 API Key。

## AI 配置

模型与 Provider 既有数据库配置，也有代码侧实现。排查模型不可用时依次确认：

1. Provider 记录已启用且类型与实现匹配。
2. 模型记录引用了正确 Provider。
3. Base URL、API Key、模型名称和超时正确。
4. 宿主机或容器能访问模型服务。
5. 流式请求未被反向代理缓冲。

向量数据库不是最小启动的强制依赖，但知识库入库和检索需要正确选择并连接一种向量存储。

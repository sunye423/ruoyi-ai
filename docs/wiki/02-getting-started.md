# 本地启动

## 1. 环境要求

- JDK 17
- Maven 3.9+
- MySQL 8.0
- Redis 6+
- 可选：Weaviate、Milvus 或 Qdrant（使用知识库向量检索时）
- 可选：MinIO 或兼容 S3 的对象存储（使用对象存储时）

## 2. 初始化数据库

创建数据库并导入主 SQL：

```sql
CREATE DATABASE `ruoyi-ai`
  DEFAULT CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

随后导入：

```text
docs/script/sql/ruoyi-ai-v3_mysql8.sql
```

`docs/script/sql/update/` 中的文件是增量升级脚本，不应在不了解当前库版本时重复执行。

## 3. 配置开发环境

默认 Maven profile 是 `dev`，会加载 `application-dev.yml`。至少确认以下配置：

```yaml
spring:
  datasource:
    dynamic:
      datasource:
        master:
          url: jdbc:mysql://127.0.0.1:3306/ruoyi-ai
          username: root
          password: root

spring.data.redis:
  host: localhost
  port: 6379
```

不要把真实模型密钥或生产凭据提交到仓库。优先使用环境变量或本机未纳入版本控制的覆盖配置。

## 4. 构建与启动

```bash
mvn clean package -Pdev
java -jar ruoyi-admin/target/ruoyi-admin.jar
```

开发时也可从 IDE 运行：

```text
org.ruoyi.RuoYiAIApplication
```

默认后端地址是 `http://localhost:6039`。访问根路径可检查应用是否已响应；接口文档是否可见由 `springdoc.api-docs.enabled` 和安全白名单共同决定。

## 5. 最小验证

```bash
curl http://localhost:6039/
curl http://localhost:6039/auth/tenant/list
```

如果启动失败，先依次检查 MySQL 库名和账号、Redis 连通性、端口 `6039` 是否被占用，以及 Maven 是否确实使用了 `dev` profile。

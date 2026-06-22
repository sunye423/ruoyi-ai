# 开发指南

## 新增普通业务接口

建议沿用现有领域结构：

1. 在对应模块增加 `domain`、BO/VO。
2. 增加 Mapper 与必要的 XML。
3. 在 Service 中实现业务规则和事务。
4. Controller 只处理协议、权限和参数校验。
5. 数据库变化放入新的增量 SQL，不修改已发布的升级脚本。
6. 增加单元或集成测试，并用显式参数运行测试。

常见注解和能力可从 `ruoyi-system` 的 Controller/Service 实现中寻找范例，包括权限校验、操作日志、重复提交、数据权限和 Excel 导出。

## 新增 AI Provider

Provider 实现位于：

```text
ruoyi-modules/ruoyi-chat/src/main/java/org/ruoyi/service/chat/impl/provider
```

新增实现时：

- 复用 `AbstractChatService` 和公共聊天契约。
- 将 Provider 选择逻辑集中在既有工厂/路由机制中。
- 支持超时、取消、流式异常和供应商错误到平台错误的转换。
- API Key 只从配置或数据库读取，不写入日志。
- 为不兼容 OpenAI 协议的字段增加独立映射，避免污染公共 DTO。

## 扩展 RAG

- 新解析器：接入附件解析链路，并为大文件、空文件和损坏文件提供明确错误。
- 新向量库：实现向量存储抽象，统一集合命名、删除和租户隔离语义。
- 新重排器：实现 rerank 服务，并明确候选数量、截断和降级策略。

## 数据与租户边界

多租户默认开启。新增表时要明确它属于：

- 租户业务表：保留租户字段并让租户拦截器生效。
- 平台共享表：仅在确有必要时加入 `tenant.excludes`。

同时检查数据权限注解、逻辑删除、审计字段和 ID 策略。默认主键策略为 `ASSIGN_ID`。

## 构建和测试

父 POM 默认 `skipTests=true`，因此常规打包不等于测试通过。提交前显式启用测试：

```bash
mvn test -DskipTests=false -Pdev
mvn clean package -DskipTests=false -Pdev
```

改动单个模块时可缩短反馈周期：

```bash
mvn -pl ruoyi-modules/ruoyi-chat -am test -DskipTests=false -Pdev
```

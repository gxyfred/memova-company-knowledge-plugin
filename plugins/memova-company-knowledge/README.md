# 公司知识助手 Plugin

这是 Memova 公司公共知识库的独立薄 Plugin。它只包含查询、入职自检、显式提交 Skill
和 `company_knowledge_assistant` MCP 连接声明；身份、权限、审计、发布、检索、引用与
新鲜度全部由公司服务端执行。

Plugin 不保存 Microsoft 或 GitHub 密码、Token，不包含员工/职位白名单，也不会后台
自动收集内容。提交知识必须由员工显式发起，并分别确认预览和最终发布。

版本 `0.4.1` 在受保护工具发现前检查专用 MCP 登录状态，并把 `Not logged in` 路由到员工本人
Microsoft OAuth。登录完成后必须完全重启 Codex 并新建任务，再执行七工具和身份自检。Plugin 不
接收密码、MFA、Token、Cookie 或回调内容。

版本 `0.4.2` 为发布预览增加与后端一致的完整 `ReceiptCandidateV1` 机器契约。提交 Skill 只按该
契约构造候选；校验失败时后端返回安全字段路径，不回显被拒绝内容，也不要求猜测服务端字段。

发布仍使用员工本人委托身份。安装与升级说明见仓库根目录 `README.md`。

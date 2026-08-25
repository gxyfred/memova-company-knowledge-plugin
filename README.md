# Memova 公司知识助手（公开分发）

本公开仓库只分发公司知识助手 Plugin 和 Codex Marketplace 元数据，不包含后端代码、
生产凭证或公司知识数据。

## 最简单的员工安装方式

员工把 [`INSTALL_PROMPT.md`](INSTALL_PROMPT.md) 中整段文字复制给自己的 Codex，
让 Codex 完成安装和验证。仓库可以匿名只读访问，不要求员工登录 GitHub，也不需要理解
`git clone`。

所有员工现在都可按统一流程参加安装和真实使用测试。第一次使用、日常查询、本人委托知识发布、
跨员工验证、更正和常见故障处理，请直接阅读
[`EMPLOYEE_GUIDE_ZH.md`](EMPLOYEE_GUIDE_ZH.md)。该指南面向完全不了解 GitHub、Plugin、MCP
或 OAuth 的员工。

安装／升级话术会先刷新公开 Marketplace 和 Plugin，然后运行 `codex mcp list`。如果专用 MCP
显示 `Not logged in`，Codex 会执行明确的 `codex mcp login company_knowledge_assistant` 命令；
员工本人在 Microsoft 页面完成登录和 MFA。Codex 不会代为操作身份页面，也不会要求员工把密码、
验证码、Token、Cookie 或 OAuth 回调内容发到对话中。

首次登录成功后需要完全退出并重新打开 Codex，再新建一个任务并发送：

> 开始公司知识助手入职自检

受保护的 MCP 工具可能在登录前完全不出现在任务工具面，因此 `0.4.1` 不再依赖首次查询自动弹出
登录。它会先完成显式登录，再在重启后的新任务中验证七个工具。

`0.4.2` 为发布预览增加完整的机器可读候选契约。Plugin 与后端 MCP 使用同一份字段、枚举和证据
结构；不完整候选会返回安全字段路径，而不会要求员工或 Codex 猜测服务端字段。

`0.4.3` 为当前 Codex 任务中的 `business_status` 增加服务端确定性收集。员工只选择状态事实；
来源定位、版本、证据锚点和知识所有者由后端生成，不需要为了当前任务证据额外提供 HTTPS 链接。

## 管理员/高级用户手动安装

```bash
codex plugin marketplace add gxyfred/memova-company-knowledge-plugin --ref main
codex plugin add memova-company-knowledge@memova-company-knowledge-pilot
codex mcp login company_knowledge_assistant
```

如果 Marketplace 已经存在，先用
`codex plugin marketplace upgrade memova-company-knowledge-pilot` 刷新快照。登录后完全重启 Codex。

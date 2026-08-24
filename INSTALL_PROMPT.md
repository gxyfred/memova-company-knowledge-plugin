# 发给员工 Codex 的固定安装话术

请帮我安装 Memova“公司知识助手”Plugin，并完成安装验证：

1. 直接添加公开 Marketplace，不检查或要求 GitHub 登录：
   `codex plugin marketplace add gxyfred/memova-company-knowledge-plugin --ref main`
2. 安装 Plugin：
   `codex plugin add memova-company-knowledge@memova-company-knowledge-pilot`
3. 只读验证 Plugin 已启用且版本为 `0.4.0`，并确认 MCP 名称为
   `company_knowledge_assistant`。不要修改或删除其他 Plugin、Marketplace 或 Codex 设置。
4. 告诉我完全退出并重新打开 Codex。重启后，新建任务并执行“开始公司知识助手入职自检”。
5. 首次连接只使用我的 Memova Microsoft 工作账号完成 OAuth；不要索取或显示任何密码、
   2FA 验证码、访问 Token 或刷新 Token。

如果某一步失败，请停在只读诊断并告诉我确切失败步骤；不要反复重装、删除配置或绕过认证。

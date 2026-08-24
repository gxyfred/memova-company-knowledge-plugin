# 发给员工 Codex 的固定安装话术

请帮我安装 Memova 私有“公司知识助手”Plugin，并完成安装验证：

1. 先检查本机 Codex CLI 和 GitHub 登录状态；如果当前 GitHub 账号没有读取私有仓库
   `gxyfred/memova-company-knowledge-plugin` 的权限，只引导我完成一次官方 GitHub 设备登录
   或联系管理员开通只读权限。不要让我在对话中粘贴密码、验证码或 Token，也不要保存凭证。
2. 添加私有 Marketplace：
   `codex plugin marketplace add gxyfred/memova-company-knowledge-plugin --ref main`
3. 安装 Plugin：
   `codex plugin add memova-company-knowledge@memova-company-knowledge-pilot`
4. 只读验证 Plugin 已启用且版本为 `0.4.0`，并确认 MCP 名称为
   `company_knowledge_assistant`。不要修改或删除其他 Plugin、Marketplace 或 Codex 设置。
5. 告诉我完全退出并重新打开 Codex。重启后，新建任务并执行“开始公司知识助手入职自检”。
6. 首次连接只使用我的 Memova Microsoft 工作账号完成 OAuth；不要索取或显示任何密码、
   2FA 验证码、访问 Token 或刷新 Token。

如果某一步失败，请停在只读诊断并告诉我确切失败步骤；不要反复重装、删除配置或绕过认证。

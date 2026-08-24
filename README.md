# Memova 公司知识助手（私有分发）

本私有仓库只分发公司知识助手 Plugin 和 Codex Marketplace 元数据，不包含后端代码、
生产凭证或公司知识数据。

## 最简单的员工安装方式

管理员先给员工的 GitHub 账号授予本私有仓库只读权限。员工随后把
[`INSTALL_PROMPT.md`](INSTALL_PROMPT.md) 中整段文字复制给自己的 Codex，让 Codex
完成检查与安装。员工不需要理解 `git clone`。

安装完成后需要完全退出并重新打开 Codex，再新建一个任务并发送：

> 开始公司知识助手入职自检

首次查询时，Codex 会打开 Memova Microsoft 工作账号登录与授权流程。Plugin 不会要求
员工把密码、验证码或 Token 发到对话中。

## 管理员/高级用户手动安装

```bash
codex plugin marketplace add gxyfred/memova-company-knowledge-plugin --ref main
codex plugin add memova-company-knowledge@memova-company-knowledge-pilot
```

安装后完全重启 Codex。

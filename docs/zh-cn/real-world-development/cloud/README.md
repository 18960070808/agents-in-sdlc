---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可以在云端异步处理工作。你可以在 GitHub 上分配任务，云端代理会在后台接手工作——探索存储库、进行更改并创建拉取请求——而你可以继续处理其他事情。

在本系列练习中，你将添加云端代理会遵循的自定义指令，然后分配一个 GitHub 议题，让它实现相关工作。你还将审查并使用自定义代理来引导其工作方式，在代理仪表板中监控并指导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云端代理会遵循的自定义指令 |
| [2. 云端代理][ex2] | 异步代理 | 将议题分配给 GitHub Copilot 云端代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并指导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云端代理

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所需的代理、MCP、代码审查和 Copilot CLI 功能，因此可用它完成全部练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云端代理适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise** 计划，Copilot Free 除外。对于 Copilot Business 或 Enterprise，组织管理员必须启用此功能。

> [!NOTE]
> 本学习路径不涉及 MCP。若要了解如何将 MCP 服务器与 Copilot 搭配使用，请参阅 [CLI 学习路径](../cli/)或 [VS Code 学习路径](../vscode/)。

## 开始学习

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/

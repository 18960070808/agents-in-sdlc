---
title: "练习 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都保存在同一分支。现在一起审查，然后创建拉取请求。之前你手动合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理分别放在几个 PR 中。为简化研讨会流程，这些工作都在同一对话和分支中完成，并合并到一个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动化合并流程；
- 检查完整功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能开发过程中，你使用 Copilot 规划、实现并验证功能。现在 Tailspin Toys 希望自动完成 PR 后续流程，同时让开发人员保留合并授权权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题——修复失败的持续集成（CI）检查、响应审查评论，并在需要时变基——然后在存储库允许的情况下启用 GitHub 自动合并。

之前一直由你手动选择 **Merge pull request**。Agent Merge 可以接手这项工作，但启用前仍应先审查并确认更改已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成，现在一起审查、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项中选择 **Default** 并按 <kbd>Enter</kbd>。
4. 使用 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认看到类似 Copilot 消息：“PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请阅读报告的原因，并在存储库允许时手动完成审查后的合并。

## 总结和后续步骤

你已自动化开发流程的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并流程；
- 检查了完整功能更改和验证证据；
- 创建了筛选功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

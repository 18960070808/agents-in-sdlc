---
title: "练习 10 - 回顾与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题；
- 为游戏卡片添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）；
- 从筛选议题开始，使用 Plan 模式确定方案、使用 Autopilot 模式实现，并使用 Interactive 模式审查；
- 使用自定义指令引导代理，随后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol（MCP）服务器，并通过真实浏览器体验筛选功能；
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整筛选更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新并应用指令、自定义 `quality-checks` 报告、创建 QA 配置，并包含相关测试。

从规划筛选功能到打开其 PR，你一直使用同一对话和分支。为简化研讨会流程，这些工作都放在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查汇总后再提交 PR。

新增测试应解决真实缺口；如果 QA 运行后无需补充测试，也可能是正确结果。合并前要审查代码和证据，并在更改后重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器则连接外部工具。审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前梳理方案，使用 **Interactive** 持续参与针对性更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可以选用更快的模型，复杂任务则可选用能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*以及*如何*构建，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 CLI 功能：

- 使用 `/review` 请代码审查代理分析更改；
- 使用 `/rubber-duck` 讨论问题并获取另一种看法；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离另一项任务；
- 使用 `/delegate` 将任务发送给 Copilot 云端代理。

## 后续步骤

持续使用工具是提升技能的最佳方式！将它用于生产代码、业余项目，或那个想了很多年却一直没做的小应用。与团队分享经验，也向他人学习。最后，别忘了继续探索文档。

如果想从终端继续扩展 Tailspin Toys，可以选择学习可选的 [Foundry Backer Concierge 系列][foundry]。如要比较不同环境，可以浏览 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云端代理研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

---
title: "练习 7 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到自定义代理中，并定义清晰的 QA 流程。

在本课中，你将：

- 了解自定义代理如何配合指令、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 代理，根据筛选议题审查其发现。

## 场景

创建拉取请求（PR）前，Tailspin Toys 希望一致地检查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调此 QA 流程并生成可复用报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你已使用的自定义项各有用途：存储库指令描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力来评估需求并报告发现。它不会取代其他能力，也不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置将定义 QA 要执行的检查及其工作边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新代理前，先审查配置，确认 Copilot 捕获了预期的 QA 工作流和边界。这样可以避免代理在你只希望验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 可选，但可以为代理提供清晰的显示名称。
3. 阅读配置指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会询问，并且不会提交更改或打开拉取请求。
5. 如果生成的配置缺少这些职责或边界，请先要求默认代理修订，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置后，恢复原来的筛选对话并选择 QA，这样它就能使用现有议题和规划决策。

1. 输入以下命令启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会显示在可用代理列表中，但仍可通过上述命令启用。

2. 使用以下提示，请 QA 审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专用角色并审查其工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 代理，根据筛选议题审查其发现。

现在实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来将它们[整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

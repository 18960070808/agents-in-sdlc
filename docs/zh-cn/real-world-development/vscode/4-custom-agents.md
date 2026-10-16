---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库 `.github/agents` 文件夹中的 Markdown 文件里定义代理，并为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML frontmatter，后续 Markdown 提示则定义代理的行为、专长和指令。

### 自定义代理与代理技能的区别

自定义代理和[代理技能][agent-skills-concept]在概念上有一些重叠。两者主要通过 Markdown 文件定义，都会指导 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行工作的主体，而**技能**是工具。

自定义代理拥有独立的上下文窗口，并且可以在工作中协调技能甚至其他代理。本实验的无障碍自定义代理会依据无障碍指南审查并更新网站；工作过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。和其他 AI 用法一样，需要针对环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题的文本与背景对比度不足，难以阅读。为解决此无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理生成相关功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已为你创建了无障碍自定义代理。先查看其内容，了解它如何引导 Copilot。

返回 codespace，打开终端并从 `main` 创建用于无障碍工作的独立分支（筛选 PR 会保留在练习 3 的分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，了解其中列出的：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择代理后，会话就会采用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异，相关步骤会特别说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认显示当前代理 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中内嵌显示文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，显示代理修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会对存储库进行实际更改，过程中可能编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即面向特定任务和领域的专用 AI 助手。使用自定义代理可以将团队专业知识和标准整理为可复用代理，更有效地引导 Copilot 完成特定工作。

你了解了以下概念：

- 如何定义自定义代理；
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来[监控并指导代理工作][next-lesson]：实时审查更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

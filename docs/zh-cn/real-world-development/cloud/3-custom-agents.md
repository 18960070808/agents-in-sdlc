---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义代理，并为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，从而帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含至少有 `name` 和 `description` 字段的 YAML frontmatter，后续 Markdown 提示则定义代理的行为、专长和指令。

### 自定义代理与代理技能的区别

自定义代理和[代理技能][agent-skills-concept]在概念上有一些重叠。两者主要通过 Markdown 文件定义，都会指导 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行工作的主体，而**技能**是工具。

自定义代理拥有独立的上下文窗口，并且可以在工作中协调技能甚至其他代理。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；工作过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。和其他 AI 用法一样，需要针对环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理，你将了解：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题的文本与背景对比度不足，难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来将使用自定义代理生成相关功能。

## 查看无障碍自定义代理

项目已为你创建了无障碍自定义代理。先查看其内容，了解它如何引导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，了解其中列出的：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误。

## 创建并分配议题

任务控制中心是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot 云端代理、监控任务，甚至重定向并补充指导。现在将实现高对比度模式的任务分配给 Copilot。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云端代理分配界面截图，突出显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 就会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即面向特定任务和领域的专用 AI 助手。使用自定义代理可以将团队专业知识和标准整理为可复用的代理，更有效地引导 Copilot 完成特定工作。

你了解了以下概念：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来前往任务控制中心，[监控并指导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为在组织中使用自定义代理做好准备][org-custom-agents]
- [为在企业中使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云端代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

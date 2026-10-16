---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还需要使用数据和外部服务，并允许 Copilot 使用更多自动化工具。这正是 MCP 服务器发挥作用的地方。MCP 服务器扩展了 Copilot CLI 内置的功能，为它提供更多工具和服务。

在本课中，你将：

- 了解 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 请代理驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新还需要实际与界面交互。你希望 Copilot 像用户一样使用网站，进一步自动化更改验证过程，并增强对结果的信心。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组可供 AI 代理访问的工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并为生态系统做贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能正常。现在使用 Copilot 中的 Playwright MCP 服务器执行相同验证。

1. 使用以下提示，请 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断使用 MCP 服务器，不一定要明确要求。不过指出要使用的工具有助于获得更一致的结果，也可以节省一些 token。

2. 观察 Copilot 如何列出在浏览器中验证功能所执行的步骤。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器并向你报告结果。

## 总结和后续步骤

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验了功能。回顾一下，你：

- 了解了 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加了 Playwright MCP 服务器；
- 请代理驱动浏览器并体验筛选功能。

接下来创建一个 [QA 自定义代理][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

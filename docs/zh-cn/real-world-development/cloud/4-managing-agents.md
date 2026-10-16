---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云端代理处理了三个独立任务，以改善用户体验并增加功能。云端代理虽然以异步、自主的方式工作，但监控任务进度仍然很重要。

你可以使用多种工具管理分配给云端代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心中，可以查看所有具有未关闭拉取请求（PR）的代理任务、探索其执行过程，甚至指导正在进行的会话。

在本课中，你将：

- 浏览代理页面，监控云端代理任务；
- 在会话执行过程中提出额外需求。

## 场景

团队在让代理实现高对比度模式后，意识到也正好可以添加浅色模式。既然已经在更新网站样式并添加切换功能，将这项功能一并实现很合理。你将指导代理，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云端代理任务

查看分配给 Copilot 云端代理的所有任务的当前状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。应能看到已分配给 Copilot 的任务，包括：
   - 更新代码文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 为 Copilot 创建工作环境；
   - 设置 MCP 服务器；
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和采用的方法而异。

4. 同时留意右侧的拉取请求（PR）窗格。你可以在此查看 PR 和已更改文件，以便进一步监控。

## 指导云端代理

现在你已了解活动任务，可以要求 Copilot 在实现高对比度模式时也加入浅色模式切换。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。看到类似下方的消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的云端代理任务截图，突出显示 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会在完成发送消息时正在执行的步骤后，将新要求纳入工作流程。

这也需要几分钟，可以借此回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和指导 GitHub Copilot 云端代理任务的中心位置。借助这个任务控制中心，可以跟踪所有活动任务和已完成任务、查看正在执行的工作，还能重定向进行中的任务以调整范围或补充指导。

你了解了以下概念：

- 使用任务控制中心和代理页面监控云端代理任务；
- 重定向进行中的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

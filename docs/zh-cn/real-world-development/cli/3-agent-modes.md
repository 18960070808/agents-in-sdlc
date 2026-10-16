---
title: "练习 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定实现方案，使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们从为项目添加一项小功能开始。更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有工作流，帮助我们以正确方式构建正确的功能。本课将开始介绍典型的代理驱动开发流程：从议题出发生成新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。通常会为不同类型的工作使用不同分支或 PR；这里为聚焦核心概念而简化流程。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话；
- 使用 Plan 模式明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还会更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办议题描述了该功能，但组合类别等细节需要在编码前达成一致。你将使用 Plan 模式确定这些决策，再授权 Autopilot 执行范围明确的实现。

## 背景

在开发工作流中引入 AI 编码代理并不会改变基本原则，甚至让这些原则更加重要。大多数开发人员的流程大致如下：

1. 从描述所需工作的议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查通过且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多与上述流程类似。

遵循这种标准方法，可以确保 AI 生成的代码满足要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试并迭代，直到任务完成。

先使用 Plan 模式审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确保存储库和终端已就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

现在提示中应出现 `#7`（或类似编号）。`#` 可将 GitHub 上的议题或拉取请求（PR）带入上下文。

## 规划筛选功能

规划能让你在交由 Copilot 执行之前，先确定实现功能或完成任务的方式。对于复杂工作，花时间规划总是有益的。切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：探索项目并确定最佳方法。

3. Copilot 可能会询问筛选功能应该如何工作。根据自己的偏好作答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划与已确定的选择不符，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现。让 Copilot 使用 Autopilot 自行处理。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，与任何其他代码一样，合并前都需要审查。现在审查代码并运行网站，确认结果正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时确定的选择检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查输出，并要求 Copilot 解决任何失败。

## 体验新功能

代码看起来没问题，但能否实际运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合议题和计划中的约定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了一项功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话；
- 使用 Plan 模式明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来通过[使用自定义指令][next-lesson]，进一步了解代码生成过程，并确保其遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

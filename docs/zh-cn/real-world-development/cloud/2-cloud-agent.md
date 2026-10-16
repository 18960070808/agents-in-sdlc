---
title: "练习 2 - GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务：未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云端代理可以自主执行代码更新和功能开发等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以把繁琐任务交给代理，加快开发进度，让开发人员专注于更重要的工作。

你将使用 Copilot 云端代理了解：

- 如何自定义代码生成环境；
- 如何确保操作安全；
- 明确限定议题范围的重要性；
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有妥善编写文档，实际上完全没有文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每个游戏的详情页显示同类别的其他游戏，让用户可以继续浏览。他们暂时不要求完善的实现，只需要可以用于 UX 验收测试的版本。目前这项工作受阻，但优先级更高的议题尚待处理。

这两项工作都可能被降优先级，非常适合交给 Copilot 云端代理异步处理。开发人员可以先处理其他工作，之后再回来审查 Copilot 的结果。

## 了解 GitHub Copilot 云端代理

[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。你可以通过多种方式委派工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 没有魔法，不能免去思考。即使看似简单的操作，深入分析后也可能很复杂，因此要[谨慎限定交给 Copilot 云端代理的任务范围][cloud-agent-best-practices]。应把它当作 AI 结对编程助手：分阶段工作、学习、实验并持续调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的指导作用。如果已经完成自定义指令的实践，就已见过 `.github/copilot-instructions.md` 的效果；否则可以现在快速查看。在分配任务前，只读浏览本存储库中的指令文件，以便稍后观察其影响。

在 GitHub Web UI 或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令应用范围限定为 `**/*.test.ts`。

下一节会将 `Code lacks documentation` 议题分配给云端代理。查看它创建的拉取请求时，留意 TSDoc 文档注释、TypeScript 约定和注释头，这些内容来自上述指令文件。后续课程审查 PR 时还会再次提到。

## 为 Copilot 云端代理设置开发环境

编写代码通常需要特定环境和设置脚本，以确保一切正常。将任务分配给 Copilot 时也是如此，它会以类似软件工程师的方式执行任务。

云端代理在工作时使用 [GitHub Actions][github-actions]。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在代理开始工作前运行，从而提供所需开发工具和依赖项。本实验已提前配置好此工作流，其中包含 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为*你的步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在这些步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的任务。Copilot 开始处理拉取请求前，GitHub Actions 会运行此任务。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于确认工作流成功，而不必等到 Copilot 运行它。

## 添加文档

每个人都知道文档的重要性，但许多项目的文档已经过时或完全缺失。这类技术债务常被忽略，导致效率降低、代码库更难维护，也更难让新开发人员加入团队。Copilot 擅长创建文档，因此很适合处理这项工作。它会在后台生成所需文档，你将在后续练习中回来审查。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办列表可能需要几分钟。它正在设置环境（运行前面介绍的工作流）、分析项目并确定合适的解决方案。

13. 查看列表及 Copilot 将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到状态更新。

16. 浏览实时会话，查看 Copilot 如何探索代码、了解当前状态、暂停思考并确定计划，以及编写代码。

整个过程可能需要几分钟。Copilot 云端代理的主要目标之一就是异步执行任务，让我们可以专注于其他工作。接下来利用这一特点，再分配一项任务给代理，然后编写代码为应用添加功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云端代理的一大优势是可以并行分配工作：你可以专注于一组任务，同时让它处理另一组任务。为设计团队添加相关游戏部分可能不会花很久，但这些时间仍可用于其他工作。现在把它交给 Copilot 云端代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理！

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正处理第二项请求！云端代理的工作方式类似软件工程师，因此无需主动监控；等任务完成后审查即可。接下来了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云端代理][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、新功能，或将代码从一种框架迁移到另一种框架。

你了解了以下概念：

- 如何自定义代码生成环境；
- 如何确保操作安全；
- 明确限定议题范围的重要性；
- 如何将议题分配给 Copilot。

Copilot 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot 云端代理也支持 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云端代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保这些上下文可供它使用。提供特定上下文的方式有很多种，其中关键方法之一是使用[指令文件][instruction-files]，为代码生成提供指导。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 在当前指令生效的情况下生成筛选功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 再次运行相同提示，观察生成代码如何采用新标准；
- 提交指令更新和筛选功能切片到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循某些标准。Copilot 具有非确定性。本练习的重点是观察更新指令后行为变化的*趋势*，而不是逐字符匹配输出。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发实践指导和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件的用途。

使用指令文件可确保 Copilot 获得正确的信息，并按这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格与需求，从而给出更相关的建议和代码片段。可以指定编码约定、库以及希望包含的注释类型。指令既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的**每次**请求都会发送给 Copilot 的单个指令文件，应包含技术栈、项目概览、最佳实践等项目级信息。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的指令，可用于 TypeScript 或 Astro 等语言，也可用于创建 UI 组件或新增一组单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理生成代码时，都会使用存储库级指令文件以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目说明、结构和全局编码标准；
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指令；
- 使用自然语言，保持指导清晰，并说明代码应该和不应该如何编写。

指令文件没有唯一正确的写法，使用 AI 也一样。通过实验可以找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，可以看到其中涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的入口各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前学习路径会在相关课程中介绍相应入口。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中汇集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

本项目附带一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob，决定指令适用的文件。此处，匹配 `**/*.test.ts` 的 TypeScript 测试文件符合条件。
5. 查看其中为本项目创建单元测试提供的专门指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它链接到其他指令文件（如 `unit-tests.instructions.md`）以及项目中的现有文件。这样可以将大型指令集拆分为更小、可复用的文件，并为 Copilot 提供生成代码时的参考示例。（这些路径相对于指令文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异，相关步骤会特别说明。

为了观察自定义指令的影响，先在现有指令生效时发送提示。之后更新指令文件，再次发送同一提示并比较结果。

1. 关闭上一个练习中打开的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，使 Copilot 可以应用文件更改。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常涉及 helper 文件和测试。
6. 留意建议的 helper 是带类型的函数，第一个参数是 `db` 客户端，返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中的数据层约定（适用于 `src/lib/*.ts`）。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使未明确要求，也可能添加文档注释。如果发生这种情况也没关系；重点是更新指令后的一致性提升。

## 添加新的存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中已记录项目编码标准，但尚未加入代码文档规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务。像刚才添加的文档注释规则这样的项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空缓冲区。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新项目标准，并将其应用到实际代码中。下一练习会在此基础上继续开发。

## 提交指令并推送分支

指令文件与存储库中的其他资产一样，应使用版本控制进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端运行以下命令，确认指令更新和 helper 更改均已完成：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为 helper 基础实现所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也请暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档标准议题。Copilot 可以使用项目中已连接的 GitHub 工具来查找议题、创建关闭该议题的 PR 并合并。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具接入工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保代理选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 请求代表你执行 GitHub 操作时，检查操作并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 将其合并到 `main` 并在本地切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 Shell 命令时，检查命令并酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何从指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在*现有*指令下生成的结果；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 再次发送相同提示，观察建议代码如何采用新标准；
- 提交指令更新和 helper 基础实现到 `main`，供下一练习使用。

接下来在[代理模式][next-lesson]中使用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

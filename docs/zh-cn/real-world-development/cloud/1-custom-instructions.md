---
title: "练习 1 - 自定义指令（云端代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云端代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保这些上下文可供它使用。[指令文件][instruction-files]可提供这类指导，让 Copilot 不仅理解要做*什么*，也理解应该*如何*完成。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，这里不会运行前后对比提示。Copilot 云端代理会异步处理 GitHub 议题，因此难以实时并排演示影响。稍后审查云端代理创建的拉取请求时，就能看到指令文件的作用。

## 指令文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指导和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件的用途。

使用指令文件可确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求，从而给出更相关的建议和代码片段。你可以指定编码约定、库以及希望包含的注释类型。指令既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的**每次**请求都会发送给 Copilot 的单个指令文件。此文件应包含项目级信息，也就是大多数 Copilot 聊天或 CLI 请求都会用到的上下文，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的指令。可用于 TypeScript 或 Astro 等语言，也可用于创建 UI 组件或新增一组单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指令。
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

花些时间阅读本存储库附带的指令文件：一个核心 `copilot-instructions.md`，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还要留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob，指定指令适用的文件。此处，所有匹配 `**/*.test.ts` 等模式的 TypeScript 测试文件都符合条件。
5. 查看其中为本项目编写单元测试提供的专门指导。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将大型指令集拆分为较小、可复用的文件，并在生成代码时为 Copilot 提供参考示例。（这些路径相对于指令文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库级标准

接下来只需进行一处编辑：添加项目级规则，要求在代码中使用 TSDoc 文档注释，并在文件开头添加注释头。云端代理在本学习路径后续处理分配给它的议题时会读取这项规则。

编辑前，先创建一个工作分支（云端代理会读取议题目标分支中的指令，但仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中已记录项目编码标准，但尚无代码文档规则，这就是之前生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务。像刚才添加的文档注释规则这样的项目级标准，适合放在存储库级文件中。

## 提交、推送并合并指令更新

云端代理会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支，因此指令更改必须先合并到 `main`，云端代理才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，云端代理在下一练习处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目中的指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。在本学习路径后续练习中，可以观察云端代理创建的拉取请求是否遵循该标准。

接下来，将[第一个议题分配给 Copilot 云端代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

---
title: "练习 4 - 使用自定义指令引导 Copilot"
description: "浏览存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 获得相关指导。[指令文件][instruction-files]不仅说明希望生成*什么*代码，还规定代码应如何组织。现在筛选功能已完成，你将查看 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解存储库指令和按路径限定范围的指令文件如何提供给代理；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发实践指导和要求：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录用途、参数和返回值，包括存在时可注入的 `db` 参数；
- 可复用 Astro 组件应记录其 `Props` 契约，代码更改时也应更新注释；
- 应遵守现有格式和 lint 指导。

使用指令文件可确保 Copilot 获得正确的信息，并按这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格与需求，从而生成更相关的建议和代码片段。可以指定编码约定、库和注释类型；指令既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的**每次**请求都会发送给 Copilot 的单个指令文件，应包含大多数请求都会用到的项目级上下文。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及支持情况因环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

入门项目已包含一组指令文件。先查看现有内容，再进行更改并观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件适用于与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览，其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 指定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这有助于将大型指令集拆分为较小、可复用的文件，并为 Copilot 提供参考示例。

## 更新指令文件以符合团队指导

现有文件是很好的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 将以下内容作为最后一条项目添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令后，退出并恢复筛选对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认内容准确描述代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能。具体来说，你：

- 了解存储库指令和按路径限定范围的指令文件如何提供给代理；
- 更新指令文件以确保遵循编码标准；
- 观察指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/

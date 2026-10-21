---
title: "练习 1 - 自定义指令（云端代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云端代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些内容。[指令文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将：

- 探索如何通过存储库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会进行前后对比提示。Copilot 云端代理会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审阅云端代理创建的拉取请求时，可以看到指令文件产生的效果。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令用于向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求，从而生成更相关的建议和代码片段。你可以指定编码约定、库，以及希望在代码中采用的注释方式。指令可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。此文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 生成代码，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他规范的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。示例项目展示了一种有代表性的方法：

- 在 `copilot-instructions.md` 中集中记录项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指南。
- 使用自然语言，清晰说明要求，并举例展示代码应该和不应该是什么样子。

创建指令文件没有唯一的方法，使用 AI 也一样。通过实验可以找到适合项目的方案。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目的文件时，你会看到涵盖多种任务的指南，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时说明。
>
> 要查找模板或起点，可以浏览包含指令文件、自定义代理和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## 浏览项目中的自定义指令文件

花些时间阅读存储库自带的指令文件：其中包含一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件中的项目简述，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件开头的 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式。此处会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看本项目针对单元测试编写的具体要求。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分为较小且可复用的文件，并让 Copilot 参考现有示例。（这些路径相对于指令文件所在目录，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需编辑一次：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后本学习路径中的云端代理处理议题时，就会使用这条规则。

编辑前，先创建一个分支（云端代理会读取议题目标分支上的指令；此处仍按规范提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准有助于改进代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。此处记录了项目编码标准，但尚未涵盖代码文档，因此之前生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指南，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指南。存储库级文件适合放置项目范围的标准，例如刚才添加的文档注释规则。

## 提交、推送并合并指令更新

云端代理会读取议题目标分支上的指令。下一练习将从 `main` 分支为 Copilot 分配议题，因此必须先将指令更改合并到 `main`，让云端代理能够使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。下一练习中，云端代理会在处理议题时读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让工作坊更贴近真实项目中的处理方式。

## 总结与下一步

你已了解 Copilot 如何从本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条存储库级标准。接下来几课中，可以在云端代理生成的拉取请求里看到该标准的应用。

接下来，将[第一个议题分配给 Copilot 云端代理][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

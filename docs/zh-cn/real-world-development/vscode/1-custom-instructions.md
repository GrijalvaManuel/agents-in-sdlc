---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些内容。[指令文件][instruction-files]是提供代码生成指导的一种重要方式。

本练习将：

- 探索如何通过存储库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范。
- 在现有指令下生成筛选功能的第一个数据切片（出版商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能偏离你设定的部分标准。Copilot 的结果具有非确定性。本练习的重点是观察更新指令前后的行为趋势，而不是让输出逐字匹配。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确信息，并按照这些实践完成任务。

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

## 为更改创建分支

遵循最佳实践，为更改创建分支：

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型选择因练习而异；每个步骤都会在相关位置说明。

为了了解自定义指令的影响，先在当前指令文件下发送提示。之后更新指令，再次发送相同提示并比较结果。

1. 关闭之前练习打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，允许 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的出版商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有非确定性，因此即使没有明确要求，它也可能添加文档注释。这并无问题；本练习的重点是观察更新指令后行为是否更一致。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改进代码建议。

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

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将标准应用于实际代码。下一练习会在此基础上继续完成筛选功能。

## 提交指令并推送分支

指令文件和其他资源一样，都由源代码管理。现在提交并推送分支：

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。可以手动完成，也可以让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用时，VS Code 可能会提示登录 GitHub；按照提示授权即可。后续课程会介绍此连接如何使用模型上下文协议 (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保代理选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的出版商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示你允许它代表你执行 GitHub 操作，请审查命令并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 请求执行 GitHub 操作或 shell 命令，请先审查命令，再酌情选择 **Allow**。

你已使用 GitHub Copilot 创建并合并拉取请求。

## 总结与下一步

你了解了 Copilot 如何从指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在**现有**指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一课继续使用。

接下来，使用[智能体模式][next-lesson]添加跨代码库的功能。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

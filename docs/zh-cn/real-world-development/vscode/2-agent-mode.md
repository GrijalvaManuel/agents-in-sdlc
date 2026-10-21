---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是应用程序中最简单的更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新，工作会不断重复。

这正是 Copilot 智能体模式可以发挥作用的地方。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再完成必要的更新、运行测试等任务，并自动修复发现的问题。现在使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云端代理名称相似，但面向不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互；云端代理则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并代码之前，如何审阅 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表增长，需要允许用户按类别和出版商筛选。上一练习已添加出版商辅助函数，现在将使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先探索 Tailspin Toys 网站，了解现有功能。

该网站是面向棋盘游戏的众筹平台，采用开发者主题。用户可以浏览游戏及其详情。网站是单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回上一个练习使用的 codespace。此时应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示框，询问是否在浏览器中打开 `http://localhost:4321`，选择 **x** 关闭即可。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从 Codespace 终端选择 localhost URL 时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道；此时 Web 服务器正运行在 codespace 中。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、说明、出版商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型选择因练习而异；每个步骤都会在相关位置说明。

网站目前可以正常运行，但还需要添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建议题列表；可以让 GitHub Copilot 展示这些条目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中列出该模型，希望由此获得相对一致的体验。不过，由于大语言模型具有概率性，收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据它认为对用户最有用的内容为条目排定了优先级。

## 查看指令文件

让代理生成代码之前，先查看用于为 Copilot 提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用指令的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在目录，可用于复用内容、拆分大型指令集，或提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页，提供筛选 UI。

此外，合并之前必须运行并通过测试。Copilot 智能体模式可以完成这些任务：

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确保 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题提示 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并定位相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项工作，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此期间，编辑器中可能出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应按钮，因此工作尚未完成时无需提前选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至看到一些测试失败。这没有关系；Copilot 会反复生成代码并运行测试，直到完成任务且不再发现错误。

   ![Copilot 智能体模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审阅 Copilot 或其他 AI 工具生成的代码。

10. 返回运行中网站的浏览器，体验新功能。
11. 确认功能正常并审阅代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与下一步

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并代码之前，如何审阅 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索：实现分页

随着游戏列表增长，需要实现分页。使用本练习学到的技能，提示 Copilot 更新网站并实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 决定分页的实现方式，例如是否允许用户选择每页大小，或将其固定。
- 为 Copilot 提供足够的背景信息和指导。
- 可能需要迭代并补充上下文，要求 Copilot 修改实现；这是与 Copilot 协作的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才使用智能体模式构建了筛选功能。创建拉取请求之前，先确认它在浏览器中确实正常运行。无需亲自操作应用，可以连接 **Playwright MCP 服务器**并让 Copilot 操作真实浏览器来测试功能，然后发布分支并创建 PR。

本练习将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 在工作区中添加 Playwright MCP 服务器。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选更改创建拉取请求。

## 什么是模型上下文协议 (MCP)？

智能体模式可以通过连接编辑器以外的工具变得更强大。模型上下文协议 (MCP) 是 Copilot 与外部工具和服务通信的标准方式。

![展示智能体模式内部工作方式及其与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。AI 代理可以借助 MCP 实时访问外部工具和服务，获取最新信息（使用资源），并代表你执行操作（使用工具）。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 代理与外部工具、服务之间的桥梁，负责管理双方的通信，例如连接现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 代理可以创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化功能，让 AI 代理可以浏览网页、填写表单和选择按钮。

还有许多提供不同工具和资源的 MCP 服务器。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助用户发现和参与生态系统建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前请仔细检查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该已经配置了 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含 `github` 条目，因此上一练习中的 Copilot 可以读取议题列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时通过 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制浏览器，正好用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 通过运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志让 Playwright 在没有可见窗口的情况下运行浏览器；codespace 没有桌面环境，因此需要此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，可以让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器提供的工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中的 `playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请先检查配置，然后选择信任，以便启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步中 Copilot 首次使用其工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制真实浏览器。代理可以打开页面、导航、应用筛选并读取结果，然后向你总结观察情况，无需离开对话就能快速确认功能是否符合预期。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是截图。这意味着代理会依据结构化且带标签的元素（如按钮、链接和列表项）进行推理，方式与辅助技术类似。因此，快速的功能检查也能兼作简单的无障碍检查。

连接服务器并启动应用后，使用以下提示让 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步测试，然后报告观察结果。将其总结与议题中的验收标准对照；如果结果不符合预期，可以追问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请重新启动后再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求 (PR) 供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

创建 PR 的方式有多种，包括通过 github.com 和 GitHub 命令行界面 (CLI)。也可以让已连接 GitHub 工具的 Copilot 查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确保代理下拉菜单中选中了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示登录 GitHub；按照提示授权即可。
5. 观察 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 提供的链接查看拉取请求，但**暂时不要合并**。

## 总结与下一步

恭喜！本练习中你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 在工作区中添加了 Playwright MCP 服务器。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选更改创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义代理][next-lesson]，以便更高效地执行无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

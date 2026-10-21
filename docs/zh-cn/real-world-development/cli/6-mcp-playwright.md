---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所述，编写代码还涉及数据、外部服务，以及让 Copilot 使用其他自动化功能。MCP 服务器可以扩展 CLI 内置功能，为 Copilot 提供更多工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让代理控制浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新也需要实际与界面交互。你希望让 Copilot 像用户一样使用网站，进一步自动验证更改，提高对功能行为的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问最新信息，并代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁；每个服务器都代表一组可供代理使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求所需的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，帮助用户发现和参与生态系统建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前请检查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

前面已手动确认功能正常。现在使用 Copilot 可用的 Playwright MCP 服务器完成同样的检查：

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。不过，明确指出工具有助于它更一致地完成任务，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行操作，以确认功能正常。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并提供报告。

## 总结与下一步

恭喜，你已在 Copilot CLI 中使用 Playwright MCP 服务器通过真实浏览器探索功能。本课中你：

- 了解了模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加了 Playwright MCP 服务器。
- 让代理控制浏览器并探索筛选功能。

接下来[创建 QA 自定义代理][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

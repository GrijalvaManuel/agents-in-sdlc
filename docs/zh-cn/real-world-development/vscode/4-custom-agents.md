---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编码为可复用的代理，例如执行 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以放在全局目录 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description`，后面跟随定义代理行为、专业领域和指令的 Markdown 提示。

### 自定义代理与代理技能的区别

自定义代理与[代理技能][agent-skills-concept]有一些重叠之处。两者主要都通过 Markdown 文件定义，并向 AI 说明如何执行任务。最简单的区分方式是：**自定义代理负责工作，技能是工具**。

自定义代理有自己的上下文窗口，并负责协调技能（甚至其他代理）完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；处理过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。和其他 AI 功能一样，应在目标环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。为解决此无障碍问题，设计团队要求实现一种可开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。本练习将：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

已为你创建了一个无障碍自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，在新分支中开展无障碍工作，以便与练习 3 的筛选 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 定义的自定义代理。选择某个自定义代理后，聊天会话会采用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型选择因练习而异；每个步骤都会在相关位置说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认显示当前代理 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并修改项目文件。每项编辑都会以内联形式显示在聊天中，包括文件路径和差异供你审阅。
6. 随着编辑逐步产生，**Files changed** 指示器会更新，显示代理修改过的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 正在实际修改存储库，例如更新 Astro 组件、CSS 和相关测试。

下一练习将审阅并引导正在进行的工作。

## 总结与下一步

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队的专业知识和标准编码为可复用的代理，帮助 Copilot 更有效地完成工作。

本课介绍了以下概念：

- 自定义代理的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义代理。

接下来[监控并引导代理的工作][next-lesson]：审阅实时生成的更改，并在同一会话中添加浅色模式切换。

## 资源

- [自定义代理简介][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

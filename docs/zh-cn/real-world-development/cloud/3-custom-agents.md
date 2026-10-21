---
title: "练习 3 - 自定义代理"
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

本课将探索以下内容：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。为解决此无障碍问题，设计团队要求实现一种可开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。本课将使用自定义代理生成相关功能。

## 查看无障碍自定义代理

已为你创建了一个无障碍自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，查看无障碍代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

代理控制中心是管理环境中所有代理的中心位置。你可以将任务分配给 Copilot 云端代理、监控任务，甚至重定向并提供额外指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云端代理分配界面截图，突出显示自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结与下一步

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队的专业知识和标准编码为可复用的代理，帮助 Copilot 更有效地完成工作。

本课介绍了以下概念：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来可以在代理控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [自定义代理简介][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云端代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

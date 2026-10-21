---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审阅工作

在本实验中，你已与 GitHub Copilot 合作完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建相关游戏功能，并实现包括高对比度和浅色模式切换在内的无障碍功能。现在检查代码更改，并在必要时向 Copilot 提供反馈，改进其成果。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审阅生成的代码，并遵循正常的 DevOps 流程。接下来，在向团队开放审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的建议。

## 安全与 GitHub Copilot 云端代理

由于 Copilot 云端代理在无人监督的情况下异步执行任务，系统设置了一些安全限制：

- Copilot 对存储库只有读取权限，对代码仅拥有其工作分支的写入权限。
- 云端代理在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流都需要人工批准。
- 默认情况下，访问外部资源（包括 MCP 服务器）受到限制。

## 审阅生成的文档

先通过 GitHub.com 的标准 PR 界面查看 Copilot 云端代理创建的第一个拉取请求 (PR)，即为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云端代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会显示 **[WIP]** 标记。请等待 Copilot 完成；这可能需要几分钟，可以趁此休息或回顾已学内容。

4. 准备好后，选择 **Files changed** 选项卡并审阅更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，包括新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。浏览时，检查是否包含 TSDoc 注释、TypeScript 约定和文件注释头；这些要求来自练习 2 中查看的自定义指令文件。
6. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域排队运行。正常情况下，单体 Astro 应用的项目检查都会通过。等待可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。可以像与团队成员协作一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理要求。由于结果具有非确定性，无法提供固定的提示文本；可以要求 Copilot：

- 为每个代码文件添加简短的文件注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 在 PR 中添加评论并提及 **@copilot**，请求它修改生成的文档。可以采用上述任一建议，也可以提出其他希望写入代码库的文档要求。
2. 选择 **View Session** 观察 Copilot 工作。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审阅更新。

可以继续迭代，直到满意为止。完成后，将 PR 从草稿转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审阅相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域排队运行。正常情况下，单体 Astro 应用的项目检查都会通过。等待可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能。和之前一样，可以通过迭代要求云端代理进行更新，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 文档注释（注意，此议题是在更新自定义指令之前分配的）。在 **Conversation** 选项卡中添加评论即可提出要求，Copilot 会启动新会话。

## 审阅无障碍功能

最后，审阅由无障碍自定义代理实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在控制中心追加要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会显示 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审阅生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域排队运行。正常情况下，单体 Astro 应用的项目检查都会通过。等待可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常并在页面重新加载后仍然保留设置。

无障碍自定义代理帮助 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进机会，可以像处理之前的 PR 一样在评论中提及 **@copilot**，要求它更新。

## 可选练习：继续委派任务

云端代理最适合处理积压中的真实工作，让你可以转而处理其他事务。可以在存储库中再创建几个议题并分配给 Copilot，培养这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端代理学习路径。本系列课程中，你：

- **检查了存储库自带的自定义指令文件**，以便稍后了解它们对云端代理输出的影响。
- **将议题分配给 Copilot 云端代理**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**实现无障碍改进，添加高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为控制中心**，监控并引导正在运行的无障碍会话。
- **审阅并迭代改进云端代理的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云端代理学习路径已完成。其他学习路径会从不同角度补充所学内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**：在终端中使用计划和自动驾驶模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端代理桥接，交付两项经过审查的更改。

在自己的存储库中还可以尝试：

- 将重构或测试覆盖率议题分配给云端代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [Copilot 代理简介][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/

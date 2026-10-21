---
title: "练习 2 - GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云端代理能够自主完成更新代码和添加功能等任务。完成后，它会创建一个草稿拉取请求，供开发人员审查。这样可以将繁琐任务交给代理，让开发人员专注于更重要的工作并加快开发流程。

本课将探索如何使用云端代理：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受聘构建网站的承包商没有为代码添加文档；团队希望先为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望在每个游戏详情页显示同一类别中的其他游戏，让用户继续浏览。暂时不需要精致的实现，只需支持 UX 验收测试即可。此项工作目前受阻，不过还有优先级更高的议题。

这些任务很容易被推迟，适合交给 Copilot 云端代理异步处理。开发人员可以继续其他工作，稍后再审查代理的成果。

## 了解 GitHub Copilot 云端代理

[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式交办工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。可以在会话进行中或完成后查看；方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法，不能替代思考。即使看似简单的操作，深入分析后也可能包含复杂性，因此要谨慎确定云端代理任务的范围，并参考[云端代理最佳实践][cloud-agent-best-practices]。应将其视为 AI 编程搭档，分阶段开展工作，持续学习、试验和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

之前的练习介绍了自定义指令及其引导 Copilot 的方式。如果已完成相关练习，就会看到 `.github/copilot-instructions.md` 的作用；否则现在可以快速查看。将任务分配给云端代理之前，先只读浏览存储库中的指令文件，以便稍后判断它们产生的影响。

在 GitHub 网页界面或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些指令限定到 `**/*.test.ts` 文件。

下一节将把 `Code lacks documentation` 议题分配给云端代理。留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自指令文件。稍后审阅拉取请求时还会再次查看。

## 为 Copilot 云端代理设置开发环境

编写代码通常需要特定环境和设置脚本，以确保项目处于良好状态。将任务交给 Copilot 也一样，它会以类似软件工程师的方式完成工作。

云端代理使用 [GitHub Actions][github-actions] 运行环境。可以创建专用[设置工作流][setup-workflow]，并配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便获得所需的开发工具和依赖项。本实验已预先配置该工作流，确保 Copilot 可使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的任务。Copilot 开始处理拉取请求之前，GitHub Actions 会运行该任务。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以测试工作流是否成功，而无需等待 Copilot 启动它。

## 添加文档

大多数项目都存在过时或缺失的文档，这类技术债务会降低效率，并让维护代码或引入新成员变得困难。Copilot 擅长生成文档，因此很适合将此议题交给云端代理；它会在后台生成所需文档，之后可以审阅。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方法。

13. 查看 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并确定计划，然后编写代码。

这通常需要几分钟。云端代理的主要优势之一是异步工作，让我们可以专注于其他任务。接下来会利用这一点，再分配一个任务给云端代理，然后编写代码为应用添加功能。

## 为游戏详情页添加相关游戏

云端代理的一大优势是能够分担工作，让你可以专注于其他任务。为设计团队添加相关游戏区域可能不需要太久，但这段时间也可以用来处理其他工作。现在将此任务分配给云端代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中应出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

现在 Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此无需持续监控；完成后再审查即可。接下来将了解如何创建和使用自定义代理。

## 总结与下一步

本课介绍了 [GitHub Copilot 云端代理][copilot-agents]。你可以将议题分配给云端代理异步处理，用于偿还技术债务、创建功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确议题范围的重要性。
- 将议题分配给 Copilot。

云端代理正在后台工作，现在可以开始创建和使用自定义代理。[Copilot 云端代理也可以使用 MCP 服务器][cloud-agent-mcp]，并支持前几课中介绍的自定义指令。

## 资源

- [Copilot 云端代理简介][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

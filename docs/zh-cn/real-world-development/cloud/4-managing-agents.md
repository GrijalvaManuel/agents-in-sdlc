---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你已要求 Copilot 云端代理完成三项改善用户体验和添加功能的任务。云端代理虽然以异步和自主方式工作，但监控任务仍然很重要。

可以使用多种工具管理分配给云端代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求 (PR) 的代理任务、了解已执行的操作，还可以引导正在进行的会话。

本课将：

- 浏览代理页面，监控云端代理任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

将任务分配给代理实现高对比度模式后，团队认为也应添加浅色模式。既然已经要更新网站样式并添加切换功能，一并实现浅色模式也很合理。现在需要引导代理，确保它同时添加高对比度和浅色模式。

## 查看 Copilot 云端代理任务

查看当前分配给 Copilot 云端代理的任务状态：

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应该能看到分配给 Copilot 的任务，例如：
    - 为代码库更新文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和所采用的方法而异。

4. 同时查看右侧显示的 PR 面板，以便监控拉取请求和更改的文件。

## 引导云端代理

你已查看活动任务，现在要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和任务状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，等待其完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中突出显示“Steer active session while Copilot is working”对话框的云端代理任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将其加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，再将新请求纳入工作流程。

这通常需要几分钟，可以利用这段时间回顾目前学到的内容。

## 总结与下一步

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云端代理任务的中心位置。可以跟踪所有进行中和已完成的任务、审阅正在执行的工作，还可以重定向正在运行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用控制中心和代理页面监控云端代理任务。
- 重定向正在运行的会话，以请求额外功能。

Copilot 正在完成无障碍功能。接下来审阅[云端代理创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [自定义代理简介][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

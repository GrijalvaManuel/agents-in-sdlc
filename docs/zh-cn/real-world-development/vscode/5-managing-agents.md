---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主工作：探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审阅每项差异、接受或拒绝单独更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍代理的工作。
- 审阅并接受建议的文件编辑。
- 在会话进行期间引导代理，在高对比度模式之外添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行中，设计团队又提出了新的要求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，可以引导当前会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会以内联形式显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看每次工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 代理提出文件编辑后，差异会以内联形式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 还可以打开聊天顶部的 **Files changed** 指示器，查看此会话期间代理修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待代理完成高对比度模式。完成后会显示总结其更改的最终消息。

## 引导代理添加浅色模式

现在高对比度模式已经实现，可以在同一对话中添加浅色模式切换。由于继续使用同一个聊天会话，代理会保留之前工作的上下文，包括修改的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会检查先前所做的更改，并继续提出浅色模式的补充更改。
3. 像审查高对比度模式一样，逐项审阅新编辑，并对每项差异选择 **Keep** 或 **Undo**。
4. 如果代理的方向不符合预期，例如试图以不喜欢的方式重构切换 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止工作，再发送澄清说明。

> [!TIP]
> 引导正在运行的会话比重新开始更快。可以用后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式并入现有设置面板”）或扩展范围（“为两个切换项添加键盘快捷键”）。

## 审阅工作集

提交之前，快速检查代理修改的所有文件：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看更改文件列表。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并检查差异，确认自定义代理的无障碍模式已落实，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与下一步

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话：逐项审阅文件编辑、接受或拒绝更改，并通过后续提示在保留上下文的情况下扩展工作。

本课介绍了以下内容：

- 实时监控代理工作。
- 接受和拒绝单独的文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，将无障碍更改发布为拉取请求，并审阅本实验中本地自定义代理的成果。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

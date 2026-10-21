---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有流程，帮助以正确的方式构建正确的功能。本课将开始一系列遵循典型代理驱动开发流程的课程：从议题出发实现功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 整个功能工作流会使用同一对话和分支。实际项目中，通常会为不同文件类型创建不同分支或 PR；本工作坊会适当简化，以便专注于核心概念。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，还会更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录日益增长，访客需要按类别和出版商筛选游戏。待办议题描述了该功能，但组合类别等细节需要在编写代码前达成一致。使用 Plan 模式确定这些选择，再授权 Autopilot 实施有明确范围的更改。

## 背景

将 AI 编码代理引入开发流程不会改变基本原则，反而会让这些原则更重要。大多数开发者遵循的流程大致如下：

1. 从描述待办工作的已创建议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体细节因团队和组织而异，但大多数流程都与上述步骤类似。

坚持使用这一标准流程，可以确保 AI 生成的代码符合既定需求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与代理共同工作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先创建计划，在此模式下无法编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试和迭代，直到认为任务完成。

先使用 Plan 模式并审阅计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能之前，返回 Codespace 并确保存储库和终端已准备就绪：

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可以将 GitHub 议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划让你可以在交给 Copilot 实施前确定功能或任务的方案。复杂任务最好花些时间先做规划。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方式。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的部分内容不符合你的决定，要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审阅，现在开始实现。使用 Autopilot 让 Copilot 继续工作。

Autopilot 会让 Copilot 反复处理问题，直到认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相似的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何遵循计划、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实施，因此请先审阅计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并之前必须审查。接下来审查代码并运行网站，确认一切正常：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式，确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 根据议题和规划阶段的决定审查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并要求 Copilot 修复失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，通过 Codespaces 转发的端口打开网站：

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用后，选择 **Open in Browser**。
3. 测试类别筛选、出版商筛选以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace 并要求 Copilot 停止启动的开发服务器。

## 总结与下一步

你使用不同的对话模式构建并审查功能。本课中你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来[使用自定义指令][next-lesson]，确保代码遵循既定实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

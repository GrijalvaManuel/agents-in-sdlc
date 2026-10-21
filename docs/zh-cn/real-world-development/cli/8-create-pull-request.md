---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，再创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选实现、指令更新、技能更新、质量保证 (QA) 配置和测试都保存在同一分支上。现在一起审阅并创建拉取请求。本次工作坊中，你已手动合并星级评分 PR；接下来使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理拆分为几个 PR。为简化工作坊，这些筛选和质量工作都保留在同一对话和分支中，并合并到一个 PR。

本课将：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整功能更改和验证证据。
- 创建筛选 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你已使用 Copilot 规划、实现并验证功能。Tailspin Toys 希望自动处理 PR 剩余工作，同时让开发人员保留合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复持续集成 (CI) 检查失败、回应审查意见，以及在需要时执行变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

之前你都是手动选择 **Merge pull request**。Agent Merge 可以承担这项工作，但必须先审查更改并确认 PR 已准备就绪。

## 使用 Agent Merge 管理 PR

代码已经完成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程：

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似 `PR #14 was squash-merged successfully.` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或缺少的权限。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结与下一步

你已自动化开发流程的多个环节，包括代码生成、代码测试和验证，以及拉取请求流程。本课中你：

- 了解了 Agent Merge 及其如何自动化合并流程。
- 检查了完整功能更改和验证证据。
- 创建了筛选 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来探索用于上下文、模型、共享以及可选云端委派的 [Copilot CLI 斜杠命令][next-lesson]。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

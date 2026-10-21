---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他 CLI 工具一样，GitHub Copilot CLI 提供了许多用于交互的斜杠命令。这些命令公开了高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和向云端代理委派的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他一些能力：管理上下文、切换模型、共享会话，以及可选地向 [Copilot 云端代理][about-cloud-agent]委派任务。

## 探索 Copilot CLI 上下文

处理较大或复杂任务时，可能会遇到模型上下文窗口限制。Copilot CLI 会在需要时自动压缩对话，也可以通过斜杠命令查看或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和剩余空间各占多少上下文。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果原对话较短，差异可能不明显。

> [!NOTE]
> 上下文窗口即将用满时，Copilot CLI 会自动压缩上下文。想自行选择时间时使用 `/compact`。如果要开始无关的新任务并需要全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型：

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型及使用信息。
3. 保留当前模型、选择另一个模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验可以帮助大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 支持的选项。
2. 如果要共享会话，请选择合适的目标，例如 `/share file` 将其导出为本地 Markdown 文件。
3. 将导出内容发送给他人或发布之前，先检查内容。会话导出可能包含提示、回答和项目详细信息。

发布链接或 gist 是可选的。不要发布团队不希望公开的存储库或对话内容。

## 可选：扩展或委派工作

核心工作坊已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以把相互独立的子任务分给子代理并行运行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云端代理，后者会异步工作并可能创建拉取请求。

这些命令是可选项，因为它们可能创建额外工作树或远程工作。尝试前，先开始一个范围明确的新任务，并按照正常流程审查结果。如果想深入了解异步代理工作，可以继续学习[云端代理工作坊][cloud-workshop]。

## 总结与下一步

使用 Copilot CLI 的斜杠命令可以配置工具、共享会话并了解后台运行情况。本课中你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和云端代理委派的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来[回顾已学内容][next-lesson]并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [Copilot 云端代理简介][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

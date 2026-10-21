---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证已上线的 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 为 Space Quiz 提供公开访问入口。Copilot 可以准备工作流并引导部署，但你仍需负责审阅更改并确认网站已上线。

本课将：

- 要求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已经位于 GitHub 存储库中，但其他人还没有方便的方式体验它。通过 GitHub Pages 发布这个单文件网站，可以使用公开 URL 访问，而无需添加服务器或运行时依赖项。

## 要求 Copilot 部署网站

在 Copilot app 中 `space-quiz` 会话里发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；包含 Build website、Upload artifact 和 Deploy to Pages 步骤的成功 GitHub Actions Deploy website 运行；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求代理创建并合并拉取请求，以及更改存储库 Pages 设置。继续之前，先审阅差异、部署权限和所有 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认仅在存储库尚无 README 时添加 README。
2. 审阅拉取请求更改，包括工作流中的构建和部署步骤。
3. 仅在必需的拉取请求检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 正常加载。

如果代理无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。部署成功且网站可访问之前，不要将任务视为完成。

## 总结与下一步

Space Quiz 现在已作为在线网站发布，并且你通过实际验证确认部署成功，而不是只依赖代理报告。继续学习[课程 13：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/

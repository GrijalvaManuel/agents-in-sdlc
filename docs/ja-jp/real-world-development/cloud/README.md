---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot にクラウド上で非同期に作業させることができます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム命令を追加し、GitHub Issue を割り当てて実装を依頼します。カスタム エージェントを確認して作業方針を調整し、エージェント ダッシュボードからセッションを監視、指示します。最後に、作成されたプルリクエストをレビューし、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントを管理する][ex4] | 監視 | エージェント セッションを監視して指示する |
| [5. 反復する][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善し、次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのワークショップを受講できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Free 以外の Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織に対して有効にする必要があります。

> [!NOTE]
> このワークショップでは MCP を扱いません。Copilot で MCP サーバーを使う方法を学ぶには、[CLI ワークショップ](../cli/) または [VS Code ワークショップ](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/

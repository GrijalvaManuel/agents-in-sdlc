---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューして、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** に処理を任せます。

> [!NOTE]
> 通常は、機能、命令、スキル、QA エージェントをいくつかの PR に分けます。このワークショップでは手順を簡略化するため、フィルターと品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを理解する。
- 機能全体の変更と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認する。

## シナリオ

フィルター機能の作業では、Copilot を使って機能の計画、実装、検証を進めました。Tailspin Toys では、マージの承認は開発者が管理したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている内容 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase など) に取り組みます。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできますが、有効にする前に作業をレビューし、マージの準備ができていると判断してください。

## Agent Merge で PR を管理する

コードが完成したら、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの進行状況を確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可されていれば、レビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学んだ。
- 機能全体の変更と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ[ほかの Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## 参考情報

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

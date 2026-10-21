---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター操作を確認しました。次は、明確な QA プロセスを定義したカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次の内容を学びます。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルターの Issue に照らして調査結果をレビューする。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な設定です。プロファイルには、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、これらの機能をどう使うか Copilot に指示します。これらの機能を置き換えるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。QA で実施するチェックと従うべき制約の両方を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のように新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を正しく設定したことを確認します。エージェントが検証だけを求められているのに、機能を変更してしまうことを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定するとエージェントの表示名がわかりやすくなります。
3. プロファイルの命令を読み、QA が要件から確認を始め、リポジトリの命令に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を含めて結果を報告し、実装コードの変更前に確認を求め、コミットや PR 作成を行わないことを確認します。
5. 役割や制約が足りない場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue に基づいて QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルターの会話を再開して QA を選択します。そうすると、すでに会話に含まれている Issue と計画時の決定事項を使えます。

1. 次のプロンプトを使って QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、使用可能なエージェントの一覧にまだ表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 作業が終わったら、レポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA エージェントを選択し、フィルターの Issue に照らして調査結果をレビューした。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートのレビュー準備ができました。次は[機能の PR にまとめて Agent Merge を使います][next-lesson]。

## 参考情報

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

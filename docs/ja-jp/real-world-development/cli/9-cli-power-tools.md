---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同じく、GitHub Copilot CLI にも操作に使えるスラッシュ コマンドが多数あります。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使い、利用可能なモデルを確認する。
- `/share` でセッションをエクスポート、共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の基本的なワークフローは完了しました。次は、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って自分で確認、圧縮できます。

1. Codespace に戻ります。まだ起動していない場合は、リポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域にコンテキストがどのように分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、目立った変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替えて会話を新しくしたい場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野があり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用方法を向上できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで `/share` が使えることを確認します。
2. セッションを共有する場合は、ローカルの Markdown エクスポートに使う `/share file` など、用途に合った方法を選択します。
3. エクスポートした内容を誰かに送信したり公開したりする前に、必ず確認します。セッション エクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を並行化または委任する

このワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクを扱うための次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に作業してプルリクエストを開くことがある Copilot cloud agent にタスクを送信できます。

これらのコマンドは、新しい worktree やリモート作業を作成することがあるため、任意です。使う場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに学びたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI では、スラッシュ コマンドで構成を変更し、セッションを共有し、内部の処理を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` を使って利用可能なモデルを確認した。
- `/share` でセッションをエクスポート、共有する方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。最後に[学んだ内容と次のステップを振り返り][next-lesson]、このワークショップを締めくくります。

## 参考情報

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

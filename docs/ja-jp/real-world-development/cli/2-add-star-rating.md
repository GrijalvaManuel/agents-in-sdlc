---
title: "レッスン 2 - 星評価を追加する: 手早くできる改善"
description: "Copilot CLI でゲームカードを小さく変更し、転送したブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したら、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学びます。

- 機能ブランチで Copilot と会話する。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリを起動し、転送したブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys のゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリ、発行元、説明しか表示されません。まずは各カードに既存の評価を表示します。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は Copilot CLI でタスクに取り組む場です。Copilot app と異なり、通常の CLI 会話では専用の worktree を作らず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。会話を保存して後で戻ることができますが、ファイルとブランチは通常の Git 状態としてディスクに残ります。

会話には、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを送信して変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合はゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、コードは例と異なる場合があります。特定の実装と一致することではなく、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで正しく動くと判断するべきではありません。Copilot に Web サイトを起動させ、Codespaces から転送したブラウザーで更新後のユーザー インターフェイス (UI) を確認しましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI が PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ、**Merge pull request** を選択してマージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースできました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot との会話を開始した。
- ゲームカードへの小さな変更をエージェントに依頼した。
- `/diff` で変更をレビューした。
- アプリを起動し、転送したブラウザーで星評価を確認した。
- 最初のプルリクエストを作成してマージした。

次は[フィルターの Issue を起点に Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## 参考情報

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

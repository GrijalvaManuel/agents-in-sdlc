---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、Copilot cloud agent にユーザー エクスペリエンスの改善や機能追加を目的とした 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。このミッション コントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を与えたりできます。

このレッスンでは、次の内容を学びます。

- cloud agent のタスクを監視するために、エージェント ページを確認する。
- 実行中のセッションに指示を追加して、機能を拡張する。

## シナリオ

高コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するとよいと考えました。サイトのスタイルを更新して切り替え機能を追加する作業がすでに進行中だったため、同じ作業に含めるのが合理的です。高コントラストとライト モードの両方を追加するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインに表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が表示されます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てたタスクを完了するために、さまざまな手順を実行する。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、追加の監視を行えます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したので、高コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラスト モードを追加するセッションを選択します。Copilot が設定した名前や作業状況によって、タイトルは異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで、Copilot の作業中にアクティブなセッションへ指示するダイアログを表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

これまでと同じように、Copilot が更新されたタスクに取り組みます。メッセージを送った時点で実行中の手順が完了すると、新しい要求が作業に反映されます。

作業には数分かかるので、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、指示するための中心的な場所である Copilot agents ページを確認しました。このミッション コントロールを使うと、進行中と完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクの範囲を変更したりガイダンスを追加したりできます。

次の内容を学びました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視する。
- 進行中のセッションに指示を追加して機能を拡張する。

Copilot がアクセシビリティ機能を完成させる間に、[作成されたプルリクエストを反復的に改善する方法][next-lesson]を確認します。

## 参考情報

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

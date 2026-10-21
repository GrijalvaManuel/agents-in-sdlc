---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルター機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存の作業フローに沿って動作するため、適切な方法で適切なものを作成できます。このレッスンから複数回にわたり、Issue を起点に新機能を作成し、コードの妥当性と動作を確認して、プロジェクトに正常にマージする、一般的なエージェント駆動型開発の流れを進めます。

> [!NOTE]
> 機能ワークフローを続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心となる概念に集中できるよう簡略化します。

このレッスンでは、次の内容を学びます。

- GitHub Issue から Copilot との新しい会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送したブラウザーで機能を手動で検証する。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや発行元で絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリの組み合わせなどの詳細はコーディング前に合意しておく必要があります。Plan モードで方針を決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、ますます重要になります。多くの開発者は次のようなフローで作業します。

1. 実装する内容が記載された Issue を選ぶ。
2. 作成するものを計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れを基にしています。

この標準的な手順に従うと、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを通るようにできます。

## 会話モード

**会話モード**では、エージェントにどの程度の自律性を与えるかを設定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にあなたの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントが自律的にコードを作成し、テストを実行し、タスクが完了するまで繰り返し作業します。

まず Plan モードで計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、アプローチを決められます。複雑な作業では、事前に時間をかけて計画するのがお勧めです。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切なアプローチを判断して計画を作成します。

3. フィルターの動作について Copilot から質問されたら、希望する内容に基づいて回答します。選択に正解・不正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を読み、内容を確認します。
6. 決めた内容と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot で Copilot に作業を任せます。

Autopilot では、Copilot が問題を解決できたと判断するまで繰り返し作業します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下のモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、実装がすぐに始まる場合があります。先に計画をレビューしてください。依存関係がない、またはポートが競合していると報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

ほかのコードと同じように、生成されたコードもマージ前にレビューします。コードを確認し、サイトを実行して期待どおりに動くことを確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. Issue と計画時に決めた内容に照らし合わせて結果を確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなさそうでも、実際に動くことを確認しましょう。前回と同様にアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、発行元、組み合わせでフィルターを試します。
4. リセット時や結果がない場合の動作が、Issue と決めた内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot との新しい会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送したブラウザーで機能を手動で検証した。

次は[カスタム命令を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## 参考情報

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能、高コントラストとライト モードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを提供し、作業を改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従う必要があります。チームでレビューを有効にする前に、ドキュメント、関連ゲーム、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのは作業に使うブランチだけです。
- Cloud agent は GitHub Actions 内で動作し、作業用に一時的な独立環境を作ります。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成したコードのドキュメント追加 PR を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を開くと、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。MCP サーバーへの外部アクセスを含め、外部リソースへのアクセスは既定で制限されているため、これは想定された動作です。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエストの一覧を開きます。
3. **Add missing documentation**、または同様のタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は Copilot が完了するまで待ちます。数分かかることがあるので、その間に学んだ内容を振り返ってもよいでしょう。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく作成された TSDoc コメントなど、コードとドキュメントを確認します。具体的な変更内容は異なる場合があります。

    変更を確認するときは、TSDoc コメント、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム命令ファイルのガイダンスに基づいています。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されることを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するとき、こちらからコメントを返すこともできます。チーム メンバーと同じように、PR やコードのインライン コメントで Copilot にメンションしてください。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。たとえば次の変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をメンションして、生成されたドキュメントへの変更をコメントで依頼します。上記の例や、コードベースのドキュメントに関する別の要望を使えます。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、新しいコミットが PR に追加されます。
5. **Files changed** タブで変更を確認します。

満足するまで反復して改善できます。問題がなければ PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されることを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、cloud agent にコメントを追加して、反復的に変更を依頼できます。たとえば、表示する関連ゲームの数を変更したり、この Issue の割り当て後に追加した命令を適用してコメント ヘッダーや TSDoc コメントを追加したりできます。**Conversation** タブで新しいコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 でミッション コントロールから依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が完了するまで数分待ちます。

4. **Files changed** タブで生成されたコードをレビューします。
5. 実装を確認します。特に次の点を確認してください。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定の保存に使うローカル ストレージ。
   - 高コントラスト モードとライト モードの CSS やスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。
6. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されることを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了には数分かかることがあります。
10. **任意:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Copilot が作成したブランチ名に `<branch-name>` を置き換えて、Codespace のターミナルで次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーで高コントラストとライト モードを切り替え、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに従うよう Copilot を導きました。懸念点や改善点があれば、前の PR と同じようにコメントで **@copilot** をメンションし、変更を依頼できます。

## 任意の演習 — 委任を続ける

実際のバックログ項目を cloud agent に渡し、自分は別の作業に集中できると、より効果的に使えます。リポジトリにいくつか Issue を作成し、Copilot に割り当ててみましょう。次のような作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のワークショップを完了しました。これらのレッスンを通じて、次のことを行いました。

- リポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力への影響を把握した。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期処理を確認した。
- アクセシビリティ用のカスタム エージェントを作成して使い、高コントラストとライト モードの切り替えを追加した。
- Copilot agents ページをミッション コントロールとして使い、アクセシビリティ セッションを監視、指示した。
- cloud agent の PR をレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認した。

## レビューと次のステップ

Cloud agent のワークショップを完了しました。さらに学びたい場合は、ほかのワークショップでも今回の内容を発展させられます。

- 🖥️ **[VS Code ワークショップ](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ワークショップ](../../cli/)** — Plan と Autopilot、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野に対応したカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## 参考情報

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/

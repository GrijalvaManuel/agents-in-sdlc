---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が完了すると、人間がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発を加速し、より大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解消したい技術的負債があります。初期サイトを作成した請負業者が、ドキュメントを残していません。最初の課題として、アプリケーション内のエクスポートされたすべての関数に TSDoc コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリの別のタイトルを関連ゲームとして表示し、閲覧を続けられるようにします。まずは UX の受け入れテストに使える簡単な実装があれば十分です。この作業は現在ブロッカーですが、ほかに優先度の高い Issue があります。

これらは優先度が下がりやすい作業の例であり、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業する間、開発者はほかのタスクに集中し、後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]などの方法があります。割り当て後、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップしてタスクに取り組みます。処理中または完了後にセッションを確認でき、解決案がレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作でも、詳細を確認すると複雑さが見つかることがあります。[Copilot cloud agent のタスク範囲を決める際は注意し][cloud-agent-best-practices]、AI ペア プログラマーのように段階を踏んで学び、試し、調整してください。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と Copilot への適用方法を確認しました。すでに実際に試した場合は `.github/copilot-instructions.md` が使われることを確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリにある命令ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する要件を確認してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` ファイルが対象です。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメント ヘッダーが含まれているか確認できます。これらは命令ファイルに由来します。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を準備する

コードを作るときは、担当者に関わらず、特定の環境やセットアップ スクリプトが必要になることがあります。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。このラボでは、手順を円滑に進めるため、セットアップ済みのワークフローが含まれています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用ブラウザー、単一の Astro アプリで使うマイグレーション済み・シード済みの SQLite データベースを準備します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなどでセットアップ中にリポジトリをクローンする場合は contents: read が必要です。クローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドやテストで使うデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub ワークフローと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、正常に動作するかをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古いか、そもそも不足しています。この種の技術的負債は放置されがちで、生産性が下がり、コードベースの保守や新しいメンバーの参加も難しくなります。Copilot はドキュメント作成が得意なので、この作業を cloud agent に割り当ててバックグラウンドで進めてもらいましょう。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の文章を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が TODO リストを PR に表示するまで数分かかる場合があります。前述のワークフローを実行して環境を作り、プロジェクトを分析して、問題への適切な対処方法を決定しています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示の更新を確認するため、ブラウザーを更新する必要がある場合があります。

16. セッションをスクロールして、Copilot が問題を解決する方法を確認します。コードの調査と状態の理解、計画を考えるために一時停止する処理、コードの作成が行われます。

作業には数分かかります。Copilot cloud agent の主な目的は、非同期にタスクを実行して、ほかの作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能追加にも取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業に時間がかからないとしても、その時間をほかの作業に使えるなら、cloud agent に任せてみましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. 空のテンプレートを使うために **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく、Issue の最初のコメントに 👀 が表示されます。Copilot が作業を開始したことを示します。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、積極的に監視する必要はありません。完了した後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を確認しました。Issue を Copilot に割り当てると、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を学びました。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム命令にも対応しています。

## 参考情報

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更し、テストを実行して、問題をデバッグし、見落としたファイルを特定してさらに変更します。作業は何度も繰り返されます。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者のようにプロジェクトの構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode でサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> agent mode と cloud agent は名前が似ていますが、用途が異なります。Agent mode は IDE 内で作業するため、短いフィードバック サイクルと対話に適しています。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更やテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習で publisher ヘルパーを追加しました。今回は Copilot Agent Mode で残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧や詳細を表示します。単一の Astro アプリで、ページはビルド時に静的 HTML としてレンダリングされます。ページの frontmatter から Drizzle ORM を使ってローカル SQLite データベースに直接クエリするため、独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを実行するための `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後に `main` に戻っていることを確認して、Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くようダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーの URL `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明の一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選ぶと、タイトル、説明、発行元、カテゴリなどを表示します。

## Copilot とバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルで **New Chat** を選択して、新しい会話を開始します。モードやモデルの選択は各手順で説明します。

Web サイトは動作していますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したとき、Issue のバックログも自動的に作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは複数ファイルの変更を計画、実装し、ターミナル コマンドやツールを使います。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使い、コードの提案を比較的一貫させるために手順に記載しています。ただし LLM は確率的に動作するため、提案が説明と異なることがあります。これは想定された動作です。

> [!NOTE]
> Copilot が異なる MCP コマンドを使う場合もありますが、タスクを実行できることに変わりはありません。

3. 次のプロンプトを送信して、バックログの Issue を Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用と思われる項目を優先順位付けすることも確認できます。

## 命令ファイルを確認する

コードの生成を開始する前に、作業のコンテキストを Copilot に提供する命令ファイルを確認します。Web サイトに機能を追加する際のアプローチを説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル向け命令へのリンク。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所を基準とします。命令セットを小さな再利用可能なファイルに分け、例やテンプレートを示すのに役立ちます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルター処理を追加または調整する。
- フィルターの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功を確認する必要があります。Copilot Agent Mode に依頼して実装しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探します。データ レイヤー、UI、テストを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べる様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストなど複数のタスクを実行します。タスクの複雑さやコードベースによって数分かかることがあります。作業中にエディターに **Keep** と **Undo** が表示されることがあります。完了すると、各変更に対して **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したりテストが失敗したりすることがあります。Copilot はエラーがなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のチャット セッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。この演習では、次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装する。
- プロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行う。
- コードベースにマージする前に、生成された変更やテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 発展演習 – ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 既存の命令ファイルなど、ベスト プラクティスに従う。
- ユーザーがページ サイズを選べるようにするか、固定するかを考える。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更を依頼してコンテキストを提供しながら、何度か反復することがある。これは通常の作業フローです。

## 参考情報

- [Copilot の ask、edit、agent mode: 違いと使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

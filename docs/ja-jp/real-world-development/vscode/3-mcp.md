---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。プルリクエストを開く前に、実際にブラウザーで動作することを確認します。自分でアプリを操作する代わりに **Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてから、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを提供する仕組みを理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルター機能を手動テストさせる。
- ブランチを公開してフィルター機能のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にアクセスできると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準です。

![Agent mode、コンテキスト、LLM、ツール、および MCP サーバーや VS Code 拡張機能が提供するツールの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP により、エージェントはリソースを使って最新情報にアクセスし、ツールを使ってあなたに代わり操作を行えます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの間を仲介し、既存の API や npm パッケージなどのローカル ツールとの通信を管理します。各サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリの管理 API にアクセスし、リポジトリの作成や更新、Issue やプルリクエストの管理を行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化を行い、Web ページの移動、フォーム入力、ボタン操作などを実行します。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内の発見や貢献を促進するため、[MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できるサーバーだけを使い、機密性の高いリソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースに使用する MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot にブラウザーを操作させて機能をテストできます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` を追加します。次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、Codespace にはデスクトップ画面がないため、Playwright がブラウザーを表示せずに実行することを指定します。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストにも Playwright を使用しているため、ブラウザーは通常すでにインストールされています。後でブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行するよう依頼して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを使うとき、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成を確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、構成を確認して、起動を許可します。

手動での起動は任意です。ここで起動しない場合は、次の手順で Copilot がツールを使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で操作する代わりに、エージェントにページを開かせ、移動し、フィルターを適用させて、結果を読み上げてもらえます。会話を離れずに、機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素に基づいてエージェントが処理するため、機能確認とアクセシビリティの簡単な確認を同時に行えます。

サーバーに接続してアプリが起動している状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らしてレポートを確認してください。問題がある場合は、プルリクエストを開く前に質問を続けるか、コードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプト送信前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したら、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が行った変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、PR を作成する方法はいくつかあります。今回は GitHub Copilot に作成してもらいましょう。Copilot は関連する Issue を探し、それに関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** を選択します。
3. 次のように Copilot に PR を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得して操作するために、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは認証が必要な場合があります。
5. Copilot が Issue を検索して、適切なものを見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを提供する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- Playwright MCP サーバーを使って、リリース前にブラウザーでフィルター機能をテストした。
- ブランチを公開し、フィルター作業のプルリクエストを開いた。

MCP サーバーで機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどの特定タスクを効率化します。

## 参考情報

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

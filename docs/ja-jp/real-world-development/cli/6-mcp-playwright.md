---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要があります。そのために使うのが MCP サーバーです。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot にさらに多くのツールやサービスを提供できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) と Copilot CLI での使い方を理解する。
- Playwright MCP サーバーがまだ利用できない場合は追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot が作業中の Web サイトを使えるようにして、変更が期待どおりに動くかを確かめ、変更作業をさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新の情報にアクセスし、あなたに代わって操作を行えます。

ツールやリソースへのアクセスには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを使います。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が構成されている場合は、セットアップを省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力して、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類に **STDIO** を選択し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動くことを手動で確認しました。次は、Copilot で利用可能な Playwright MCP サーバーを使って、同じ確認を行います。

1. Copilot に Playwright MCP サーバーを使って機能を検証するよう、次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても、Copilot が自動的に選択します。使うサーバーがわかっている場合は、明示すると結果が安定し、トークンの節約にもつながります。

2. Copilot がブラウザーで機能を確認するために実行している手順を見ます。
3. レポートを読み、期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後に結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使って、Copilot CLI から実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学んだ。
- Playwright MCP サーバーが利用できない場合に追加した。
- エージェントにブラウザーを操作させ、フィルター機能を確認した。

次は[QA カスタム エージェントを作成し][next-lesson]、スキルとブラウザー ツールを組み合わせた専門的な役割を設定します。

## 参考情報

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

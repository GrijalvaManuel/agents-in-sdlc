---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意されたフィルターの Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースの調査、コードの生成、コマンドの実行、外部ツールとの連携をコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、集中して作業できます。まずツールをインストールしましょう。使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼して、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを通じてフィルターの Issue を見つける。

## シナリオ

チームは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に使うターミナルでその機能を利用できます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js がインストール済みなので、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントによる認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動すれば、準備は完了です。

## 基本を確認する

通常のシェル プロンプトでは、コマンドは Codespace 上で直接実行されます。Copilot CLI を起動すると、自然言語の入力はエージェントに送られ、スラッシュ コマンドで会話を操作できます。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプを閉じます。
3. 次の簡単な質問を Copilot に送り、正常に動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が答える前にリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のように依頼して、フィルターの Issue を見つけます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更が完了した後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使い、自動的に worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可されていないツールを使う前に確認します。このワークショップでは、`--yolo` を使って再起動します。ユーザーが承認したこのショートカットを使うと、Codespace 内の承認プロンプトが省かれ、演習に集中できます。

> [!CAUTION]
> `--yolo` は全自動の権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離環境でのみ使い、日常の開発用の既定値として alias に設定しないでください。詳細は[ツールの利用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` により、後のレッスンで Issue やプルリクエストを扱う際に使う読み書き可能な GitHub MCP ツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更をレビューしてください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトに関する簡単な質問をもう一度送り、会話が使えることを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりした後、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼して、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーを通じてフィルターの Issue を見つけた。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## 参考情報

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use

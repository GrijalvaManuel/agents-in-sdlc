---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。手動でコードを検証し、命令ファイルを使って標準に従う方法を学びました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**agent skill** が適しています。スキルを使うと、こうした操作を適切に実行する方法を Copilot に教えられます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

Agent skill には、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースをまとめます。基本的には、スキル名のフォルダーと `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、スキルが呼び出されたときに使うスクリプトやその他のリソースを格納するサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは既存のプロジェクト コマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリの `.github/skills` フォルダーか、ユーザーの `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングの判断に役立ちます。
3. 命令を読み、テストと lint の手順がどのように示されているかを確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。Copilot に 3 つのチェックを実行してもらいましょう。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが出力した情報がわかるレポートにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。変更後の動作を確認します。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックに使う `quality-checks` スキルを確認した。
- 結果のレポート形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更はフィルター機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティで公開されている次の例は参考情報です。このワークショップで追加作業は必要ありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing

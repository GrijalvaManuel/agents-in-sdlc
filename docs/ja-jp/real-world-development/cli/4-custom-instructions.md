---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。特定の方法で作業する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、作成するコードだけでなく、その構成方法も記述できます。フィルター機能を作成したので、Copilot が参照した命令を確認し、ドキュメント作成の標準を追加してコードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの命令と、パスを対象とする命令ファイルがエージェントに適用される仕組みを確認する。
- コーディング標準に従うよう、命令ファイルを更新する。
- 命令ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- コメントには、コードを言い換えるのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、`db` 引数を注入する場合も含め、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードを変更したときはコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に提供できます。規約、ライブラリ、コメントの方針などを指定して、より適切な提案やコード スニペットを促せます。リポジトリ全体、または特定のファイルやタスクを対象にできます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一のファイルです。多くのリクエストに共通するプロジェクトの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスク向けのガイドラインです。

> [!NOTE]
> ほかの命令形式やそのサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが用意されています。変更を加える前に内容を確認してみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて、Astro、Drizzle データ レイヤー、テストなどの命令を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、命令を適用するファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト内のファイルへのリンクを確認します。これにより、大きな命令セットを再利用可能な小さなファイルに分割し、Copilot に参考例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルはよい出発点ですが、改善の余地があります。新たに生成する TypeScript に TSDoc コメントを付けるため、`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを読み込むため、命令を保存した後でフィルターの会話を終了し、再開します。

1. フィルターの会話を開いたままの場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの命令と、パスを対象とする命令ファイルがエージェントに適用される仕組みを確認した。
- コーディング標準に従うよう、命令ファイルを更新した。
- 命令ファイルがコードに与える影響を確認した。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## 参考情報

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/

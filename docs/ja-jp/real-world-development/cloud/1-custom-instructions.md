---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」実行するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム命令とパスを対象にした命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に適用される方法を確認する。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加する。

> [!NOTE]
> VS Code や CLI のワークショップと異なり、ここでは変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue を非同期に処理するため、その影響をリアルタイムで並べて示すのは困難です。このワークショップの後半で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず単体テストを追加します。
- UI はダーク モードで、現代的な見た目にします。
- コードのドキュメントとして TSDoc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に提供し、より適切な提案やコード スニペットを促せます。好みの規約、ライブラリ、コメントの方針などを指定できます。リポジトリ全体、または特定のファイルやタスクを対象にできます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一のファイルです。技術スタック、プロジェクトの概要、ベスト プラクティス、全体的なガイダンスなど、多くのリクエストに共通する情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro など特定の言語や、UI コンポーネントや単体テストの作成など特定のタスク向けのガイドラインです。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の命令ファイルにも対応しています。[命令ファイルのサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` には、作成するもの、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記述します。
- `*.instructions.md` ファイルは、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに対する命令を記述します。
- 自然な言葉を使い、明確なガイダンスとコード例を示します。

命令ファイルや AI の使い方に唯一の正解はありません。試行を重ねて、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に命令ファイルの作成を手伝ってもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、使う環境によって方法が異なります。該当する手順は各ワークショップで説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタム エージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスも、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、リポジトリのルートを基準に命令の適用対象となる glob を指定します。ここでは `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に固有の命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にあるほかの命令ファイル (`unit-tests.instructions.md` など) や既存のプロジェクト ファイルへのリンクを確認します。命令を小さく再利用可能なファイルに分け、参考例を Copilot に示せます。リンクのパスはリポジトリのルートではなく、命令ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc コメントとファイル ヘッダーのルールを追加します。

## リポジトリ全体の標準を追加する

この演習で編集するのは、コードに TSDoc コメントとファイル単位のコメント ヘッダーを追加するという、プロジェクト全体のルールです。後の演習で Issue を割り当てると、cloud agent がこのルールを読み込みます。

まず、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから命令を読みますが、変更をきちんとコミットするためにブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、プロジェクト全体の情報を Copilot に提供します。コードの提案を改善するため、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。既存のコーディング標準には、コード内のドキュメント作成が含まれていないことを確認します。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc コメントを追加するよう Copilot に指示する次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) で全体に適用することも、特定の言語、ファイルの種類、タスク用の `*.instructions.md` として作成することもできます。今回追加したドキュメント作成ルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 命令の変更をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチから命令ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されるため、cloud agent に命令を読ませるには変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で割り当てる Issue に取り組む際、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップでは、`main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ここでは実際のプロジェクトの手順に近づけるため、ブランチを使います。

## まとめと次のステップ

プロジェクトで命令ファイルから Copilot にコンテキストが渡る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。このワークショップの後半で cloud agent が作成するプルリクエストをレビューするときに、この標準が使われているか確認できます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## 参考情報

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

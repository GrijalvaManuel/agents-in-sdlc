---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、コード生成の「何を」だけでなく「どのように」も Copilot に伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム命令とパスを対象にした命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に適用される方法を確認する。
- 最初のフィルター用データ スライス (発行元ヘルパー) を、現在の命令を使って生成する。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用される様子を確認する。
- 命令の更新とフィルターの実装を `main` にコミットし、次の演習で更新済みのガイダンスを使う。

> [!CAUTION]
> 生成コードは、設定した標準と異なる場合があります。Copilot の動作は確率的です。この演習では、出力の文字列を完全に一致させるのではなく、命令を更新した後の傾向の変化を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず単体テストを追加します。
- UI はダーク モードで、現代的な見た目にします。
- コードのドキュメントとして TSDoc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に提供できます。規約、ライブラリ、コメントの方針を指定すると、より関連性の高い提案やコード スニペットが得られます。リポジトリ全体または特定のファイルを対象にできます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのリクエストに共通する情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro など特定の言語、UI コンポーネントや単体テストの作成など特定のタスク向けの命令ファイルです。

> [!NOTE]
> IDE の命令ファイルは、Copilot Chat でのコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の命令ファイルにも対応しています。[カスタム命令のサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ レイヤーなど、ファイルの種類やタスクに特化した命令を記述します。
- 自然な言葉を使い、明確なガイダンスとコード例を示します。

命令ファイルの作成方法や AI の使い方に唯一の正解はありません。試行を重ねて、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] などのタスク用ファイルがあります。
>
> Copilot に命令ファイルを作成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境ごとに方法が異なります。
>
> テンプレートや出発点を探す場合は、命令ファイルやカスタム エージェントなどが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに付属する命令ファイル (共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイル) を読みます。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスも、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここでは `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に固有の命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある `unit-tests.instructions.md` などの命令ファイルやプロジェクト内ファイルへのリンクを確認します。リンクのパスはリポジトリのルートではなく、命令ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイル ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードやモデルの選択は演習によって異なるため、必要な手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。後で命令を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを残します。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを編集できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーやテストなどのコードを更新します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きの publisher 配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイル単位のコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加することがあります。その場合も問題ありません。この演習では、命令を更新した後に一貫性が改善することを確認します。

## リポジトリの新しい標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するため、コーディング標準にドキュメント作成ルールを追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近の **Code formatting requirements** セクションを探します。既存の標準には、コード内のドキュメント作成ルールがなく、そのために生成されたヘルパーにもコメントがなかったことを確認します。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc コメントを追加する次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) で全体に適用することも、特定の言語、ファイルの種類、タスク用の `*.instructions.md` として作成することもできます。今回追加したドキュメント作成ルールのような全体的な標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 変更前と同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習で使う実際のコードに反映できました。

## 命令と変更をコミットしてブランチをプッシュする

命令ファイルも、ほかのアセットと同じようにソース管理で扱います。命令の更新とヘルパーの変更をコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドで命令の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の更新とヘルパーの基盤に関するファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue に関連付けたプルリクエストを開きます。手動で操作することもできますが、GitHub のツールが接続済みなので、Copilot に Issue を検索させ、Issue をクローズする PR を作成してマージします。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub のツールが設定済みです。初回使用時に VS Code で GitHub へのサインインを求められる場合があります。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。命令の更新に加え、新しい publisher ヘルパーの基盤についても説明に含めるようにします。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索や操作を行う際に、必要に応じて **Continue** を選択します。GitHub ツールの初回使用時は認証を求められる場合があります。
5. Copilot が Issue を検索して PR を作成したことを確認します。
6. 次のプロンプトを使い、PR をマージしてローカルのブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行を Copilot に許可するよう求められたら、内容を確認してから **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot にコンテキストが渡る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の命令でコード生成プロンプトを送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、提案コードに新しい標準が適用されることを確認した。
- 命令の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにした。

次は、Copilot がこの命令を使って機能を追加する[agent mode][next-lesson]を確認します。

## 参考情報

- [GitHub Copilot のカスタマイズ用命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot

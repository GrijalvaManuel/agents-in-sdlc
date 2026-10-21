---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、特定の作業に役立つ命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供します。チームの知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディングに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` またはユーザーの `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、命令を定義する Markdown プロンプトが含まれます。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキルやほかのエージェントを調整します。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに照らしてサイトをレビュー、更新します。その際にプルリクエスト ワークフローやテストの実行、管理などのスキルを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同じく、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関わらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにすることを重視しています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分で、現在のダーク テーマが読みにくいという意見がありました。デザイン チームは、切り替え可能な高コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。この演習では、カスタム エージェントを使って機能を生成します。

この演習では、次の内容を学びます。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使って高コントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot の動作を導く内容を確認します。

まず Codespace に戻り、ターミナルを開いて、フィルター機能の PR と分けるため `main` から新しいブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読みます。アクセシブルな Web サイトのコード生成における主な責任、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題や間違いが説明されています。

## Copilot Chat でカスタム エージェントを使う

VS Code では `.github/agents` にあるカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの命令やツールを使うよう会話を設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 使用可能なエージェントから **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が適用されると、エージェントが変更したファイルの一覧を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなど、リポジトリの実際のファイルを変更しています。

次の演習では、進行中の作業をレビューして指示を追加します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。チームの知識や標準を再利用可能なエージェントにできます。

次の内容を学びました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して指示し][next-lesson]、変更を確認しながら同じセッションでライト モードの切り替えを追加します。

## 参考情報

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

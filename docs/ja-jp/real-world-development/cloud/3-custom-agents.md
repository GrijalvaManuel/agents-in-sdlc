---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義し、特定の作業をより効果的に行うための命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に提供します。チームの知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどがあります。

カスタム エージェントは、プロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、命令を定義する Markdown プロンプトが含まれます。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept]には論理的な重複があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに照らしてサイトをレビュー、更新します。その作業の中で、プルリクエスト ワークフローのスキルやテストを実行、管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同じく、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次の内容を学びます。

- カスタム エージェントを定義する方法。
- カスタム エージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みに関わらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることを重視しています。最近のユーザー フィードバックでは、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいという意見がありました。デザイン チームは、ユーザーが切り替えられる高コントラスト モードを追加するよう依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot を導く方法を理解するために、内容を確認しましょう。

Codespace に戻り、次の手順でアクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコードを生成する際の主な役割、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題や間違いが記載されています。

## Issue を作成して割り当てる

ミッション コントロールは、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、監視し、方向を変えたり追加のガイダンスを与えたりできます。高コントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![クラウド エージェントの割り当てで、カスタム エージェントと Accessibility が選択された画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、特定のタスクや分野に合わせて専門的な AI アシスタントを作成し、チームの知識や標準を再利用可能な形式で Copilot に提供できます。

次の概念を学びました。

- カスタム エージェントを定義する方法。
- カスタム エージェントにタスクを割り当てる方法。

Copilot が高コントラスト モードの実装を進めている間に、[ミッション コントロールでエージェント セッションを監視し、指示する方法][next-lesson]を確認します。

## 参考情報

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

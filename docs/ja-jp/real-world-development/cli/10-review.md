---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプルリクエスト、再利用可能なカスタマイズ、参考情報を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルターの Issue を見つけた。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルターの Issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認した。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認結果を評価した。
- フィルター機能全体をレビューし、フィルターの PR で Agent Merge を承認した。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認した。

## 作成したもの

このワークショップには、それぞれ最新の `main` から作成したブランチで進める、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、未評価であることを明示する状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー**: フィルターを実装し、命令を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルでは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では、その結果と要件やテスト範囲のレビューをまとめてから PR を作成しました。

追加したテストで実際の不足が解消されることが重要です。QA の実行結果として、新しいテストが不要な場合もあります。マージを承認する前に、コードと根拠をレビューしてください。変更を加えた場合は、影響する検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。命令は標準を示し、スキルは再利用可能なタスクを定義し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、変更内容とツールの結果そのものを確認してください。

タスクに合わせて**モードとモデル**を選びます。作業前に方針を考えるには **Plan**、範囲を絞った変更に取り組みながら進めるには **Interactive**、明確な範囲のタスクを任せるには **Autopilot** を使います。通常の編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールやインフラストラクチャと同じく、コンテキストも重要です。作りたいもの、その理由、作り方を明確に伝えると、出力が大きく変わります。

## さらに試す

中心的なワークフローを学びました。Copilot CLI の機能をさらに試すには、次のコマンドがあります。

- `/review` でコードレビュー エージェントに変更の分析を依頼する。
- `/rubber-duck` で問題について話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。本番コード、個人のプロジェクト、以前から作りたいと思っていた小さなアプリにも活用してください。チームと学びを共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルで Tailspin Toys を拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境も比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## 参考情報

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

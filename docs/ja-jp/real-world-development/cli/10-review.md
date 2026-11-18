---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。具体的には、次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター機能の Issue を見つけた。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初の pull request (PR) を手動でマージした。
- フィルター機能の Issue から開始し、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューした。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証を確認した。
- フィルター機能の変更全体をレビューし、Agent Merge による PR のマージを承認した。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認した。

## リリースした内容

ワークショップでは、更新済みの `main` から作成した別々のブランチで、2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルでは、単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト範囲のレビューを加え、PR 作成前の確認をまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認してください。変更があった場合は、影響を受ける検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の成果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し行う作業を定義し、カスタムエージェントは専門的な役割を担い、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

作業に合わせて **モードとモデル** を選択します。**Plan** は実装前に方針を検討するとき、**Interactive** は対象を絞った変更を対話しながら進めるとき、**Autopilot** は範囲が明確な作業を任せるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

基盤と同じように、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に伝えると、結果が大きく変わります。

## さらに試す

主なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらう。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別の作業を分離する。
- `/delegate` で Copilot cloud agent に作業を送る。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、長年作ってみたいと思っていた小さなアプリなどに活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

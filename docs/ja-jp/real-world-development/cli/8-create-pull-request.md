---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令の更新、スキルの更新、quality assurance (QA) プロファイル、テストは、すべて1つのブランチに保存されています。まとめてレビューし、pull request を作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、命令、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質確認の作業を1つの会話とブランチで進め、1つの PR にまとめています。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージのライフサイクルを自動化する方法を理解する。
- 機能全体の変更と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、PR 作業の残りを自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は pull request を取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合の rebase を行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして、マージの準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントの選択画面を開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可される手順に従ってレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テスト、検証に加えて、pull request の処理も自動化しました。このレッスンでは、次の内容を学びました。

- Agent Merge と、マージのライフサイクルを自動化する方法を理解した。
- 機能全体の変更と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は[Copilot CLI の便利なスラッシュコマンド][next-lesson]を確認し、コンテキスト、モデル、共有、任意のクラウドへの委任を学びます。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
